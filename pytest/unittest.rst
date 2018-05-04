unittest
===============================================

unittest单元测试框架不仅可以适用于单元测试，还可以适用WEB自动化测试用例的开发与执行，该测试框架可组织执行测试用例，并且提供了丰富的断言方法，判断测试用例是否通过，最终生成测试结果。今天笔者就总结下如何使用unittest单元测试框架来进行WEB自动化测试。

**unittest.TestCase：**TestCase类，所有测试用例类继承的基本类。

**unittest.main():**使用她可以方便的将一个单元测试模块变为可直接运行的测试脚本，main()方法使用TestLoader类来搜索所有包含在该模块中以“test”命名开头的测试方法，并自动执行他们。执行方法的默认顺序是：根据ASCII码的顺序加载测试用例，数字与字母的顺序为：0-9，A-Z，a-z。所以以A开头的测试用例方法会优先执行，以a开头会后执行。

**unittest.TestSuite()：**unittest框架的TestSuite()类是用来创建测试套件的。

**unittest.TextTextRunner():**unittest框架的TextTextRunner()类，通过该类下面的run()方法来运行suite所组装的测试用例，入参为suite测试套件。

**unittest.defaultTestLoader():** defaultTestLoader()类，通过该类下面的discover()方法可自动更具测试目录start_dir匹配查找测试用例文件（test*.py），并将查找到的测试用例组装到测试套件，因此可以直接通过run()方法执行discover。用法如下：

**unittest.skip()**:装饰器，当运行用例时，有些用例可能不想执行等，可用装饰器暂时屏蔽该条测试用例。一种常见的用法就是比如说想调试某一个测试用例，想先屏蔽其他用例就可以用装饰器屏蔽。

**@unittest.skip(reason):** skip(reason)装饰器：无条件跳过装饰的测试，并说明跳过测试的原因。

**@unittest.skipIf(reason):** skipIf(condition,reason)装饰器：条件为真时，跳过装饰的测试，并说明跳过测试的原因。

**@unittest.skipUnless(reason):** skipUnless(condition,reason)装饰器：条件为假时，跳过装饰的测试，并说明跳过测试的原因。

**@unittest.expectedFailure():** expectedFailure()测试标记为失败。



废话不多说，直接看代码：

::

	# coding=utf-8
	#1.先设置编码，utf-8可支持中英文，如上，一般放在第一行

	#2.注释：包括记录创建时间，创建人，项目名称。
	'''
	Created on 2018-05-02
	@author: Lvjj
	Project:使用unittest框架编写测试用例思路
	'''
	#3.导入unittest模块
	import unittest

	#4.定义测试类，父类为unittest.TestCase。
	#可继承unittest.TestCase的方法，如setUp和tearDown方法，不过此方法可以在子类重写，覆盖父类方法。
	#可继承unittest.TestCase的各种断言方法。
	class Test(unittest.TestCase): 
	    
	#5.定义setUp()方法用于测试用例执行前的初始化工作。
	#注意，所有类中方法的入参为self，定义方法的变量也要“self.变量”
	#注意，输入的值为字符型的需要转为int型
	    def setUp(self):
	        self.number=raw_input('Enter a number:')
	        self.number=int(self.number)

	#6.定义测试用例，以“test_”开头命名的方法
	#注意，方法的入参为self
	#可使用unittest.TestCase类下面的各种断言方法用于对测试结果的判断
	#可定义多个测试用例
	#最重要的就是该部分
	    def test_case1(self):
	        print self.number
	        self.assertEqual(self.number,10,msg='Your input is not 10')
	        
	    def test_case2(self):
	        print self.number
	        self.assertEqual(self.number,20,msg='Your input is not 20')

	    @unittest.skip('暂时跳过用例3的测试')
	    def test_case3(self):
	        print self.number
	        self.assertEqual(self.number,30,msg='Your input is not 30')

	#7.定义tearDown()方法用于测试用例执行之后的善后工作。
	#注意，方法的入参为self
	    def tearDown(self):
	        print 'Test over'
	        
	#8如果直接运行该文件(__name__值为__main__),则执行以下语句，常用于测试脚本是否能够正常运行
	if __name__=='__main__':
	#8.1执行测试用例方案一如下：
	#unittest.main()方法会搜索该模块下所有以test开头的测试用例方法，并自动执行它们。
	#执行顺序是命名顺序：先执行test_case1，再执行test_case2
	    unittest.main()

	'''
	#8.2执行测试用例方案二如下：
	#8.2.1先构造测试集
	#8.2.1.1实例化测试套件
	    suite=unittest.TestSuite()
	#8.2.1.2将测试用例加载到测试套件中。
	#执行顺序是安装加载顺序：先执行test_case2，再执行test_case1
	    suite.addTest(Test('test_case2'))
	    suite.addTest(Test('test_case1'))
	#8.2.2执行测试用例
	#8.2.2.1实例化TextTestRunner类
	    runner=unittest.TextTestRunner()
	#8.2.2.2使用run()方法运行测试套件（即运行测试套件中的所有用例）
	    runner.run(suite)
	'''
	    
	'''
	#8.3执行测试用例方案三如下：
	#8.3.1构造测试集（简化了方案二中先要创建测试套件然后再依次加载测试用例）
	#执行顺序同方案一：执行顺序是命名顺序：先执行test_case1，再执行test_case2
	    test_dir = './'
	    discover = unittest.defaultTestLoader.discover(test_dir, pattern='test_*.py')
	#8.3.2执行测试用例
	#8.3.2.1实例化TextTestRunner类
	    runner=unittest.TextTestRunner()
	#8.3.2.2使用run()方法运行测试套件（即运行测试套件中的所有用例）
	    runner.run(discover)   
	'''


