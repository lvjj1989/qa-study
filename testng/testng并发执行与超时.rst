TestNG 并发执行与超时控制
======================================

概述
-------------------------------

TestNG 提供了多种参数来控制用例的执行次数、并发度和超时时间，常用的有：

* ``invocationCount``：同一个用例重复执行多次；
* ``threadPoolSize``：为同一个用例设置线程池并发执行；
* ``timeOut``：为单个测试方法设置最大执行时间。

这些特性非常适合用于简单的性能验证或接口稳定性回归。

invocationCount：重复执行
-------------------------------

通过 ``invocationCount`` 可以让同一个测试方法执行多次::

    import org.testng.annotations.Test;

    public class InvocationExample {

        @Test(invocationCount = 5)
        public void repeatTest() {
            System.out.println("执行 repeatTest 一次");
        }
    }

上述示例中，``repeatTest`` 会连续执行 5 次，默认是单线程顺序执行。

threadPoolSize：并发执行
-------------------------------

与 ``invocationCount`` 配合使用时，通过 ``threadPoolSize`` 可以让同一个测试方法在多个线程中并发执行::

    public class ConcurrencyExample {

        @Test(invocationCount = 10, threadPoolSize = 5)
        public void concurrentTest() {
            System.out.println("线程: " + Thread.currentThread().getName());
        }
    }

说明：

* 这里总共执行 10 次，用 5 个线程并发执行；
* 常用于对某个接口或方法做简单的并发压测。

timeOut：方法级超时
-------------------------------

为单个测试方法设置最大执行时间（单位：毫秒）::

    public class TimeOutExample {

        @Test(timeOut = 1000)
        public void longRunning() throws InterruptedException {
            // 小于 1 秒则通过，大于 1 秒则失败
            Thread.sleep(500);
        }
    }

如果方法执行时间超过设置的 ``timeOut``，则该用例会被标记为失败。

综合示例：简单压测
-------------------------------

下面的示例演示了一个简单的“多线程 + 超时控制”的组合用法::

    import org.testng.annotations.Test;

    public class PerformanceExample {

        @Test(invocationCount = 10, threadPoolSize = 5, timeOut = 2000)
        public void stressApi() throws InterruptedException {
            // TODO: 这里可以调用实际的接口
            Thread.sleep(100);
        }
    }

说明：

* 总共执行 10 次，使用 5 个线程并发；
* 每次执行最长允许 2 秒，超过则视为失败。

注意事项
-------------------------------

* TestNG 的并发能力适合做 **轻量级的并发验证**，不建议直接替代专业性能测试工具（如 JMeter）；
* 并发测试时要注意被测接口是否具备幂等性、线程安全性等；
* 当并发度较高或逻辑较复杂时，建议配合专门的性能测试工具和监控系统使用。

小结
-------------------------------

本篇介绍了 TestNG 在并发执行与超时控制方面的基础能力，通过灵活组合 ``invocationCount``、``threadPoolSize`` 和 ``timeOut``，可以快速构建一些简单的稳定性或压力验证用例。

