pytest参数化
======================================

pytest中使用@pytest.mark.parametrize进行参数话，可以来看一下下面的例子：

::

	# coding=utf8


	import pytest

	# 测试加减法
	class TestDemo:
	    # 加法    
	    @pytest.mark.parametrize("a, b, expected",
	    [(1,2,3), (2,3,5), (3,4,8)])    
	    def test_add(self, a, b, expected):
	        # 求和
	        sum = a + b        
	        # 断言
	        assert sum == expected    
	    
	    # 减法    
	    @pytest.mark.parametrize("a, b, expected",
	    [(1,2,-1), (8,3,5), (3,4,8)])    
	    def test_sub(self, a, b, expected):
	        # 减法
	        s = a - b        
	        
	        # 断言
	        assert s == expected