**使用方案一执行测试用例结果如下：**

Enter a number:10
10
Test over
Enter a number:.10
Fs

Ran 3 tests in 6.092s

FAILED (failures=1, skipped=1)
10
Test over

因为先执行test_case1,再执行test_case2,所以第一次输入10时，执行通过，返回. 第二次输入10时，执行不通过，返回F，最终一个用例通过，一个用例失败，还有一个用例是直接跳过的（装饰器）。

**使用方案二执行测试用例结果如下：**

Enter a number:10
10
Test over
Enter a number:F10
.

Ran 2 tests in 4.973s

FAILED (failures=1) 
10
Test over

因为先执行test_case2,再执行test_case1,所以第一次输入10时，执行不通过，返回F , 第二次输入10时，执行通过，返回. ，最终一个用例通过，一个用例失败。

**使用方案三执行测试用例结果如下（执行测试用例顺序同方案一）：**

Enter a number:10
10
Test over
Enter a number:.10
Fs

Ran 3 tests in 6.092s

FAILED (failures=1, skipped=1)
10
Test over

因为先执行test_case1,再执行test_case2,所以第一次输入10时，执行通过，返回. 第二次输入10时，执行不通过，返回F，最终一个用例通过，一个用例失败，还有一个用例是直接跳过的装饰器。


百度搜索测试用例Test Case：
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

	# coding=utf-8
	'''
	Created on 2018-05-02
	@author: Lvjj
	Project:登录百度测试用例
	'''
	from selenium import webdriver
	import unittest, time

	class BaiduTest(unittest.TestCase):
	    def setUp(self):
	        self.driver = webdriver.Firefox()
	        self.driver.implicitly_wait(30) #隐性等待时间为30秒
	        self.base_url = "https://www.baidu.com"
	    
	    def test_baidu(self):
	        driver = self.driver
	        driver.get(self.base_url + "/")
	        driver.find_element_by_id("kw").clear()
	        driver.find_element_by_id("kw").send_keys("unittest")
	        driver.find_element_by_id("su").click()
	        time.sleep(3)
	        title=driver.title
	        self.assertEqual(title, u"unittest_百度搜索") 

	    def tearDown(self):
	        self.driver.quit()

	if __name__ == "__main__":
	    unittest.main()


有道翻译测试用例Test Case：
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

	# coding=utf-8
	'''
	Created on 2018-05-02
	@author: Lvjj
	Project:使用有道翻译测试用例
	'''
	from selenium import webdriver
	import unittest, time

	class YoudaoTest(unittest.TestCase):
	    def setUp(self):
	        self.driver = webdriver.Firefox()
	        self.driver.implicitly_wait(30) #隐性等待时间为30秒
	        self.base_url = "http://www.youdao.com"
	    
	    def test_youdao(self):
	        driver = self.driver
	        driver.get(self.base_url + "/")
	        driver.find_element_by_id("translateContent").clear()
	        driver.find_element_by_id("translateContent").send_keys(u"你好")
	        driver.find_element_by_id("translateContent").submit()
	        time.sleep(3)
	        page_source=driver.page_source
	        self.assertIn( "hello",page_source) 

	    def tearDown(self):
	        self.driver.quit()

	if __name__ == "__main__":
	    unittest.main()


web测试用例：通过测试套件TestSuite来组装多个测试用例。
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

	# coding=utf-8
	'''
<<<<<<< HEAD
	Created on 2018-05-02
=======
	Created on 2018-05-05
>>>>>>> 907de13f66f27f08ead0ec823c7ef7bf652d7a33
	@author: Lvjj
	Project:编写Web测试用例
	'''
	import unittest
	from test_case import test_baidu
	from test_case import test_youdao

	#构造测试集
	suite = unittest.TestSuite()
	suite.addTest(test_baidu.BaiduTest('test_baidu'))
	suite.addTest(test_youdao.YoudaoTest('test_youdao'))

	if __name__=='__main__':
	    #执行测试
	    runner = unittest.TextTestRunner()
	    runner.run(suite)