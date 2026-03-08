TestNG 常用注解
======================================

注解总览
-------------------------------

TestNG 通过注解来控制测试执行的生命周期和顺序，常用注解包括：

* ``@Test``：标记一个方法为测试方法；
* ``@BeforeMethod`` / ``@AfterMethod``：在每个 ``@Test`` 方法前 / 后执行，常用于用例级别的初始化和清理；
* ``@BeforeClass`` / ``@AfterClass``：在当前类中所有 ``@Test`` 方法执行前 / 后执行；
* ``@BeforeSuite`` / ``@AfterSuite``：在整个测试套件开始前 / 结束后执行，常用于全局准备和收尾；
* ``@BeforeTest`` / ``@AfterTest``：在 ``testng.xml`` 中 ``<test>`` 标签对应的一组测试前 / 后执行。

生命周期示例
-------------------------------

下面的示例演示了常见生命周期注解的调用顺序::

    import org.testng.annotations.*;

    public class LifeCycleExample {

        @BeforeSuite
        public void beforeSuite() {
            System.out.println("BeforeSuite: 整个套件开始前执行一次");
        }

        @BeforeClass
        public void beforeClass() {
            System.out.println("BeforeClass: 当前类中所有测试方法前执行一次");
        }

        @BeforeMethod
        public void beforeMethod() {
            System.out.println("BeforeMethod: 每个测试方法前执行");
        }

        @Test
        public void testCase1() {
            System.out.println("执行用例 testCase1");
        }

        @Test
        public void testCase2() {
            System.out.println("执行用例 testCase2");
        }

        @AfterMethod
        public void afterMethod() {
            System.out.println("AfterMethod: 每个测试方法后执行");
        }

        @AfterClass
        public void afterClass() {
            System.out.println("AfterClass: 当前类中所有测试方法执行完后执行一次");
        }

        @AfterSuite
        public void afterSuite() {
            System.out.println("AfterSuite: 整个套件结束后执行一次");
        }
    }

``@Test`` 进阶属性
-------------------------------

1. priority（优先级）
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

通过 ``priority`` 可以控制同一个类中测试方法的大致执行顺序（数值越小优先级越高）::

    public class PriorityExample {

        @Test(priority = 1)
        public void testLogin() {
            System.out.println("先执行登录");
        }

        @Test(priority = 2)
        public void testOrder() {
            System.out.println("再执行下单");
        }
    }

2. enabled（是否启用）
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

通过 ``enabled`` 可以临时关闭某个测试方法，而不必删除代码::

    public class EnabledExample {

        @Test(enabled = false)
        public void testOldCase() {
            // 此用例暂时不执行
        }

        @Test
        public void testNewCase() {
            System.out.println("正常执行的新用例");
        }
    }

3. expectedExceptions（预期异常）
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

当某个用例预期会抛出异常时，可以使用 ``expectedExceptions`` 声明::

    public class ExceptionExample {

        @Test(expectedExceptions = ArithmeticException.class)
        public void divideByZero() {
            int x = 1 / 0;
        }
    }

如果没有抛出指定异常，则测试失败；抛出了指定异常则视为通过。

4. timeOut（方法级超时）
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

为单个测试方法设置最大执行时间（毫秒）::

    public class TimeOutExample {

        @Test(timeOut = 1000)
        public void longRunning() throws InterruptedException {
            Thread.sleep(500);  // 小于 1 秒，测试通过
        }
    }

小结
-------------------------------

本篇介绍了 TestNG 中最常用的生命周期注解和 ``@Test`` 的一些进阶属性。熟练掌握这些注解，有助于你根据业务需要精细控制测试用例的执行流程。

