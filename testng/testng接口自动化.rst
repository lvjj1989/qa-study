使用 TestNG 编写接口自动化测试
======================================

为什么用 TestNG 做接口自动化
-------------------------------

在 Java 生态下，接口自动化常见的组合有：

* **TestNG**：负责用例组织、分组、依赖、参数化、并发执行、报告等；
* **HTTP 调用库**：如 Apache HttpClient、OkHttp、RestAssured 等；
* **构建工具 / 持续集成**：如 Maven + Jenkins。

TestNG 本身不负责发 HTTP 请求，但非常适合作为“测试框架底座”，把“接口调用 + 断言 + 数据准备”这些步骤串在一起。

工程结构与依赖示例（Maven）
-------------------------------

典型接口自动化项目结构（简化版）::

    src
    ├── main
    │   └── java
    │       └── com.demo.api.client        # 封装接口调用的客户端类
    └── test
        └── java
            └── com.demo.api.test         # TestNG 测试类

建议在 ``pom.xml`` 中至少加入以下依赖（TestNG + RestAssured 示例）::

    <dependencies>
        <!-- TestNG -->
        <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>7.10.0</version>
            <scope>test</scope>
        </dependency>

        <!-- RestAssured，便于发送 HTTP 请求和做断言 -->
        <dependency>
            <groupId>io.rest-assured</groupId>
            <artifactId>rest-assured</artifactId>
            <version>5.5.0</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

封装一个简单的接口客户端
-------------------------------

以一个“获取用户信息”的 GET 接口为例::

    package com.demo.api.client;

    import io.restassured.response.Response;

    import static io.restassured.RestAssured.given;

    public class UserApiClient {

        private final String baseUrl;

        public UserApiClient(String baseUrl) {
            this.baseUrl = baseUrl;
        }

        public Response getUserById(String userId) {
            return given()
                    .baseUri(baseUrl)
                    .basePath("/api/users/{id}")
                    .pathParam("id", userId)
                    .when()
                    .get()
                    .then()
                    .extract()
                    .response();
        }
    }

使用 TestNG 编写基础接口用例
-------------------------------

在 ``src/test/java`` 下编写 TestNG 用例，通过断言状态码和响应体字段来判断是否通过::

    package com.demo.api.test;

    import com.demo.api.client.UserApiClient;
    import io.restassured.response.Response;
    import org.testng.Assert;
    import org.testng.annotations.BeforeClass;
    import org.testng.annotations.Test;

    public class UserApiTest {

        private UserApiClient userApiClient;

        @BeforeClass
        public void setUp() {
            // 可以从配置文件或系统变量中读取
            String baseUrl = "https://example.com";
            userApiClient = new UserApiClient(baseUrl);
        }

        @Test
        public void testGetUserSuccess() {
            Response response = userApiClient.getUserById("1");

            // 断言状态码
            Assert.assertEquals(response.statusCode(), 200, "状态码不是 200");

            // 断言响应体字段
            String userName = response.jsonPath().getString("name");
            Assert.assertNotNull(userName, "用户名不应为空");
        }
    }

配合 @DataProvider 做多数据驱动
-------------------------------

对同一个接口用多组入参进行验证时，可以结合 ``@DataProvider``::

    import org.testng.annotations.DataProvider;
    import org.testng.annotations.Test;

    public class UserApiDataDrivenTest {

        private final UserApiClient userApiClient = new UserApiClient("https://example.com");

        @DataProvider(name = "userIds")
        public Object[][] userIds() {
            return new Object[][]{
                    {"1"},    // 正常用户
                    {"2"}     // 另一个用户
            };
        }

        @Test(dataProvider = "userIds")
        public void testGetUserMulti(String userId) {
            Response response = userApiClient.getUserById(userId);
            Assert.assertEquals(response.statusCode(), 200);
        }
    }

通过 testng.xml 管理环境与分组
-------------------------------

可以在 ``testng.xml`` 中配置不同环境的 ``baseUrl``，以及只执行特定分组的接口用例::

    <!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd" >
    <suite name="ApiSuite">
        <parameter name="baseUrl" value="https://example.com"/>

        <test name="UserApiTests">
            <groups>
                <run>
                    <include name="smoke"/>
                </run>
            </groups>
            <classes>
                <class name="com.demo.api.test.UserApiGroupedTest"/>
            </classes>
        </test>
    </suite>

测试类中读取参数、使用分组::

    import io.restassured.response.Response;
    import org.testng.Assert;
    import org.testng.annotations.*;

    public class UserApiGroupedTest {

        private UserApiClient userApiClient;

        @Parameters("baseUrl")
        @BeforeClass
        public void setUp(String baseUrl) {
            userApiClient = new UserApiClient(baseUrl);
        }

        @Test(groups = "smoke")
        public void testGetUserSmoke() {
            Response response = userApiClient.getUserById("1");
            Assert.assertEquals(response.statusCode(), 200);
        }
    }

常见实践建议
-------------------------------

* **业务逻辑尽量封装在 client / service 层**，测试类只负责“调用 + 断言”；
* **统一处理公共前置 / 后置**：如鉴权、token 获取、清理数据等，放在 ``@BeforeClass`` / ``@AfterClass`` 中；
* **善用分组与参数化**：将冒烟、回归、全量用例分组，方便 CI 中按需执行；
* **与 Jenkins 结合**：通过 Maven 命令（如 ``mvn test -DsuiteFile=testng.xml``）在 Jenkins 中触发执行，并收集 TestNG 报告。

通过以上方式，就可以在 TestNG 目录下形成一套完整的“接口自动化测试方法”示例，便于后续团队成员参考和扩展。

