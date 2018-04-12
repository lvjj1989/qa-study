运行python
======================================


在解释器中运行
--------------------------------------
在命令行中输入python或者ipython，解释器交互环境，然后输入python脚本，敲击回车，即可执行::

    ➜ ~ python
    Python 2.7.13 (default, Apr 26 2017, 20:42:49)
    [GCC 4.2.1 Compatible Apple LLVM 8.0.0 (clang-800.0.42.1)] on darwin
    Type "help", "copyright", "credits" or "license" for more information.
    >>>

该种模式下退出交互环境，输入exit()::

    >>> exit()


::

    ➜ ~ ipython
    Python 2.7.13 (default, Apr 26 2017, 20:42:49)
    Type "copyright", "credits" or "license" for more information.

    IPython 5.1.0 -- An enhanced Interactive Python.
    ?         -> Introduction and overview of IPython's features.
    %quickref -> Quick reference.
    help      -> Python's own help system.
    object?   -> Details about 'object', use 'object??' for extra details.

    In [1]:

该种模式下退出输入quit或exit

直接运行python文件
--------------------------------------
在命令行模式下，执行输入 ``python <文件名>`` ，即可执行python脚本，如python脚本（a.py）内容为::

    print(123)

执行a.py::

    ➜ ~ python a.py
    123
    ➜ ~

如何新建python脚本文件
--------------------------------------
新建一个普通的纯文本文件，保存后缀为.py即可

.. danger::
  在windows下不允许使用notepad（记事本）、word新建脚本文件
