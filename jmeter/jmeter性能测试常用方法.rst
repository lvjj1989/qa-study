jmeter性能测试常用方法
=================================


从Jenkins中获取参数到jmeter脚本
----------------------------------------

在Jenkins中可以添加配置参数，会加入到临时的环境变量中，在Jenkins通过BeanShel脚本变可以获取到Jenkins的参数

在Jenkins中添加参数，是你需要透传给jmeter的
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. figure:: /_static/jmeter/j_userd_1.png
    :width: 15.0cm


在jmeter添加一个前置处理器，BeanShell PreProcessor，输入以下代码：
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

	String DataId= System.getenv("data_id");
	vars.put("DataId",DataId);


在jmeter脚本脚本中通过 ${DataId}，获取Jenkins中配置的data_id
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


beanshell断言，可以在报告中展示异常的response
----------------------------------------------------------

在性能测试中，我们如果只使用了响应断言，那边在用例失败时，我们只知道响应里面没有需要的断言数据，但不知道实际响应了什么，这时候我们就可以把响应断言修改成以下的beanshell断言，这样失败时，FailureMessage就会出现在我们的报告中

.. figure:: /_static/jmeter/j_userd_2.png
    :width: 15.0cm


.. figure:: /_static/jmeter/j_userd_3.png
    :width: 15.0cm

代码如下：

::

	String response = "";
	String Str = "\"code\":0";
	response = prev.getResponseDataAsString();
	if (response == ""){
	    Failure = true;
	    FailureMessage = "系统无响应";

	    // System.out.print( FailureMessage);
	    }
	else if((response.contains(Str)) == false){
	    Failure = true;
	    FailureMessage = "接口响应异常，接口实际响应内容为：" + response;
	    // System.out.print(FailureMessage);
	    }

多个字符串断言，如Str1和Str2两者存在其一即为通过，代码如下：

::

	String response = "";
	String Str1 = "assert_str1";
	String Str2 = "assert_str2";
	response = prev.getResponseDataAsString();
	if (response == ""){
	    Failure = true;
	    FailureMessage = "系统无响应";
	// System.out.print( FailureMessage);
	}
	else if(!(response.contains(Str1) || response.contains(Str2))){
	    Failure = true;
	FailureMessage = "接口响应异常，接口实际响应内容为：" + response;
	// System.out.print(FailureMessage);
	}


jmeter使用Beanshell预处理器从指定列表中获取随机值
-------------------------------------

新增beanshell前置处理器

代码如下：

::

	//随机字符串
	String[] nation = new String[]{"china", "US", "UK"};
	Random random = new Random();
	int i = random.nextInt(nation.length);
	vars.put("mynation",nation[i]);
	//随机数字
	String[] num = new String[]{"8", "2", "1","7"};
	Random r = new Random();
	int j = r.nextInt(num.length);
	vars.put("anum",num[j]);

然后在脚本中使用${mynation}或者${anum}变可以获取对应参数

*注：数量较多时推荐使用csv，当然也可以使用随机函数，但是在性能测试中不推荐使用随机函数，部分随机函数生成时的性能较差，无法提供足够的负载*

仅一次控制器
----------------------------------------------
在编写性能测试是，有时候部分场景我们只希望执行一次，如登录，这时候我们就可以使用【仅一次控制器】，将登录接口加入到仅一次控制器中。这样每个线程只会执行一次登录。

**方法：在线程组中添加逻辑控制器，仅一次控制器**

