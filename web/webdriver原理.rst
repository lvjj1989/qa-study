selenium webdriver原理
======================================
我们来结合下图简单讨论下webdriver的理，以chromedriver为例
当你运行chromedriver时，chromedriver会在本地启动一个web服务，这个服务定义了一些api，你可以通过调用这些web api来操作浏览器，
详情可查看 https://w3c.github.io/webdriver/webdriver-spec.html#list-of-endpoints 

  .. figure:: /_static/web/webdriver.png
    :width: 80.0cm

一个实验
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
在深入之前，我们先做一个实验

* 第一步： 启动chromedriver，在命令行里执行如下命令，从执行的反馈来看，chromedriver启动后在本地启动了一个WEB服务，端口是9515，但只能用于本地链接::

    sun@sun:~ » chromedriver
    Starting ChromeDriver 2.31.488763 (092de99f48a300323ecf8c2a4e2e7cab51de5ba8) on port 9515
    Only local connections are allowed.

* 第二步：把下面的代码copy到python编辑器里，执行

::

    # coding=utf-8
    import requests
    import time
    
    capabilities = {
        "capabilities": {
            "alwaysMatch": {
                "browserName": "chrome"
            },
            "firstMatch": [
                {}
            ]
        },
        "desiredCapabilities": {
            "platform": "ANY",
            "browserName": "chrome",
            "version": "",
            "chromeOptions": {
                "args": [],
                "extensions": []
            }
        }
    }
    
    # 打开浏览器 http://127.0.0.1:9515/session
    res = requests.post('http://127.0.0.1:9515/session', json=capabilities).json()
    session_id = res['sessionId']
    
    # 打开百度
    requests.post('http://127.0.0.1:9515/session/%s/url' % session_id,
                json={"url": "http://www.baidu.com", "sessionId": session_id})
    
    time.sleep(3)
    
    # 关闭浏览器，删除session
    requests.delete('http://127.0.0.1:9515/session/%s' % session_id, json={"sessionId": session_id})

由此，我们可以看到，我们通过调用 chromedriver提供的web api可以操作浏览器


webdriver是什么
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    The primary new feature in Selenium 2.0 is the integration of the WebDriver API. WebDriver is designed to provide a simpler, more concise programming interface in addition to addressing some limitations in the Selenium-RC API. Selenium-WebDriver was developed to better support dynamic web pages where elements of a page may change without the page itself being reloaded. WebDriver’s goal is to supply a well-designed object-oriented API that provides improved support for modern advanced web-app testing problems. 

上面是官网的介绍，翻译成中文就是：

* selenium 2.0最重要的特性就是集成了WebDrvier API，WebDriver除了解决一些Selenium-RC API的不足外，旨在提供更简单，更简洁的编程接口
* Selenium-WebDrivers为更好的支持动态页面（也就是ajax，不刷新页面改变DOM）而开发
* 目标是提供一套精心设计的面向对象的API，为现代WEB应用自动化测试提供更好的支持

总结一下，就是说它是为了更好的支持动态页面，更简单易用的编程接口

webdriver怎么运行
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Selenium-WebDriver makes direct calls to the browser using each browser’s native support for automation. How these direct calls are made, and the features they support depends on the browser you are using.

webdriver直接调用了浏览器对自动化测试的原生接口，具体怎么调用，取决于你使用的浏览器（chrome使用chromedriver，IE使用iedriver），但重要的是最终提供出来的接口是一样的

再简化下这个概念:

* 每个浏览器都有自己自动化测试接口，如打开网页，点击等
* 每个浏览器自己的webdriver实现，如chromedriver iedriver都封装了这些自动化测试接口，然后把这些操作以web service形式暴露出来

