Jmeter断言方式总结
============================================

响应断言
---------------------------------------------

**判断返回内容中的内容作用对象：响应报文中的所有对象**


.. figure:: /_static/jmeter/j_1.png
    :width: 15.0cm

**APPly to:适用范围：**

Main sample and sub-samples:主要样本和次级样本，Main sample only：仅主要样本，Sub-samples only:仅次级样本，JMeter Variable:jmeter变量(输入框内可输入jmeter的变量范围)；

**要测试的响应字段：要检查的项：**

 响应文本：服务器响应文本，一般普通http响应，都勾选这个

Documeng(text)：测试文件，一切Apache Tika 支持服务器响应，包括文本响应，还支持 PDF, Office, Audio, Video，formats。jmeter会用Apache Tika

去解析服务器响应内容，会很耗内存，而且也很容易解析失败。所以一般普通http请求，不要选择这个。
URL样本：对样板的url进行断言。如果请求没有重定向（302），那么就是这个就是请求url。 如果有重定向，那么url就包含请求url 和 重定向url。

响应代码：http status的断言，如200，404，500等。

响应信息：http响应代码对应的响应信息，如：HTTP/1.1 200 Ok。

Response Headers:响应头部，如Content-Type: text/html。

Ignore status：忽略返回的响应报文状态码。

**模式匹配规则：**

包括：返回结果包括你指定的内容，支持正则匹配。

匹配： 　
(1) 相当于 equals 。当返回值固定时，可以返回值做断言，效果和equals相同；
(2) 正则匹配 。 用正则表达式匹配返回结果，但必须全部匹配。 即正则表达式必须能匹配整个返回值，而不是返回值的一部分。  

Equals：返回结果与指定断言完全一致。

Substring：返回结果包括你指定的内容，不支持正则匹配；否：不进行匹配。

要测试的模式:可以填写多个响应断言，通过按钮【添加】、【删除】是进行内容管理。

断言持续时间
------------------------------------------------

**用于判断服务器的响应时间**

.. figure:: /_static/jmeter/j_2.png
    :width: 15.0cm

APPly to:适用范围

Main sample and sub-samples:主要样本和次	级样本

Main sample only：仅主要样本

Sub-samples only：仅次级样本

断言持续时间：响应时间设置（单位：毫秒），如果响应时间大于设置的响应时间，则断言失败，否则成功！

备注：关于应用范围，我们大多数勾选“main sample only” 就足够了，因为我们一个请求，实质上只有一个请求。但是当我们发一个请求时，可以触发多个服务器请求，类似于ajax那种，那么就有main sample  和 sub-sample之分了。此外，对于有重定向的请求，并且勾选了“跟随重定向”， 那么这两个请求都是sub-sample，重定向后的请求（第二个请求）就是main-sample




Size断言
------------------------------------------------

.. figure:: /_static/jmeter/j_3.png
    :width: 15.0cm


用于判断返回内容的大小；
作用对象：返回信息，响应报文

APPly to:应用范围：
Main sample and sub-samples:主要样本和次级样本

Main sample only：仅主要样本

Sub-samples only:仅次级样本

JMeter Variable:jmeter变量(输入框内可输入jmeter的变量范围)

Response Size Field to Test:

响应字节的测试范围（可以选择用于判断的响应范围）

Full Response：全部响应；Response Headers:响应头部；Response 

Body：响应主体；响应代码：响应报文相关的代码；响应信息：响应报文的信息；

Size to Assert:断言字节范围

字节大小单位为：字节




XML断言
--------------------------------------------


.. figure:: /_static/jmeter/j_4.png
    :width: 15.0cm

XML(可扩展标记语言) 提供一种描述结构化数据的方法。与主要用于控制数据的显示和外观的 HTML 标记不同，XML 标记用于定义数据本身的结构和数据类型；

作用对象：判断返回结果是否和xml的格式即<></>成对出现




XML Schema 断言
------------------------------------------

.. figure:: /_static/jmeter/j_5.png
    :width: 15.0cm


亦可以称为XML模型断言/XML数据类型断言；XML Schema 定义了两种主要的数据类型：
1、xml document schema 文档架构 ;
2、文档架构xml-schema xml模式
作用对象：返回结果为XML概要断言的2中数据类型的消息

XML Schema：XML概要模型

File Name:文件名，写入需要断言的文件路径及名称，通常为.xsd文件，用于断言元素名称，类型和值，父子节点位置等信息


HTML 断言
---------------------------------------------

.. figure:: /_static/jmeter/j_6.png
    :width: 15.0cm

对响应类为XML类型的文件进行断言；
作用对象：针对sampler中的SOAP/XML-RPC Request而使用的断言

Tidy Settings:Tidy 环境（Tidy是一个HTML语法检查器和打印工具，可以将HTML转换为XML类型的文件）

