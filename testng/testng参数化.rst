TestNG 参数化测试
======================================

概述
-------------------------------

TestNG 支持两种常用的参数化方式：

* 基于 ``@Parameters`` + ``testng.xml`` 的外部配置参数；
* 基于 ``@DataProvider`` 的多组数据驱动。

这两种方式可以单独使用，也可以结合使用，灵活构建不同的数据驱动测试场景。

@Parameters + testng.xml
-------------------------------

通过 ``@Parameters`` 可以从 ``testng.xml`` 中读取配置参数，并注入到测试方法中::

    import org.testng.annotations.Parameters;
    import org.testng.annotations.Test;

    public class ParamExample {

        @Test
        @Parameters({"username", "password"})
        public void login(String username, String password) {
            System.out.println("用户名: " + username + ", 密码: " + password);
        }
    }

对应的 ``testng.xml`` 示例::

    <!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd" >
    <suite name="ParamSuite">
        <test name="LoginTest">
            <parameter name="username" value="test_user"/>
            <parameter name="password" value="123456"/>
            <classes>
                <class name="ParamExample"/>
            </classes>
        </test>
    </suite>

特点：

* 适合做 **环境级** 或 **测试级** 的配置（如 baseUrl、环境标识、账号信息等）；
* 参数在 XML 中集中管理，修改无需改代码。

@DataProvider 多组数据驱动
-------------------------------

``@DataProvider`` 用于为同一个测试方法提供多组数据，每组数据执行一次测试::

    import org.testng.annotations.DataProvider;
    import org.testng.annotations.Test;

    public class DataProviderExample {

        @DataProvider(name = "loginData")
        public Object[][] loginData() {
            return new Object[][]{
                    {"user1", "pwd1"},
                    {"user2", "pwd2"}
            };
        }

        @Test(dataProvider = "loginData")
        public void login(String username, String password) {
            System.out.println("用户名: " + username + ", 密码: " + password);
        }
    }

进阶用法：数据动态生成
-------------------------------

``@DataProvider`` 方法中不仅可以返回静态数组，也可以根据数据库、配置文件、随机数等动态生成测试数据::

    @DataProvider(name = "randomNumbers")
    public Object[][] randomNumbers() {
        Object[][] data = new Object[3][1];
        for (int i = 0; i < 3; i++) {
            data[i][0] = (int) (Math.random() * 100);
        }
        return data;
    }

    @Test(dataProvider = "randomNumbers")
    public void testRandom(int number) {
        System.out.println("随机数: " + number);
    }

结合使用示例
-------------------------------

可以在 ``@DataProvider`` 中读取 ``@Parameters`` 提供的配置信息，再拼装多组测试数据，例如：

* 通过 ``@Parameters`` 传入环境标识 ``env``（dev/test/prod）；
* 在 ``@DataProvider`` 中根据 ``env`` 选择不同的数据源（如不同库或不同 CSV）。

小结
-------------------------------

* ``@Parameters`` 适合从 XML 读取少量配置型参数；
* ``@DataProvider`` 适合构建多组数据驱动测试；
* 结合两者可以实现灵活、可维护的数据驱动测试方案。

