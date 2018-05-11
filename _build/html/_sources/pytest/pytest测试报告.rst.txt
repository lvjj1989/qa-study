pytest测试报告
==================================

测试报告
-------------------------------
pytest可以方便的生成测试报告，即可以生成HTML的测试报告，也可以生成XML格式的测试报告用来与持续集成工具集成。

需要先按照pytest测试报告模块

::

	pip install pytest-html

**生成HTML格式报告：**

::

	py.test --resultlog=path  

或者：

::

	py.test  --html=result.html

**生成XML格式的报告：**

::
	py.test --junitxml=path  

或者：

::
	--junit-xml=result.xml