Doctype：取值类型，默认取值: auto，此选项规定Tidy生成的DOCTYPE 声明. 设为 "omit" 输出不包含 DOCTYPE 声明；设为 "auto" 则依据内容作经验判断；设为 "strict", Tidy 设置 DOCTYPE 为严格(strict) DTD； 设为 "loose", DOCTYPE 设为 loose (transitional) DTD. 作为选择, 你可以给一个字符串作为FPI(the formal public identifier)。

Format：文件格式（可选择HTML/XHTML/XML三种不同类型的文件格式来检查返回内容）

Errors only：误差校正（能接受的最大值）

Error threshold：误差/错误范围（可选择误差/错误数量的范围，最大值）

Warning threshold：警告范围（可选择误差警告的数量范围，最大值）

如果勾选“Error only”这里忽略Warning，只对误差作统计检查；如果对返回内容的检查结果不超过指定结果，则断言通过，否则失败。

Write JTidy report to file:写入JTidy报告的文件（JTidy是Tidy的一个java移植，可以将它当成一个处理HTML文件的DOM解析器）




XPath断言
-------------------------------------------------

.. figure:: /_static/jmeter/j_7.png
    :width: 15.0cm

XPath即为XML路径语言，它是一种用来确定XML（标准通用标记语言的子集）文档中某部分位置的语言。XPath基于XML的树状结构，提供在数据结构树中找寻节点的能力。
作用对象：针对返回信息为XPAth的数据类型进行断言


Tidy Settings:Tidy 环境（Tidy是一个HTML语法检查器和打印工具，可以将HTML转换为XML类型的文件）

Doctype：取值类型，默认取值: auto，此选项规定Tidy生成的DOCTYPE 声明. 设为 "omit" 输出不包含 DOCTYPE 声明；设为 "auto" 则依据内容作经验判断；设为 "strict", Tidy 设置 DOCTYPE 为严格(strict) DTD； 设为 "loose", DOCTYPE 设为 loose (transitional) DTD. 作为选择, 你可以给一个字符串作为FPI(the formal public identifier)。

Format：文件格式（可选择HTML/XHTML/XML三种不同类型的文件格式来检查返回内容）

Errors only：误差校正（能接受的最大值）

Error threshold：误差/错误范围（可选择误差/错误数量的范围，最大值）

Warning threshold：警告范围（可选择误差警告的数量范围，最大值）

如果勾选“Error only”这里忽略Warning，只对误差作统计检查；如果对返回内容的检查结果不超过指定结果，则断言通过，否则失败。

Write JTidy report to file:写入JTidy报告的文件（JTidy是Tidy的一个java移植，可以将它当成一个处理HTML文件的DOM解析器）

XML Parsing Options：XML解析选项

Use Tidy(tolerant parser):使用Tidy（容错解析器），默认选择quiet（不显示）

Quiet：不显示

Report errors：错误报告

Show warnings:显示错误

Use Namespaces:使用名称空间

Validate XML:验证XML（文件包/数据）

Ignore Whitespace:忽略空格（这允许你指定语法分析器可以忽略哪个空格，而哪个空格是重要的）

Fetch external DTDs:获取外部DTDs（一些XML元素具有属性，属性包含应用程序使用的信息，属性仅在程序对元素进行读、写操作时，提供元素的额外信息，这时候需要在DTDs中声明）

XPath Assertion:输入框中写入xpath断言，点击Validate验证其正确性 True if nothing matches:确认都不匹配




MD5Hex断言
-------------------------------------------------

.. figure:: /_static/jmeter/j_8.png
    :width: 15.0cm

MD5是一种消息摘要算法，用以提供消息的完整性保护；
作用对象：针对参数类型为MD5Hex加密的参数的断言

MD5Hex：将已被MD5加密的参数写入其中，对接口响应的body做MD5Hex的校验


BeanShell断言
-----------------------------------------------------

.. figure:: /_static/jmeter/j_9.png
    :width: 15.0cm

作为脚本语言，能够方便的调用java类。

Reset bsh.interpreter before each call:在每次调用Bean Shell之前重置bsh.interpreter类（bsh.interpreter是Bean Shell脚本语言的一种类，也可以理解为一种解析器）

Parameters（String Parameters and String []bsh.args）:String参数（String []bsh.args是主类main函数的形式参数,是一个String 对象数组，可以用来获取命令行用户输入进去的参数）

Script file：脚本文件（可以填入脚本文件路径）

Script（see below for variables that are defined）:参照下文定义的变量（使脚本文件参照定义的变量来运行）

log对象：写日志；SampleResult对象：可以从中获取响应对象，响应码等信息；Response对象：获取响应数据，只读；

Failure：用例判断成功与否，Boolean类型，true代表失败；FailureMessage：失败信息；ResponseCode：响应码；

