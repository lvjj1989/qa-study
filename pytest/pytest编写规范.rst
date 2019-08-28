pytest编写规范
=======================================

如何编写pytest测试样例
-------------------------------

需要按照下面的规则：

* 测试文件以test_开头（以_test结尾也可以）
* 测试类以Test开头，并且不能带有 __init__ 方法
* 测试函数以test_开头
* 断言使用基本的assert即可


如何执行pytest测试样例
----------------------------------

执行测试样例的方法很多种，上面第一个实例是直接执行py.test，第二个实例是传递了测试文件给py.test。其实py.test有好多种方法执行测试：

::


	py.test               # run all tests below current dir  
	py.test test_mod.py   # run tests in module  
	py.test somepath      # run all tests below somepath  
	py.test -k stringexpr # only run tests with names that match the  
	                      # the "string expression", e.g. "MyClass and not method"  
	                      # will select TestMyClass.test_something  
	                      # but not TestMyClass.test_method_simple  
	py.test test_mod.py::test_func # only run tests that match the "node ID",  
	                   # e.g "test_mod.py::test_func" will select  
	                               # only test_func in test_mod.py  

或者通过::
	python3 -m pytest ********
	python -m python ********


