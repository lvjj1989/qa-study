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


url测试报告输出
----------------------------
url格式的报告是将测试结果发送给pastebin服务器，在用例执行完成后，生成一个url地址

运行命令：
::

	pytest --pastebin=all #如果只想看失败的信息把all换成failed



allure测试报告
--------------------------
如果是在Jenkins中，推荐使用allure测试报告，会比较漂亮。同时allure也只是其他的自动化测试框架，包括testng

pytest中需要安装第三方类库

::

	pip install allure-pytest

详情可见以下地址:
https://docs.qameta.io/allure/#_pytest

https://blog.csdn.net/HuJinke_/article/details/70405885?locationNum=11&fps=1

推荐使用pytest单元测试框架+jenkins持续集成+allure报告，
教程见：https://www.jianshu.com/p/200601e444a8

这里暂时不做进一步展开了




