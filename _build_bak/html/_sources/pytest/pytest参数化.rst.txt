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

::

    
    test_data = [
        {
            'case': '登入成功',
            'usr': 'admin',  # 正常登入
            'psw': '123456'
        },
        {
            'case': '账号不存在',
            'usr': 'admin1',  # 账号不存在
            'psw': '123456'
        },
        {
            'case': '密码错误',
            'usr': 'admin',  # 密码错误
            'psw': '12345'
        },
        {
            'case': '账号或密码为空',
            'usr': '',  # 账号或密码为空
            'psw': ''
        },
    ]


    @pytest.mark.parametrize('param', test_data, ids=[data.get('case') for data in test_data])  # ids需要传入一个列表，我们利用列表推导式
    def test_login(param):
        usr = param.get('usr')
        psw = param.get('psw')
        print(f'usr: {usr} , psw: {psw}')
        # 调用login接口，传入usr和psw，代码省略


    if __name__ == '__main__':  # 定义主函数
        pytest.main()  # 调用pytest

解决执行过程中的一下乱码，在conftest.py文件中加上下面这块代码：

::

    def pytest_collection_modifyitems(items):
    """
    修改用例名称中文乱码
    :param items:
    :return:
    """
    for item in items:
        item.name = item.name.encode('utf-8').decode('unicode_escape')
        item._nodeid = item.nodeid.encode('utf-8').decode('unicode_escape')


使用fixture实现参数化
----------------------------------------

fixture提供了这么一个机制，fixture装饰的函数拥有一个内置的对象request，同时fixture中还有一个params参数是用来传递参数化数据的，直接上代码：

::

    import pytest  # 导入pytest

    test_data = [
        {
            'case': '登入成功',
            'usr': 'admin',  # 正常登入
            'psw': '123456'
        },
        {
            'case': '账号不存在',
            'usr': 'admin1',  # 账号不存在
            'psw': '123456'
        },
        {
            'case': '密码错误',
            'usr': 'admin',  # 密码错误
            'psw': '12345'
        },
        {
            'case': '账号或密码为空',
            'usr': '',  # 账号或密码为空
            'psw': ''
        },
    ]


    @pytest.fixture(params=test_data)  # 给params传入参数化数据
    def param_data(request):
        return request.param  # 返回request对象中的param，这里存放的就是参数化数据


    def test_login(param_data): # 测试函数传入fixture
        usr = param_data.get('usr')
        psw = param_data.get('psw')
        print(f'usr: {usr} , psw: {psw}')
        # 调用login接口，传入usr和psw，代码省略


    if __name__ == '__main__':  # 定义主函数
        pytest.main()  # 调用pytest

fixture也给我们提供了ids的参数，用来传递用例名称，代码如下：

::

    
    @pytest.fixture(params=test_data, ids=[data.get('case') for data in test_data])  # 给params传入参数化数据,ids传入case名称列表
    def param_data(request):
        return request.param  # 返回request对象中的param，这里存放的就是参数化数据