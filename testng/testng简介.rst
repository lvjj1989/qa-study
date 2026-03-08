TestNG 简介与环境准备
======================================

简介
-------------------------------

TestNG 是一个受 JUnit 和 NUnit 启发而设计的测试框架，主要用于 Java 项目的自动化测试。它支持：

* 单元测试、集成测试、端到端测试等多种测试类型；
* 灵活的注解（如 ``@Test``、``@BeforeMethod``、``@AfterMethod`` 等）；
* 强大的分组测试、依赖测试、参数化测试和并发执行；
* 支持基于 XML 的测试套件配置和丰富的报告输出。

与 JUnit 相比，TestNG 在配置灵活性、依赖管理、分组和并发方面更加强大，非常适合做自动化回归、接口测试和 UI 测试的测试用例编排。

环境准备
-------------------------------

1. 安装 JDK
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* 建议使用 JDK 8 或以上版本；
* 安装完成后，配置好 ``JAVA_HOME`` 和 ``PATH`` 环境变量。

2. 构建工具集成（推荐 Maven）
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

如果使用 Maven 管理项目，在 ``pom.xml`` 中加入 TestNG 依赖（示例版本号可根据实际需要调整）::

    <dependency>
        <groupId>org.testng</groupId>
        <artifactId>testng</artifactId>
        <version>7.10.0</version>
        <scope>test</scope>
    </dependency>

如果使用 Gradle，可在 ``build.gradle`` 中加入::

    testImplementation 'org.testng:testng:7.10.0'

3. IDE 配置
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* IntelliJ IDEA：新建 Maven/Gradle 项目后，添加好依赖即可直接创建 TestNG 测试类；
* Eclipse：可通过安装 TestNG 插件或使用已有的 TestNG 支持来创建和运行测试。

第一个 TestNG 测试用例
-------------------------------

1. 创建测试类
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

在 ``src/test/java`` 目录下新建一个 Java 类，例如 ``CalculatorTest``::

    import org.testng.Assert;
    import org.testng.annotations.Test;

    public class CalculatorTest {

        @Test
        public void testAdd() {
            int a = 1;
            int b = 2;
            int result = a + b;
            Assert.assertEquals(result, 3, "加法结果不正确");
        }
    }

2. 运行方式
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* 在 IDE 中：右键测试类或方法，选择 “Run 'CalculatorTest' with TestNG”；
* 使用 Maven 命令行::

    mvn test

小结
-------------------------------

本篇主要解决三个问题：

* TestNG 是什么、适合做什么；
* 如何在工程中引入 TestNG 依赖；
* 如何编写并运行第一个简单的 TestNG 测试用例。

后续章节会在此基础上介绍注解、分组与依赖、参数化和并发执行等更高级用法。

