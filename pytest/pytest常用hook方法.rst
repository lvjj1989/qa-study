pytest常用hook方法
================================================

pytest_runtest_makereport
-----------------------------------------------


pytest_runtest_makereport这个钩子方法，了解用例的执行过程，并获取到每个用例的执行结果。
相关的源码，在``_pytest/runner.py``下::

	from _pytest import runner

	# 对应源码
	def pytest_runtest_makereport(item, call):
	    """ return a :py:class:`_pytest.runner.TestReport` object
	    for the given :py:class:`pytest.Item` and
	    :py:class:`_pytest.runner.CallInfo`.
	    """

这里item是测试用例，call是测试步骤，具体执行过程如下：

* 先执行call.when='setup' 返回setup 的执行结果
* 然后执行call.when='call' 返回call 的执行结果
* 最后执行call.when='teardown'返回teardown 的执行结果

**运行案例**
conftest.py写pytest_runtest_makereport内容::

	# conftest.py 
	import pytest


	@pytest.hookimpl(hookwrapper=True, tryfirst=True)
	def pytest_runtest_makereport(item, call):
	    print('------------------------------------')

	    # 获取钩子方法的调用结果
	    out = yield
	    print('用例执行结果', out)

	    # 3. 从钩子方法的调用结果中获取测试报告
	    report = out.get_result()

	    print('测试报告：%s' % report)
	    print('步骤：%s' % report.when)
	    print('nodeid：%s' % report.nodeid)
	    print('description:%s' % str(item.function.__doc__))
	    print(('运行结果: %s' % report.outcome))


pytest_collection_modifyitems
----------------------------------------------------------

pytest_collection_modifyitems是当测试用例收集完成后，可以改变测试用例集合(items)的顺序

conftest.py内容::

	def pytest_collection_modifyitems(session, items):
	    print(type(items))
	    print("收集到的测试用例:%s" % items)

	    # sort排序，根据用例名称item.name 排序
	    items.sort(key=lambda x: x.name)
	    print("排序后的用例：%s" % items)
	    for item in items:
	        print("用例名:%s" % item.name)


重新排序后就可以按用例的名称顺序执行了。


pytest_terminal_summary
---------------------------------------------
用例执行完成后，获取到执行的结果，快速统计用例的执行情况。
关于TerminalReporter类可以在_pytest.terminal中查看到

conftest.py中写个 pytest_terminal_summary 函数收集测试结果::

	def pytest_terminal_summary(terminalreporter, exitstatus, config):
	    '''收集测试结果'''
	    print(terminalreporter.stats)
	    print("total:", terminalreporter._numcollected)
	    print('passed:', len(terminalreporter.stats.get('passed', [])))
	    print('failed:', len(terminalreporter.stats.get('failed', [])))
	    print('error:', len(terminalreporter.stats.get('error', [])))
	    print('skipped:', len(terminalreporter.stats.get('skipped', [])))
	    # terminalreporter._sessionstarttime 会话开始时间
	    duration = time.time() - terminalreporter._sessionstarttime
	    print('total times:', duration, 'seconds')


pytest_report_teststatus
----------------------------------------------------
打印整个用例的测试结果
pytest_report_teststatus(report, config): 返回各个测试阶段的result, 

* when=='setup' 用例的前置操作
* when=='call' 用例的执行
* when=='teardown' 用例的后置操作

可以用when属性来区分不同阶段::

	def pytest_report_teststatus(report, config):
	    '''turn . into √，turn F into x'''
	    if report.when == 'call' and report.failed:
	        return (report.outcome, 'x', 'failed')
	    if report.when == 'call' and report.passed:
	        return (report.outcome, '√', 'passed')



钩子函数集合API
------------------------------------------
最后附上所有常用的钩子函数集合

setuptools
~~~~~~~~~~~~~~~~~~~~~~~~~~

引导挂钩要求足够早注册的插件(内部和setuptools插件)，可以使用的钩子

* pytest_load_initial_conftests(early_config,parser,args): 在命令行选项解析之前实现初始conftest文件的加载。
* pytest_cmdline_preparse(config,args): (不建议使用)在选项解析之前修改命令行参数。
* pytest_cmdline_parse(pluginmanager,args): 返回一个初始化的配置对象,解析指定的args。
* pytest_cmdline_main(config): 要求执行主命令行动作。默认实现将调用configure hooks和runtest_mainloop。

初始化挂钩
~~~~~~~~~~~~~~~~~~~~~~~~

初始化钩子需要插件和conftest.py文件
* pytest_addoption(parser): 注册argparse样式的选项和ini样式的配置值，这些值在测试运行开始时被调用一次。
* pytest_addhooks(pluginmanager): 在插件注册时调用，以允许通过调用来添加新的挂钩
* pytest_configure(config): 许插件和conftest文件执行初始配置。
* pytest_unconfigure(config): 在退出测试过程之前调用。
* pytest_sessionstart(session): 在Session创建对象之后，执行收集并进入运行测试循环之前调用。
* pytest_sessionfinish(session,exitstatus): 在整个测试运行完成后调用，就在将退出状态返回系统之前。
* pytest_plugin_registered(plugin,manager):一个新的pytest插件已注册。


