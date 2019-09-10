pytest常用插件
================================================


失败重试
-----------------------------------------

在做测试的时候我们通常会遇到网络波动，这事我们需要做失败重试机制，确认失败后才进行失败告警或者在报告中体现

在pytest中，我们通常会使用安装pytest-rerunfailures来进行失败重试

::

	pip install -U pytest-rerunfailures

在命令行中增加 --reruns NUM，NUM未失败重试的最大次数
::

	pytest test_se.py --reruns 3


重复运行
-----------------------------------------

::

	pip install pytest-repeat


在执行命令中添加--count=NUM NUM为每条用例需要执行的次数

::

	pytest test_se.py --count=3


并行运行
-----------------------------------------

::

	pip install pytest-xdist




测试时间限制
-----------------------------------------