ResponseMessage：响应信息；ResponseHeader：响应头信息；RequestHeader：请求头信息；SampleLabel：取样器Lable信息；

SampleData：发送给服务器的数据；Ctx：(JmeterContext)：Jmrter上下文信息，从中可以获取到线程数，线程号等信息；

Vars(JmeterVariables)，获取Jmeter中定义的变量，或者设置变量；

Props：(JmeterProperties)，获取JMeter中的属性，或者设置属性。

举个例子，我们可以通过以下代码，当进行接口的响应断言，并且接口响应失败时打印或者在报告中输出错误的响应::

    String response = "";
    String Str = "\"code\":0"; //判断在响应中包含的内容
    response = prev.getResponseDataAsString();
    if (response == ""){
        Failure = true;
        FailureMessage = "系统无响应";//会输出在报告中
        System.out.print( FailureMessage);//将结果打印到控制台，若失败率较高建议不要打印
    }
    else if((response.contains(Str)) == false){
        Failure = true;
        FailureMessage = "接口响应异常，接口实际响应内容为：" + response;//会输出在报告中
        System.out.print(FailureMessage);//将结果打印到控制台，若失败率较高建议不要打印
    }





BSF断言
------------------------------------------------

.. figure:: /_static/jmeter/j_10.png
    :width: 15.0cm

BSF(Bean Scripting Framework)之前也介绍过，是一个支持在Java应用程序内调用脚本语言 (Script)，并且支持脚本语言直接访问Java对象和方法的一个开源项目；
作用对象：针对sampler中的BSF sampler而使用的断言

Script language（e.g.beanshell,javascirpt,jexl）:脚本语言（可以从下面的下拉框中选择对应的脚本语言JavaScript、beanshell等）
parameters to be passed to script（=> String Parameters and String []args）:（传递给脚本的参数→可以理解为使用BSF断言脚本时候一起引用的参数 ）
Script file（overrides script）：重写脚本（可以通过选择脚本文件的状态，是浏览调用已有的脚本还是在在下方的输入框内写入脚本；）
Script：下面的输入框表示可以输入变量类型，运用的脚本（取样结果、断言结果、取样日志文件等参数）



比较断言（Compare  Assertion）
--------------------------------------------------

.. figure:: /_static/jmeter/j_11.png
    :width: 15.0cm

这是一种比较特殊的断言元件，针对断言进行字符串替换时使用；
作用对象：需要替换的字符串

Select Comparison Operators:选择比较运算符

Compare Content:可以选择比较的内容类型（true/false或者自定义，编辑）

Compare Time：比较时间（可以设定比较的时间，单位为秒，默认为-1）

Comparison Fitters:比较修改工具

regular expression substitutions:替换正则表达式

Regex String:要替换的字符串（可从断言结果中选择）

substitutions：替换的字符串（替换结果）

用来比较两次取样结果，结果支持正在我表达式过滤。——比较断言会消耗很多资源，一般用于调试，不建议在压测中使用。


SMIME断言
----------------------------------------------

.. figure:: /_static/jmeter/j_12.png
    :width: 15.0cm


SMIME是一种多用途网际邮件扩充协议，相比于之前的SMAP邮件传输协议，增加了安全性，对邮件主题进行保护；

作用对象：针对采用了该种邮件传输协议的信息

signature:签名（可选择对协议的签名验证状态）

Verify signature:验证签名；Message not signed:没有签名消息。

Signer certificate：签名证书（因为SMIME协议增加了安全传输，需要证书验证）

 No check：不检查，Check values:检查。

Signer distinguished name:签名证书者名称（证书注册者的名称）

Sigmer email address:签名者的邮件地址（注册的邮件地址）

Issuer distinguished name:发行者名称（由谁发行的证书）

Serial Number:证书序号

Certificate file:选择证书文件

Execute assertion message at position:执行断言消息的位置（在返回消息的具体哪个位置执行断言）


JSR223 断言
------------------------------------------

.. figure:: /_static/jmeter/j_13.png
    :width: 15.0cm

JSR223即Java 规范请求，是指向JCP(Java Community Process)提出新增一个标准化技术规范的正式请求；
作用对象：针对sampler中的JSR223 sampler而使用的断言

Script language（e.g.beanshell,javascirpt,jexl）:脚本语言（可以从下面的下拉框中选择对应的脚本语言JavaScript、beanshell等）

parameters to be passed to script（=> String Parameters and String []args）:（传递给脚本的参数→可以理解为使用JSR223断言脚本时候一起引用的参数 ）

Script file（overrides script）：重写脚本（可以通过选择脚本文件的状态，是浏览调用已有的脚本还是在在下方的输入框内写入脚本；）

Script：下面的输入框表示可以输入变量类型，运用的脚本（取样结果、断言结果、取样日志文件等参数）
