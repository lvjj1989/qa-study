TestNG 分组与依赖测试
======================================

分组测试（groups）
-------------------------------

通过 ``groups`` 属性可以为用例打标签，在执行时按组来选择要运行的测试用例，例如把用例分为 smoke（冒烟）和 regression（回归）::

    import org.testng.annotations.Test;

    public class GroupExample {

        @Test(groups = "smoke")
        public void testLogin() {
            System.out.println("smoke: 登录用例");
        }

        @Test(groups = "regression")
        public void testOrder() {
            System.out.println("regression: 下单用例");
        }
    }

在 ``testng.xml`` 中只运行指定分组::

    <!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd" >
    <suite name="ExampleSuite">
        <test name="SmokeTests">
            <groups>
                <run>
                    <include name="smoke"/>
                </run>
            </groups>
            <classes>
                <class name="GroupExample"/>
            </classes>
        </test>
    </suite>

常见用法：

* 按 **模块** 划分：``login``、``order``、``pay`` 等；
* 按 **测试级别** 划分：``smoke``、``regression``、``performance`` 等；
* 按 **接口类型** 划分：``api``、``ui`` 等。

依赖测试（dependsOnMethods / dependsOnGroups）
------------------------------------------------

1. 基于方法的依赖
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

当一个测试依赖另一个测试成功时，可以使用 ``dependsOnMethods`` 指定依赖关系::

    import org.testng.annotations.Test;

    public class DependsExample {

        @Test
        public void initEnv() {
            System.out.println("初始化环境");
        }

        @Test(dependsOnMethods = "initEnv")
        public void runBusiness() {
            System.out.println("依赖 initEnv 成功后才执行");
        }
    }

特点：

* 如果 ``initEnv`` 执行失败或被跳过，则 ``runBusiness`` 会被标记为 **SKIP**；
* 适合“前置准备 + 业务执行”这类强依赖场景。

2. 基于分组的依赖
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

当一组用例依赖另一组用例时，可以使用 ``dependsOnGroups``::

    public class DependsOnGroupExample {

        @Test(groups = "init")
        public void initDb() {
            System.out.println("初始化数据库");
        }

        @Test(groups = "init")
        public void initCache() {
            System.out.println("初始化缓存");
        }

        @Test(dependsOnGroups = "init")
        public void runAllBusiness() {
            System.out.println("所有 init 组用例成功后才会执行");
        }
    }

注意事项
-------------------------------

* 避免构建过长的依赖链（A 依赖 B，B 依赖 C，C 又依赖 D...），否则排查问题会比较困难；
* 前置步骤建议尽量保持简单、稳定，否则容易导致大量后续用例被 SKIP；
* 对于“数据准备”这类工作，可以考虑用 ``@BeforeClass`` 或 ``@BeforeMethod``，而不是复杂的依赖关系。

小结
-------------------------------

分组测试可以方便地按标签选择要执行的用例，依赖测试则可以表达“先做什么，再做什么”的依赖关系。合理使用 groups 与 depends，有助于搭建清晰、可维护的大型自动化用例集。

