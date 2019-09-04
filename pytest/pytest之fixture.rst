pytest之fixture
=================================

fixture介绍
--------------------------------

 fixture是pytest特有的功能，它用pytest.fixture标识，定义在函数前面。在你编写测试函数的时候，你可以将此函数名称做为传入参数，pytest将会以依赖注入方式，将该函数的返回值作为测试函数的传入参数。 fixture有明确的名字，在其他函数，模块，类或整个工程调用它时会被激活。 fixture是基于模块来执行的，每个fixture的名字就可以触发一个fixture的函数，它自身也可以调用其他的fixture。 我们可以把fixture看做是资源，在你的测试用例执行之前需要去配置这些资源，执行完后需要去释放资源。比如module类型的fixture，适合于那些许多测试用例都只需要执行一次的操作。 fixture还提供了参数化功能，根据配置和不同组件来选择不同的参数。 fixture主要的目的是为了提供一种可靠和可重复性的手段去运行那些最基本的测试内容。比如在测试网站的功能时，每个测试用例都要登录和退出，利用fixture就可以只做一次，否则每个测试用例都要做这两步也是冗余。

 Fixture基础实例入门

 在函数声明之前加上“@pytest.fixture”。其他函数要来调用这个Fixture，只用把它当做一个输入的参数即可

::

	import pytest

	@pytest.fixture()
	def before():
	    print('\nbefore each test')

	def test_1(before):
	    print('test_1()')

	def test_2(before):
	    print('test_2()')
	    assert 0

下面是运行结果，test_1和test_2运行之前都调用了before，也就是before执行了两次。默认情况下，fixture是每个测试用例如果调用了该fixture就会执行一次的。

::

	collected 2 items 

	test_fixture_basic.py::test_1
	before each test
	test_1()
	PASSED
	test_fixture_basic.py::test_2
	before each test
	test_2()
	FAILED

	================================== FAILURES ===================================
	___________________________________ test_2 ____________________________________

	before = None

	    def test_2(before):
	        print('test_2()')
	>       assert 0
	E       assert 0

	test_fixture_basic.py:12: AssertionError
	===================== 1 failed, 1 passed in 0.23 seconds ======================


调用fixture的三种方式
----------------------------------------------

1. 在测试用例中直接调用它，例如第二部分的基础实例。
2. 用fixture decorator调用fixture

可以用以下三种不同的方式来写，我只变化了函数名字和类名字，内容没有变。第一种是每个函数前声明，第二种是封装在类里，类里的每个成员函数声明，第三种是封装在类里在前声明。在可以看到3中不同方式的运行结果都是一样。 test_fixture_decorator.py

::

	import pytest

	@pytest.fixture()
	def before():
	    print('\nbefore each test')

	@pytest.mark.usefixtures("before")
	def test_1():
	    print('test_1()')

	@pytest.mark.usefixtures("before")
	def test_2():
	    print('test_2()')

	class Test1:
	    @pytest.mark.usefixtures("before")
	    def test_3(self):
	        print('test_4()')

	    @pytest.mark.usefixtures("before")
	    def test_4(self):
	        print('test_4()')

	@pytest.mark.usefixtures("before")
	class Test2:
	    def test_5(self):
	        print('test_5()')

	    def test_6(self):
	        print('test_6()')

3. 用autos调用fixture

fixture decorator一个optional的参数是autouse, 默认设置为False。 当默认为False，就可以选择用上面两种方式来试用fixture。 当设置为True时，在一个session内的所有的test都会自动调用这个fixture。 权限大，责任也大，所以用该功能时也要谨慎小心。

::

	import time
	import pytest

	@pytest.fixture(scope="module", autouse=True)
	def mod_header(request):
	    print('\n-----------------')
	    print('module      : %s' % request.module.__name__)
	    print('-----------------')

	@pytest.fixture(scope="function", autouse=True)
	def func_header(request):
	    print('\n-----------------')
	    print('function    : %s' % request.function.__name__)
	    print('time        : %s' % time.asctime())
	    print('-----------------')

	def test_one():
	    print('in test_one()')

	def test_two():
	    print('in test_two()')

fixture scope
--------------------------------------------

function：每个test都运行，默认是function的scope class：每个class的所有test只运行一次 module：每个module的所有test只运行一次 session：每个session只运行一次

比如你的所有test都需要连接同一个数据库，那可以设置为module，只需要连接一次数据库，对于module内的所有test，这样可以极大的提高运行效率。


fixture 返回值
------------------------------------------------

在上面的例子中，fixture返回值都是默认None，我们可以选择让fixture返回我们需要的东西。如果你的fixture需要配置一些数据，读个文件，或者连接一个数据库，那么你可以让fixture返回这些数据或资源。

如何带参数 fixture还可以带参数，可以把参数赋值给params，默认是None。对于param里面的每个值，fixture都会去调用执行一次，就像执行for循环一样把params里的值遍历一次。 test_fixture_param.py

::

	import pytest

	@pytest.fixture(params=[1, 2, 3])
	def test_data(request):
	    return request.param

	def test_not_2(test_data):
	    print('test_data: %s' % test_data)
	    assert test_data != 2
