> 本笔记跟学[北大的陈斌老师的Python算法课](【【慕课+课堂实录】数据结构与算法Python版-北京大学-陈斌-字幕校对-【完结！】】https://www.bilibili.com/video/BV1gy4y1E7M5?p=2&vd_source=b0e615d38d99501542b4ad41dbfdddcb)



# 概述

python的`timeit`模块可以用于判断程序的性能

```py
from timeit import Timer

t1 = Timer("test1()", "from __main__ import test1")
print("concat %f seconds\n" % t1.timeit(number = 1000))
```

