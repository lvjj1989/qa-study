Jmeter进行http接口测试
==========================================



**前言：**

本文主要针对http接口进行测试，使用Jmeter工具实现。
Jmter工具设计之初是用于做性能测试的，它在实现对各种接口的调用方面已经做的比较成熟，因此，本次直接使用Jmeter工具来完成对Http接口的测试。

开发接口测试案例的整体方案
------------------------------------------

* 第一步：我们要分析出测试需求，并拿到开发提供的接口说明文档；
* 第二步：从接口说明文档中整理出接口测试案例，里面要包括详细的入参和出参数据以及明确的格式和检查点。
* 第三步：和开发一起对接口测试案例进行评审。
* 第四步：结合开发库，准备接口测试案例中的入参数据和出参数据，并整理成csv格式的文件。
* 第五步：结合接口测试案例文档和csv格式的数据文档，做接口测试案例的自动化案例开发。


接口自动化适用场景
--------------------------------------

目前设计的自动化接口测试案例有两个运行场景：

1. 测试前置、开发自测：一个新的自动化接口测试案例开发完成后，直接发给接口对应的开发，安排在开发本地环境执行，一旦开发确认完成接口开发，就开始执行接口测试案例，基本上可以实时拿到测试结果，方便开发快速做出判断。【开发本地运行的方式就是打开JMeter工具，导入JMX文件，开始执行可。】
2. 回归测试：开发本地测试通过后，或整个需求手工测试通过后，把自动化的接口测试案例做分类整理，挑选出需要纳入到回归测试中的案例，在持续集成环境重新准备测试数据，并把案例纳入到持续集成的job中来，这些用于回归的接口测试案例需要配置到持续集成平台自动运行。

接口测试环境准备
---------------------------


Jdk1.6或以上：http://www.oracle.com/technetwork/java/javase/downloads/index.html

Jmeter，下载址址：http://jmeter.apache.org/download_jmeter.cgi

插件的下载安装地址：http://www.jmeter-plugins.org/

创建工程：
-------------------------------

1、打开Jmeter：下载好Jmeter后，双击bin目录下的jmeter.bat文件：

	.. figure:: /_static/jmeter/jmeter_1.jpg
	    :width: 20.0cm

2、添加线程组：在“测试计划”上点击鼠标右键-->添加-->threads(Users)-->线程组，添加测试场景设置组件，接口测试中一般设置为1个“线程数”，根据测试数据的个数设定“循环次数”。

	.. figure:: /_static/jmeter/jmeter_2.png
	    :width: 20.0cm

3、添加“HTTP Cookie管理器”：

	.. figure:: /_static/jmeter/jmeter_3.png
	    :width: 20.0cm

4、添加“Http请求默认值”组件，当被测系统有唯一的访问域名和端口时，这个组件很好用：

	.. figure:: /_static/jmeter/jmeter_4.png
	    :width: 20.0cm

5、在“HTTP 请求默认值”组件配置页面，填写被测系统的域名和端口，http请求的实现包版本以及具体协议类型，线程组里的所有“HTTP Sampler”可默认使用此设置。

	.. figure:: /_static/jmeter/jmeter_5.jpg
	    :width: 20.0cm

6、在“线程组”里添加“HTTP 请求”的Sampler

	.. figure:: /_static/jmeter/jmeter_6.png
	    :width: 20.0cm

7、在HTTP请求设置页面，录入被测接口的详细信息，包括请求路径，对应的请求方法，以及随请求一起发送的参数列表：

	.. figure:: /_static/jmeter/jmeter_7.jpg
	    :width: 20.0cm

8、设置检查点：在被测接口对应的“HTTP 请求”上，添加“响应断言”


	.. figure:: /_static/jmeter/jmeter_8.png
	    :width: 20.0cm

9、在设置页面上添加对相应结果的正则表达式存在性判断即可：

	.. figure:: /_static/jmeter/jmeter_9.jpg
	    :width: 20.0cm

10、添加监听器：方便查看运行后的结果

	.. figure:: /_static/jmeter/jmeter_10.png
	    :width: 20.0cm

**运行结果：**

	.. figure:: /_static/jmeter/jmeter_11.jpg
	    :width: 20.0cm



`参考出处 <http://www.cnblogs.com/puresoul/p/5092628.html>`_ 