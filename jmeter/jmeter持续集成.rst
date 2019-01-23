jmeter在linux上基于Jenkins持续集成参数化配置
==================================================

在本地完成jmeter脚本
-----------------------------------------------

编写线程组时，将线程数与执行时间时进行参数化处理，从shell脚本中获取字段值
--------------------------------------------------

.. figure:: /jmeter/pytest/jmeter_jenkins_1.png
    :width: 15.0cm

线程组修改为：${__P(ThreadNumber,400)}
执行时间修改为：${__P(Diration,300)}

备注：若在shell为传递时对应参数时，线程数默认取400，执行时间默认300秒


保存后将生成的jmx文件上传到Linux服务器对应的路径上，配置任务
----------------------------------------------------------


.. figure:: /jmeter/pytest/jmeter_jenkins_3.png
    :width: 15.0cm


.. figure:: /jmeter/pytest/jmeter_jenkins_2.png
    :width: 15.0cm


.. figure:: /jmeter/pytest/jmeter_jenkins_5.png
    :width: 15.0cm


点击构建，配置线程数等
--------------------------------------------------------------


.. figure:: /jmeter/pytest/jmeter_jenkins_4.png
    :width: 15.0cm


点击【立即构建】，开始自动执行性能测试（建议先2个线程，10秒执行测试一下）


执行完成后，自动生成测试报告，点击【HTML Report】查看测试报告
----------------------------------------------------

.. figure:: /jmeter/pytest/jmeter_jenkins_6.png
    :width: 15.0cm


若报告中数据无法正常显示，原因可能为Jenkins中html安全问题导致，

进入系统管理——>脚本命令行——>输入：

System.setProperty("hudson.model.DirectoryBrowserSupport.CSP", "")

点击运行，再重新构建脚本即可。
