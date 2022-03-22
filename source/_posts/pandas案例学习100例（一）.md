---
title:  pandas案例学习100例（一）
date: 2022-02-18 15:00:30
categories: 数据分析

---



hello 大家好，长时间没有发布新的文章，证明小编在这段日子不是很忙，就是很堕落，显然是后者了

捡起键盘，准备推出pandas 案例100系列，分十次记录学习，多了你也看不进去，我也写不进去：

<!--more-->

**案例1：**

生成一年中，所有周一的日期：

```python
#生成一年中，所有周一的日期
import pandas as pd
date_range = pd.date_range(start='2021-01-01', end='2021-12-31', freq="W-MON")
date_range = pd.date_range(start='2021-01-01', periods=52, freq="W-MON")  # 一年52个周
print(date_range)

```

**案例2：**

生成一个月份的所有日期

```python
# 生成一个月份的所有日期
date_range = pd.date_range(start='2021-12-01', end='2021-12-31')
date_range = pd.date_range(start='2021-12-01', periods=31)  # 一個月31天
print(date_range)
```

**案例3：**

生成一天的所有小時

```python
#生成一天的所有小時
date_range = pd.date_range(start='2021-12-01', periods=24, freq="H")
date_range = pd.date_range(start='2021-12-01', end='2021-12-02', closed="left", freq="H")
print(date_range)
```

**案例4：**

使用list构造 Series

```python
import pandas as pd
# 使用list构造 Series
sites = ["Google", "Runoob", "Wiki"]
myvar = pd.Series(sites)
print(myvar)
```

**案例5：**

使用dict 构造 Series

```python
# 使用dict 构造 Series
sites = {'x': "Google", 'y': "Runoob", 'z': "Wiki"}
myvar = pd.Series(sites)
print(myvar)
```

**案例6：**

Series转换list

```python
import pandas as pd
# Series转换list
sites = {'x': "Google", 'y': "Runoob", 'z': "Wiki"}
myvar = pd.Series(sites)
data = myvar.tolist()
print(data)python
```

案例7：

将Series转换DataFrame

```python
import pandas as pd
# 将Series转换DataFrame
grades = {'语文': "100", '数学': "120", '英语': "60"}
myvar = pd.Series(grades)
data = pd.DataFrame(myvar, columns=["grade"])
print(data)
```

**案例8：**

用numpy 创建Series

101    10.0
102    20.0
103    30.0
104    40.0
105    50.0
106    60.0
107    70.0
108    80.0
109    90.0

```python
import pandas as pd
import numpy as np
# 用numpy 创建Series
s = pd.Series(
    np.arange(10, 100, 10),  # 数值：10-90，间隔10
    index=np.arange(101, 110),  # 索引：101-109， 间隔7
    dtype="float"  # 类型：float64
)
print(s)
```

**案例9：**

 转换Series 的数据类型

```python
import pandas as pd
import numpy as np

# 转换Series 的数据类型
s = pd.Series(
    data=["001", "002", "003", "004"],
    index=list("abcd"),
    dtype="float"  # 类型：float64
)

s = s.astype(int)  # 方法一
# s = s.map(int)# 方法二
print(s)

```

**案例10：**

给Series添加元素

```python
import pandas as pd

# 给Series添加元素
grades = {'语文': "100", '数学': "120", '英语': "60"}
data = pd.Series(grades)
data = data.append(pd.Series({
    "计算机": 30,
    "化学": 45
}))
print(data)

```

**案例11：**

用reset index将Series 转换成df

```python
import pandas as pd
# 用reset index将Series 转换成df
grades = {'语文': "100", '数学': "120", '英语': "60"}
data = pd.Series(grades)
df = data.reset_index()
df.columns = ["course", "grade"]
print(data)
```

