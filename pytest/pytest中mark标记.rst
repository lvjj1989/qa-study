pytest中mark标记
============================================

指定用例测试执行实现冒烟测试
-------------------------------------

在测试过程中，可能出现业务场景，指定某个文件的某几条测试用例进行测试
这个时候我们就可以使用@pytest.mark.xxx(xxx可以自行定义)，比如以下代码：

::

	import pytest

	@pytest.mark.lvjj
	def test_1():
	    pass 

	@pytest.mark.lvjj
	def test_2():
	    pass

	def test_3():
	    pass


	if __name__ == "__main__":
	    pytest.main(["-s", "test_server.py", "-m=lvjj"])


如果我们只想只想@pytest.mark.lvjj中的测试用例，我们在只想命令函数的时候就使用::

	pytest -v -m lvjj

如果我们想只想除了@pytest.mark.lvjj以外的其他测试用例
::

	import pytest

	@pytest.mark.lvjj
	def test_1():
	    pass 

	@pytest.mark.lvjj
	def test_2():
	    pass

	def test_3():
	    pass


	if __name__ == "__main__":
	    pytest.main(["-s", "test_server.py", "-m=not lvjj"])

或者执行命令::

	pytest -v -m "not lvjj"

在这里我们通常会标记冒烟测试用例，如：@pytest.mark.smoke，这样就能实现pytest的冒烟测试了


跳过测试用例
----------------------------------------

编写中。。