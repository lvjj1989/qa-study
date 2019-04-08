jmeter在linux上基于Jenkins持续集成参数化配置
==================================================


在本地完成jmeter脚本
------------------------------------------------------------------------------------


编写线程组时，将线程数与执行时间时进行参数化处理，从shell脚本中获取字段值
------------------------------------------------------------------------------------

.. figure:: /_static/jmeter/jmeter_jenkins_1.png
    :width: 15.0cm

线程组修改为：${__P(ThreadNumber,400)}
执行时间修改为：${__P(Diration,300)}

备注：若在shell未传递，则对应参数：线程数默认取400，执行时间默认300秒



保存后将生成的jmx文件上传到Linux服务器对应的路径上，配置任务
------------------------------------------------------------------------------------


.. figure:: /_static/jmeter/jmeter_jenkins_3.png
    :width: 15.0cm


.. figure:: /_static/jmeter/jmeter_jenkins_2.png
    :width: 15.0cm


.. figure:: /_static/jmeter/jmeter_jenkins_5.png
    :width: 15.0cm



点击构建，配置线程数等
------------------------------------------------------------------------------------


.. figure:: /_static/jmeter/jmeter_jenkins_4.png
    :width: 15.0cm


点击【立即构建】，开始自动执行性能测试（建议先2个线程，10秒执行测试一下）



执行完成后，自动生成测试报告，点击【HTML Report】查看测试报告
------------------------------------------------------------------------------------

.. figure:: /_static/jmeter/jmeter_jenkins_6.png
    :width: 15.0cm


若报告中数据无法正常显示，原因可能为Jenkins中html安全问题导致，

进入系统管理——>脚本命令行——>输入：

System.setProperty("hudson.model.DirectoryBrowserSupport.CSP", "")

点击运行，再重新构建脚本即可。


分布式压测（或者将本地机器作为调度机，其他服务器作为负载机）
--------------------------------------------------------------

在较大并发量时推荐使用分布式压测，修改Jenkins是中的shell脚本即可

方法流程：

1、修改控制机中jmeter.properties 文件remote_host参数，remote_host=127.0.0.1,xxx.xxx.xxx.xxx:1099
(xxx.xxx.xxx.xxx为负载机ip，可配置多个，逗号隔开)

2、启动负载机中jmeter-server

3、在shell命令中增加-r，修改【-JThreadNumber=$ThreadNumber -JDiration=$Diration】为【-GThreadNumber=$ThreadNumber -GDiration=$Diration】
(-J为局部变量配置，如果需要分布式压测，需要将命令行参数带到负载机中，需要用-G全部变量配置)

4、开始压测


5、Windows下本地机器作为负载机时，

修改本地jmeter文件bin目录下jmeter.properties以下参数中：

remote_hosts=xxx.xxx.xxx.xxx:1099

server.rmi.ssl.disable=true

启动时点击【运行】——>【远程启动】