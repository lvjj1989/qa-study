pytest中mark标记
============================================

指定用例测试执行实现冒烟测试
-------------------------------------

在测试过程中，可能出现业务场景，指定某个文件的某几条测试用例进行测试
这个时候我们就可以使用@pytest.mark.xxx(xxx可以自行定义)，比如以下代码::

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

如果我们想只想除了@pytest.mark.lvjj以外的其他测试用例::

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

pytest自身内置了一些标记，如：skip、skipif、xfail。


skip
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

对于那些尚未开发完成的测试用例，最好的处理方式就是略过而不执行测试，这个时候，skip就可以发挥作用了::

	import pytest

	@pytest.mark.skip(reason="跳过原因")
	def test_1():
	    assert 1==2 

该条测试用例不会被真正执行，测试用例状态为：SKIPPED

所以，要跳过某个测试,只需要简单地在测试函数上方添加epytest.mark.skip()装饰器即可。

skipif
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

我们可以给跳过测试用例加上一些条件，当条件满足时，测试用例才会被掉过，这里就需要用到skipif了::

	import pytest

	A = 1


	@pytest.mark.skipif(A == 1, reason='测试skipif')
	def test_1():
	    assert 1 == 1


	@pytest.mark.skipif(A == 2, reason='测试skipif')
	def test_2():
	    assert 1 == 1


	if __name__ == '__main__':
	    pytest.main()

xfail
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

在使用pytest时，有些用例我们预计他可能会失败，这个时候就需要使用xfail了，当测试用例失败时，会被跳过，不会影响继续执行测试用例::

	import pytest


	@pytest.mark.xfail()
	def test_1():
	    assert 1 == 2


	@pytest.mark.xfail()
	def test_2():
	    assert 1 == 1


	if __name__ == '__main__':
	    pytest.main()




