pytest参数化
======================================

pytest中使用@pytest.mark.parametrize进行参数化，可以来看一下下面的例子：

::

	import pytest

	# 测试数据
	data = [
		{
			"num1": 1,
			"num2": 2,
			"res": 3
		},
		{
			"num1": 5,
			"num2": 10,
			"res": 15
		},
		{
			"num1": 0.1,
			"num2": 0.1,
			"res": 0.2
		},
		{
			"num1": 0.1,
			"num2": 0.2,
			"res": 0.3  # 这里为什么会是False可以网上看一下资料，和浮点转二进制后被截取有关系
		}
	]

	# 被测函数
	def fun_add(num_1, num_2):
		return num_1 + num_2

	# 测试类	
	class TestDemo:
		@pytest.mark.parametrize('data_add', data)
		def test_add(data_add):
			assert fun_add(data_add['num1'], data_add['num2']) == data_add['res']


	if __name__ == '__main__':
	    pytest.main()