pytest常用插件
================================================


失败重试
-----------------------------------------

在做测试的时候我们通常会遇到网络波动，这事我们需要做失败重试机制，确认失败后才进行失败告警或者在报告中体现

在pytest中，我们通常会使用安装pytest-rerunfailures来进行失败重试

::

    pip install -U pytest-rerunfailures

在命令行中增加 --reruns NUM，NUM为失败重试的最大次数
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


设定执行顺序
--------------------------------------------



::

    pip  install  pytest-ordering

根据order参数从小到大执行

::

    @pytest.mark.run(order=2)
    def test_order1():
        print ("first test")
        assert True
    @pytest.mark.run(order=1)
    def test_order2():
        print ("second test")
        assert True


测试时间限制
-----------------------------------------

在正常时间下，pytest是没有测试时间限制的，但有时候需要控制测试用例执行执行，可以使用pytest-timeout。

::

    pip install pytest-timeout

在命令行中添加 --timeout=X
X为该条命令执行的总时间限制，单位秒

::

    pytest --timeout=0.5 test_xxx.py



