使用python requests调用接口
======================================
在这一节，我们使用python的requests库调用上一节中写的接口

准备工作
--------------------------------------

* 运行上一节的脚本
* 安装requests库::
  
    pip install requests

测试获取LIST
--------------------------------------

在交互环境或者IDE中输入如下代码::

    # coding=utf-8
    import requests

    # 请求该接口
    response = requests.get('http://localhost:5000/todos')

    # 获取响应数据，并解析JSON，转化为python字典
    result = response.json()

    # 打印响应状态码
    print(response.status_code)

    # 打印result
    print(result)

    # 打印结果中的 ‘todo1’ 任务
    print(result['todo1'])

然后运行，输出如下::

    200
    {u'todo1': {u'task': u'build an API'}, u'todo3': {u'task': u'profit!'}, u'todo2': {u'task': u'?????'}}
    {u'task': u'build an API'}
    [Finished in 0.4s]

增加一条
---------------------------------------

在交互环境或者IDE中输入如下代码::

    # coding=utf-8
    import requests

    # 定义要添加的任务
    task = {'task': 'my task'}

    # 请求该接口
    response = requests.post('http://localhost:5000/todos', data=task)

    # 获取响应数据，并解析JSON，转化为python字典
    result = response.json()

    # 打印响应状态码
    print(response.status_code)

    # 打印result
    print(result)

响应如下::

    201
    {u'task': u'my task'}
    [Finished in 0.6s]

requests
---------------------------------------

看了上面的两个例子，大家对requests有了大致的了解，现在总结一下基本的使用方法，更多可以查看一下官方文档 http://docs.python-requests.org/en/master/ 或者中文文档（不是最新的，不过影响不大） http://cn.python-requests.org/zh_CN/latest/

* 发送不同的http请求，直接使用 ``requests.<方法名小写>`` 即可，如 ``requests.get`` 、 ``requests.post`` 

* 传送url查询参数的方法如下，试验过程中，大家可以抓包看一下效果::

    import requests

    p = {'a', 1}
    requests.get('http://xxxx', params=p)


* 传送form表单格式的数据，使用方法如下::

    import requests
    d = {'a': 1}
    requests.post('http://xxxxx', data=d)

* 传送json数据使用方法如下::

    import requests
    d = {'a': 1}
    requests.post('http://xxxxx', json=d)
    