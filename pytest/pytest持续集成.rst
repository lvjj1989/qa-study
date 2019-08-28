pytest持续集成
===========================================================

安装python环境
-------------------------------------------------------
https://www.python.org/
推荐安装python3.6以上版本


推荐安装以下第三方类库
-------------------------------------------------------
pip3 install requests
pip3 install pytest
pip3 install pytest-html
pip3 install pytest-allure-adaptor
pip3 install PyMySQL



注：pip下载python包是从(https://pypi.python.org/ ) 下载的，pypi服务器在国外，因此国内访问可能速度会比较慢，但使用时可以指定国内源，也就是从国内的镜像服务器下载，
如使用清华的源: pip3 install requests -i https://pypi.tuna.tsinghua.edu.cn/simple


安装Python IDE pycharm
-------------------------------------------------------------

推荐安装专业版，破解注册码：http://idea.lanyus.com/



构建Python项目
--------------------------------------------------------------

.. figure:: /_static/pytest/1.png
    :width: 15.0cm



配置Pycharm
------------------------------------------------------------

.. figure:: /_static/pytest/2.png
    :width: 15.0cm

建议使用虚拟环境（https://blog.csdn.net/qq_23924249/article/details/77602928）

.. figure:: /_static/pytest/3.png
    :width: 15.0cm


通过pytest自动化测试框架编写接口自动化测试
-----------------------------------------------------------------

pytest基础教程：

https://www.jianshu.com/p/a754e3d47671
http://lvjunjie.cn/qa-study/pytest/index.html

pytest官方文档：
https://docs.pytest.org/en/latest/



搭建持续集成
--------------------------------------------------------------------

1. 通过Jenkins下载allure插件
#. 配置构建项目时添加构建后操作，Allure Report
#. 执行pytest命令时，添加 ::

	--alluredir ${WORKSPACE}/allure-results


.. figure:: /_static/pytest/4.png
    :width: 15.0cm

**注：这里可能会有个坑，经测试pytest和pytest-allure-adaptor会存在着一些版本的兼容性问题，这里我用的pytest版本是4.0.2，pytest-allure-adaptor的版本是1.7.10*

生成allure测试报告
--------------------------------------------------



.. figure:: /_static/pytest/5.png
    :width: 15.0cm



allure相关资料：
https://docs.qameta.io/allure/#_pytest
https://www.cnblogs.com/yrxns/p/8386267.html

