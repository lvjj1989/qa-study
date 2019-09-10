pytest介绍
======================================



pytest简介
-----------------------------------------------

pytest是python的一种单元测试框架，与python自带的unittest测试框架类似，但是比unittest框架使用起来更简洁，效率更高。根据pytest的官方网站介绍，它具有如下特点：

* 非常容易上手，入门简单，文档丰富，文档中有很多实例可以参考
* 能够支持简单的单元测试和复杂的功能测试
* 支持参数化
* 执行测试过程中可以将某些测试跳过，或者对某些预期失败的case标记成失败
* 支持重复执行失败的case
* 支持运行由nose, unittest编写的测试case
* 具有很多第三方插件，并且可以自定义扩展
* 方便持续集成

安装pytest
-------------------------------------------------

与安装其他的python类库无异，直接使用pip安装。

::

	pip install -U pytest  
	pip install -U pytest-html  
	pip install -U pytest-rerunfailures  



安装完成后，可以验证安装的版本：

::

	py.test --version  


一个实例
---------------------------------------------

我们可以通过下面的实例，看看使用py.test进行测试是多么简单。


::

	# content of test_sample.py  
	  
	def func(x):  
	    return x+1  
	  
	def test_func():  
	    assert func(3) == 5  


这里我们定义了一个被测试函数func，该函数将传递进来的参数加1后返回。我们还定义了一个测试函数test_func用来对func进行测试。test_func中我们使用基本的断言语句assert来对结果进行验证。
下面来运行这个测试：

::

	$ py.test  
	=========================== test session starts ============================  
	platform linux -- Python 3.4.1 -- py-1.4.27 -- pytest-2.7.1  
	rootdir: /tmp/doc-exec-101, inifile:  
	collected 1 items  
	test_sample.py F  
	================================= FAILURES =================================  
	_______________________________ test_answer ________________________________  
	def test_answer():  
	> assert func(3) == 5  
	E assert 4 == 5  
	E + where 4 = func(3)  
	test_sample.py:5: AssertionError  
	========================= 1 failed in 0.01 seconds =========================  

执行测试的时候，我们只需要在测试文件test_sample所在的目录下，运行py.test即可。pytest会在当前的目录下，寻找以test开头的文件（即测试文件），找到测试文件之后，进入到测试文件中寻找test_开头的测试函数并执行。

通过上面的测试输出，我们可以看到该测试过程中，一个收集到了一个测试函数，测试结果是失败的（标记为F），并且在FAILURES部分输出了详细的错误信息，帮助我们分析测试原因，我们可以看到"assert func(3) == 5"这条语句出错了，错误的原因是func(3)=4，然后我们断言func(3) 等于 5。

再一个实例
--------------------------------

当需要编写多个测试样例的时候，我们可以将其放到一个测试类当中，如：

::

	# content of test_class.py  
	  
	class TestClass:  
	    def test_one(self):  
	        x = "this"  
	        assert 'h' in x  
	  
	    def test_two(self):  
	        x = "hello"  
	        assert hasattr(x, 'check')  

我们可以通过执行测试文件的方法，执行上面的测试：

::

	$ py.test -q test_class.py  
	.F  
	================================= FAILURES =================================  
	____________________________ TestClass.test_two ____________________________  
	self = <test_class.TestClass object at 0x7fbf54cf5668>  
	def test_two(self):  
	x = "hello"  
	> assert hasattr(x, 'check')  
	E assert hasattr('hello', 'check')  
	test_class.py:8: AssertionError  
	1 failed, 1 passed in 0.01 seconds  

从测试结果中可以看到，该测试共执行了两个测试样例，一个失败一个成功。同样，我们也看到失败样例的详细信息，和执行过程中的中间结果。pytest中用点号(.)表示一条用例被执行并通过，用F来标识一条用例被执行并失败，如果想查看详情可以在py.test命令后面加上-v或者--verbose选项，具体效果可以自行尝试。

如何获取帮助信息
--------------------------

::

	py.test --version # shows where pytest was imported from  
	py.test --fixtures # show available builtin function arguments  
	py.test -h | --help # show help on command line and config file options 


