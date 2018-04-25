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
* 方便的和持续集成工具集成

安装pytest
-------------------------------------------------

与安装其他的python类库无异，直接使用pip安装。

::

	pip install -U pytest  

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

从测试结果中可以看到，该测试共执行了两个测试样例，一个失败一个成功。同样，我们也看到失败样例的详细信息，和执行过程中的中间结果。

如何获取帮助信息
--------------------------

::

	py.test --version # shows where pytest was imported from  
	py.test --fixtures # show available builtin function arguments  
	py.test -h | --help # show help on command line and config file options  

最佳实践
----------------------------

其实对于测试而言，特别是在持续集成环境中，我们的所有测试最好是在虚拟环境中。这样不同的虚拟环境中的测试不会相互干扰的。
由于我们的实际工作中，在同一个Jekins中，运行了好多种不同项目册的测试，因此，各个测试项目运行在各自的虚拟环境中。

将pytest安装在虚拟环境中：

::

	virtualenv .        # create a virtualenv directory in the current directory  
	source bin/activate # on unix  

