这是做什么的？
=======================
这是一个任务安排工具，可以根据设定的时间运行指定的任务（函数），时间设置丰富，欢迎使用

怎么安装？
=========
pip install sxclzy

怎么使用
=========
```
from sxclzy.sxclzy import Sxclzy

def your_function(func_arc):
    # your missions with func_arc
    pass

SL = Sxclzy()
SL.add_schedule(
            name='your function name',          # （必须）任务名称，是唯一的，任务的标示
            func=your_function,                 # （必须）任务函数，注意不要加上括号()
            schedule_dic={'second': '*/10'},    # （必须）任务安排，传入字典，具体解释见下面
            run_times=3,                        # 任务运行的次数，默认为无数次，到达任务次数后需要重新add一次任务
            args={'func_arc': args},             # 传入任务函数的关键字字典，
            status=1,                           # 任务的启用状态，默认为1为启用，0 则不启用
            overwrite_if_exist=True             # 若任务列表已有任务，是否覆盖，默认为True
        )
SL.start()  # 开始任务
```

关于时间设置
============
“schedule_dic” 的值需要一个字典，类似：
```
{"second": "*/30"}
```
意思是每30秒运行一次，也可以设置为具体的某个时间点，例如：
```
...
{
    'year': 2019,
    'month': 6,
    'day': 4,
    'hour': 10,
    'minute': 50,
    'second': 50
  }
```

这样的设置是单次任务模式，任务只在设定的时间点（2019-06-04 10:50:50）运行一次

也可以是只有分钟，或者只有秒，或者只有小时、天、月、年，程序将自动计算下次到达设定的时间的时间间隔，

例如只给定：
```
{'hour': 10}
```
则任务会在从明天开始的每一天的10点的当前分秒数启动
例如当设置为：
```
{"hour": 10}
```
而此时系统时间为 17:37:12，则爬虫将在明天，以及接下来的每一天的上午的 10:37:12 自动运行任务。

若只设置为：
```
{'day': 4}
```
则意思是从下个月开始的每个月的4号的当前时分秒启动，以此类推

需要注意的是，设定的月为1~12， 日小于等于月最大天数，小时为0-23， 分钟数0-59，秒数0-59

此外，我想你应该已经完全了解在参数中加 “ * / ” 或 不加 “ * / ”的作用了，若仍觉得不清楚，直接使用它你会发现其中的含义。

此外还可以设定星期数，例如：
```
{'week': '*/2'}
```
意思是每周的周三执行，对的，星期的范围为0~6

星期可以配合年、月、时、分、秒同时设定，但是不可同时设定星期数和天，否则将只按照天来计算，并忽略星期的设定。

