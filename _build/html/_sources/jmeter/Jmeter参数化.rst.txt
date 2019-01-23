Jmeter参数化
==========================================

CSV Data Set Config
--------------------------------------

CSV Data Set Config可以制定文本文件这一行一行的提取文本内容，跟进分割符拆解这一行内容与变量名对应，然后这些变量就可以提供给取样器引用了。

参数说明如下：

**名称**：随意设置，最好有业务意义

**注释**：随意设置，可以为空

**Filename**：应用文件地址，可以是相对路径，也可以是绝对路径。相对路径的根节点是Jmeter的启动文件路径（%JMETER_HOME%\bin）

**File encoding**：读取参数文件的编码格式，建议使用UTF-8

**Variable Names**：定义参数名称，用逗号隔开，将会与文件中的参数对应，如果这里的参数数量超过了文件中的数量，多余的参数将会取不到值。如果格式如：loginname,passWord，在金额meter中读取该参数的方式为${loginname}

**Delimiter**：用例分割文件的分隔符，默认是英文逗号，如果参数文件用的是tab分割，则输入：\t

**Allow qioted data？**：是否允许拆分完成的参数中有分隔符，如果选是，则允许

**Recycle on EOF？**：是，参数文件循环遍历；否，参数文件遍历完成后不循环。

**Stop thread on EOF？**：与Recycle on EOF的False选择复用；是停止测试，否不停止测试

**Sharing mode**：参数文件共享模式，有以下三种

* All threads参数文件多所有现场共享，这就包括了同一测试计划中的不同现场组
* Current thread groups：只针对当前线程组共享
* Current thread：只针对当前现场获取
