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
pip3 install Jinja2
pip3 install PyMySQL
pip3 install redis



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

.. figure:: /_static/pytest/4.png
    :width: 15.0cm


生成allure测试报告
--------------------------------------------------

.. figure:: /_static/pytest/5.png
    :width: 15.0cm



参考代码：https://github.com/lvjunjie84/pytest_interface