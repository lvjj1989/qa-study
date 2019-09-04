pytest预期异常
=============================================



有时候我们在做测试的时候，预期就是抛出一个异常，但如果在正常情况下，抛出异常后pytest或停止该条测试用例的继续执行，所以我们需要一个期望抛出异常的方法，pytest.raises就可以做到这一点，代码如下：

::

	import pytest


	def test_add_raises():
		with pytest.raises(AssertionError):
			# 此处必须抛出AssertionError的异常，用例才会通过
			assert 1 + 1 == 3