使用raises可以帮助我们断言某些代码会引发某个异常
-------------------------------------

::

	# content of test_sysexit.py
	import pytest
	def f():
	    raise SystemExit(1)

	def test_mytest():
	    with pytest.raises(SystemExit):
	        f()


多个测试的类
------------------------------
当我们开发了多个测试时，可能会把它们分组到一个类中，我们现在可以使用pytest创建一个包含多个测试的类
::

	# content of test_class.py
	class TestClass(object):
	    def test_one(self):
	        x = "this"
	        assert 'h' in x

	    def test_two(self):
	        x = "hello"
	        assert hasattr(x, 'check')


pytest会发现所有test_命名的函数，没有必要继承任何东西，我们可以简单地通过传递它的文件名来运行测试：

::

	$ pytest -q test_class.py
	.F                                                                   [100%]
	================================= FAILURES =================================
	____________________________ TestClass.test_two ____________________________

	self = <test_class.TestClass object at 0xdeadbeef>

	    def test_two(self):
	        x = "hello"
	>       assert hasattr(x, 'check')
	E       AssertionError: assert False
	E        +  where False = hasattr('hello', 'check')

	test_class.py:8: AssertionError
	1 failed, 1 passed in 0.12 seconds

使用内置fixture
--------------------------------------
fixture是pytest中的一个特性，fixture可以请求任意资源，用文字不太好理解，我们就通过实例来理解吧。首先，通过以下命令可以找出所有pytest内置的fixture：

::

	$ pytest --fixtures

我们就以tmpdir这个内置的fixture来演示，tmpdir能返回一个唯一的临时目录路径，新建一个test_tmpdir.py文件，输入以下代码：

::

	def test_needsfiles(tmpdir):
	    print (tmpdir)
	    assert 0

在测试函数的参数中列出tmpdir，pytest将在执行测试函数之前查找并调用fixture工厂来创建资源：

::

	$ pytest -q test_tmpdir.py

在测试运行之前，pytest会创建一个唯一的，供每个测试调用的临时目录

::

	=================================== FAILURES ===================================
	_________________________ TestBuilding.test_needsfiles _________________________

	self = <test_suites.test_building.test_building_process.TestBuilding object at 0x105ed2ba8>
	tmpdir = local('/private/var/folders/08/lxy0ywy90mj9ck0rz1tq0y_r0000gn/T/pytest-of-lvjunjie/pytest-2/test_needsfiles0')

	    def test_needsfiles(self, tmpdir):
	        print(tmpdir)
	>       assert 0
	E       assert 0

	test_building_process.py:102: AssertionError
	----------------------------- Captured stdout call -----------------------------
	/private/var/folders/08/lxy0ywy90mj9ck0rz1tq0y_r0000gn/T/pytest-of-lvjunjie/pytest-2/test_needsfiles0
	=========================== 1 failed in 0.12 seconds ===========================
	Process finished with exit code 0

fixture的scope参数
----------------------------

scope参数有四种，'function','module','class','session'，默认为function。

* function：每个test都运行，默认是function的scope
* class：每个class的所有test只运行一次
* module：每个module的所有test只运行一次
* session：每个session只运行一次

setup和teardown操作

* setup，在测试函数或类之前执行，完成准备工作，例如数据库链接、测试数据、打开文件等
* teardown，在测试函数或类之后执行，完成收尾工作，例如断开数据库链接、回收内存资源等
* 备注：也可以通过在fixture函数中通过yield实现setup和teardown功能



最佳实践
----------------------------

其实对于测试而言，特别是在持续集成环境中，我们的所有测试最好是在虚拟环境中。这样不同的虚拟环境中的测试不会相互干扰的。
由于我们的实际工作中，在同一个Jenkins中，运行了好多种不同项目册的测试，因此，各个测试项目运行在各自的虚拟环境中。

将pytest安装在虚拟环境中：

::

	virtualenv .        # create a virtualenv directory in the current directory  
	source bin/activate # on unix  



