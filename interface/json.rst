JSON
======================================

JSON(JavaScript Object Notation, JS 对象标记) 是一种轻量级的数据交换格式，易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。

JSON语法
--------------------------------------

* 对象表示为键值对
* 数据由逗号分隔
* 花括号保存对象
* 方括号保存数组

**怎么表示对象**
对象以一对花括号包含，里面包含键值对，如 ``{"a":1, "b": "1"}`` ，这样子，键值必须以双引号括起来，数据之前以逗号分隔

**怎么表示数组**
数组以一对方括号包含，如 ``[1, 2, "2, {"a": 1}]`` 这样子

**JSON对象和JSON字符串**
JSON对象比较像python里的字典，但还是有区别的，JSON对象里的字符串只能用双引号，python可以用单引号，JSON对象里只能包括基础数据类型，即数字（包含小数）、字符串、数组、字典、空值，但python字典可以包含其它对象，下面定义一个JSON对象::

    {
        "key1": 1,
        "key2": "1",
        "key3": [1, "1"],
        "key4": {
            "key5": 1
        }
    }

JSON字符串是将JSON序列化后的字符串，将上面的JSON对象序列化后为::

    '{"key3": [1, "1"], "key2": "1", "key1": 1, "key4": {"key5": 1}}'


在python中使用json
--------------------------------------

python标准库中自带json模块，可以序列化和反序列化json，在python中json对象就是字典，但python字典不一定是json对象

**序列化JSON**::

    # coding=utf-8

    import json

    raw_json = {
            "key1": 1,
            "key2": "1",
            "key3": [1, "1"],
            "key4": {
                "key5": 1
            }
        }

    json_str = json.dumps(raw_json)
    print(repr(json_str))

输入为::

    '{"key3": [1, "1"], "key2": "1", "key1": 1, "key4": {"key5": 1}}'
    [Finished in 0.2s]

**反序列化JSON**::

    # coding=utf-8

    import json

    raw_json = {
            "key1": 1,
            "key2": "1",
            "key3": [1, "1"],
            "key4": {
                "key5": 1
            }
        }

    json_str = '{"key3": [1, "1"], "key2": "1", "key1": 1, "key4": {"key5": 1}}'
    raw_json = json.loads(json_str)
    print(repr(raw_json))

输出结果为::

    {u'key3': [1, u'1'], u'key2': u'1', u'key1': 1, u'key4': {u'key5': 1}}
    [Finished in 0.2s]