collection 收集钩子
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* pytest_collection(session): 执行给定会话的收集协议。
* pytest_collect_directory(path, parent): 在遍历目录以获取集合文件之前调用。
* pytest_collect_file(path, parent) 为给定的路径创建一个收集器，如果不相关，则创建“无”。
* pytest_pycollect_makemodule(path: py._path.local.LocalPath, parent) 返回给定路径的模块收集器或无。
* pytest_pycollect_makeitem(collector: PyCollector, name: str, obj: object) 返回模块中Python对象的自定义项目/收集器，或者返回None。在第一个非无结果处停止
* pytest_generate_tests(metafunc: Metafunc) 生成(多个)对测试函数的参数化调用。
* pytest_make_parametrize_id(config: Config, val: object, argname: str) 返回val 将由@ pytest.mark.parametrize调用使用的给定用户友好的字符串表示形式，如果挂钩不知道，则返回None val。
* pytest_collection_modifyitems(session: Session, config: Config, items: List[Item]) 在执行收集后调用。可能会就地过滤或重新排序项目。
* pytest_collection_finish(session: Session) 在执行并修改收集后调用。

测试运行(runtest)钩子
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* pytest_runtestloop(session: Session) 执行主运行测试循环(收集完成后)。
* pytest_runtest_protocol(item: Item, nextitem: Optional[Item]) 对单个测试项目执行运行测试协议。
* pytest_runtest_logstart(nodeid: str, location: Tuple[str, Optional[int], str]) 在运行单个项目的运行测试协议开始时调用。
* pytest_runtest_logfinish(nodeid: str, location: Tuple[str, Optional[int], str])在为单个项目运行测试协议结束时调用。
* pytest_runtest_setup(item: Item) 调用以执行测试项目的设置阶段。
* pytest_runtest_call(item: Item) 调用以运行测试项目的测试(调用阶段)。
* pytest_runtest_teardown(item: Item, nextitem: Optional[Item]) 调用以执行测试项目的拆卸阶段。
* pytest_runtest_makereport(item: Item, call: CallInfo[None]) 被称为为_pytest.reports.TestReport测试项目的每个设置，调用和拆卸运行测试阶段创建一个。
* pytest_pyfunc_call(pyfuncitem: Function) 调用基础测试功能。


Reporting 报告钩子
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* pytest_collectstart(collector: Collector) 收集器开始收集。
* pytest_make_collect_report(collector: Collector) 执行collector.collect()并返回一个CollectReport。
* pytest_itemcollected(item: Item) 我们刚刚收集了一个测试项目。
* pytest_collectreport(report: CollectReport) 收集器完成收集。
* pytest_deselected(items: Sequence[Item]) 要求取消选择的测试项目，例如按关键字。
* pytest_report_header(config: Config, startdir: py._path.local.LocalPath) 返回要显示为标题信息的字符串或字符串列表，以进行终端报告。
* pytest_report_collectionfinish(config: Config, startdir: py._path.local.LocalPath, items: Sequence[Item]) 返回成功完成收集后将显示的字符串或字符串列表。
* pytest_report_teststatus(report: Union[CollectReport, TestReport], config: Config) 返回结果类别，简写形式和详细词以进行状态报告。
* pytest_terminal_summary(terminalreporter: TerminalReporter, exitstatus: ExitCode, config: Config) 在终端摘要报告中添加一个部分。
* pytest_fixture_post_finalizer(fixturedef: FixtureDef[Any], request: SubRequest) 在夹具拆除之后但在清除缓存之前调用，因此夹具结果fixturedef.cached_result仍然可用(不是 None)
* pytest_warning_captured(warning_message: warnings.WarningMessage, when: Literal[‘config’, ‘collect’, ‘runtest’], item: Optional[Item], location: Optional[Tuple[str, int, str]]) (已弃用)处理内部pytest警告插件捕获的警告。
* pytest_warning_recorded(warning_message: warnings.WarningMessage, when: Literal[‘config’, ‘collect’, ‘runtest’], nodeid: str, location: Optional[Tuple[str, int, str]]) 处理内部pytest警告插件捕获的警告。
* pytest_runtest_logreport(report: TestReport) 处理项目的_pytest.reports.TestReport每个设置，调用和拆卸运行测试阶段产生的结果。
* pytest_assertrepr_compare(config: Config, op: str, left: object, right: object) 返回失败断言表达式中的比较的说明。
* pytest_assertion_pass(item: Item, lineno: int, orig: str, expl: str) (实验性的)在断言通过时调用。

调试/相互作用钩
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
很少有可以用于特殊报告或与异常交互的挂钩：
* pytest_internalerror(excrepr: ExceptionRepr, excinfo: ExceptionInfo[BaseException]) 要求内部错误。返回True以禁止对将INTERNALERROR消息直接打印到sys.stderr的回退处理。
* pytest_keyboard_interrupt(excinfo: ExceptionInfo[Union[KeyboardInterrupt, Exit]]) 要求键盘中断。
* pytest_exception_interact(node: Union[Item, Collector], call: CallInfo[Any], report: Union[CollectReport, TestReport]) 在引发可能可以交互处理的异常时调用。
* pytest_enter_pdb(config: Config, pdb: pdb.Pdb) 调用了pdb.set_trace()。