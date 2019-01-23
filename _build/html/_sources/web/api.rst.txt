selenium webdriver api介绍
======================================
在上一节中， 我们使用requests发送http请求，调用了chromedriver的操作浏览器的web api，看起来有点繁琐，但我们只是演示用来理解webdriver原理的，生产中我们完全不用这么干
这些繁琐的调用selenium这个项目已经帮我们封装好了，selenium就是干这个事儿的，就是封装了对webdriver的web api调用的客户端实现，如下脚本是实现了对BOPS后台的登录功能

::

    import time
    from selenium import webdriver
    
    # 创建driver对象
    driver = webdriver.Chrome()
    driver.implicitly_wait(30)
    
    # 打开登录页
    driver.get('http://bopsadmin.test.apitops.com')
    
    # 输入登录用户名
    driver.find_element_by_id('loginName').send_keys('top')
    
    # 输入登录密码
    driver.find_element_by_id('password').send_keys('123456')
    
    # 点击登录
    driver.find_element_by_id('btnLoginCRM').click()
    
    # 等待5秒后退出
    time.sleep(5)
    driver.quit()

可以看到，我们没有手工发送一个http请求，但我们完成了浏览器的操作，这就是selenium webdriver的作用，下面我们介绍一些常用的api

浏览器操作
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
::

    # 打开登录页
    driver.get('http://bopsadmin.test.apitops.com')
    
    # 浏览器当前地址
    driver.current_url
    # 后退
    driver.back()
    
    # 前进
    driver.forward()
    
    # 刷新
    driver.refresh()
    
    # 最大化
    driver.maximize_window()

cookie操作
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
::

    # 添加cookie
    driver.add_cookie({'xxx': 'asdfasdfasdf'})
    
    # 删除cookie
    driver.delete_all_cookies()
    driver.delete_cookie('xxx')
    
    # 获取cookie
    driver.get_cookie('xxx')
    driver.get_cookies()

元素定位
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
::

    # 通过id定位
    driver.find_element_by_id('xxx')
    
    # 通过name属性定位
    driver.find_element_by_name('xxx')
    
    # 通过classname定位
    driver.find_element_by_class_name('xxx')
    
    # 通过css选择器定位
    driver.find_element_by_css_selector('xxx')
    
    # 通过xpath定位
    driver.find_element_by_xpath('//xxx')
    
    # 通过tag name定位
    driver.find_element_by_tag_name('xxx')
    
    # 通过链接的文字
    driver.find_element_by_link_text('xxx')
    
    # 通过部分链接文字
    driver.find_element_by_partial_link_text('xxx')

元素操作
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
::

    # 点击元素
    web_element.click()
    
    # 清空输入框（这个方法用于输入框有效）
    web_element.clear()
    
    # 输入字符（这个方法用于输入框）
    web_element.send_keys('xxx')
    
    # 获取属性
    web_element.get_attribute('xxx')
    
    # 元素是否可见
    web_element.is_displayed()
    
    # 元素是否enable
    web_element.is_enabled()
    
    # 元素是否选中（针对选项框或单选框）
    web_element.is_selected()
    
    # 提交（针对表单的提交按钮，不过一般用click就够了）
    web_element.submit()
    
    # 截图
    web_element.save_screenshot('xxx.png')


窗口对象操作
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
::

    # 当前浏览器窗口引用
    driver.current_window_handle
    
    # 当前所有的浏览器窗口引用集合
    driver.window_handles
    
    # 切换到指定窗口，通过窗口名或窗口引用
    driver.switch_to.window('window_name')

弹窗处理
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
::

    # 确定浏览器弹窗
    driver.switch_to.alert.accept()
    
    # 取消浏览器弹窗
    driver.switch_to.alert.dismiss()


frame操作
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
现在前端基本上已经不使用frame了。。 不过还是有必要讲一下::

    """
    Switches focus to the specified frame, by index, name, or webelement.
    
    :Args:
    - frame_reference: The name of the window to switch to, an integer representing the index,
                        or a webelement that is an (i)frame to switch to.
    
    :Usage:
        driver.switch_to.frame('frame_name')
        driver.switch_to.frame(1)
        driver.switch_to.frame(driver.find_elements_by_tag_name("iframe")[0])
    """
    
    driver.switch_to.frame('xxxx')
    
    # 恢复到默认的frame
    driver.switch_to.default_content()

时间相关
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
::

    # 隐式等待元素被发现，一个driver session只需要调用一次
    driver.implicitly_wait(30)
    
    # 设置页面加载超时时间
    driver.set_page_load_timeout(30)
    
    # 设置脚本超时时间
    driver.set_script_timeout(30)

    # 等带某个元素出现(在timeout秒内出现locator)
    import selenium.webdriver.support.ui as ui
    import selenium.webdriver.support.expected_conditions as EC
    ui.WebDriverWait(driver, timeout).until(EC.visibility_of_element_located((By.XPATH, locator)))

其它不常用操作
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
::

    # 异步执行JS脚本
    driver.execute_async_script('alert(123)')
    
    # 同步执行JS脚本
    driver.execute_script('alert(123)')
    
    # 获取浏览器窗口大小
    driver.get_window_size()
    
    # 获取浏览器窗口在屏幕中的坐标
    driver.get_window_rect()
    
    # 设置窗口位置
    driver.set_window_position()
    
    # 设置窗口大小
    driver.set_window_size()

鼠标事件
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
鼠标事件方法被通常封装是ActionChains中
::

    #右击
    context_click()

    #双击
    double_click()

    #拖动
    drag_and_drop()

    #鼠标悬停
    move_to_element()

    #滚动到元素target_xpath
    target = driver.find_element_by_xpath(target_xpath)
    driver.execute_script("arguments[0].scrollIntoView();", target_xpath)


键盘事件
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
在使用键盘按钮方法前需要先导入keys类
::

    from selenium.webdriver.common.keys import Keys

    # 以下为常用的键盘操作
    send_keys(Keys.BACK_BACK_SPACE) # 删除键
    send_keys(Keys.BACK_SPACE)      # 空格键
    send_keys(Keys.Tab)             # 制表键(Tab)
    send_keys(Keys.ESCAPE)          # 回退键(ESC)
    send_keys(Keys.ENTER)           # 回车键
    send_keys(Keys.CONTROL,'a')     # 全选(Ctrl + A)
    send_keys(Keys.CONTROL,'c')     # 复制(Ctrl + C)
    send_keys(Keys.CONTROL,'x')     # 减去(Ctrl + X)
    send_keys(Keys.CONTROL,'v')     # 粘贴(Ctrl + V)
    send_keys(Keys.F1)              # F1
