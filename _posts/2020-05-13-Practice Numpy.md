---
layout: post
title:  "利用numpy数据分析---酒鬼漫步"
categories: Python学习
tags: Python Numpy
author: Fomo
---

## 1. 实验内容
在一片空旷的平地上，有一个酒鬼，他最初停留在原点的位置，这个酒鬼每走一步时，方向是不确定的，在经过时间t之后，我们希望计算出这个酒鬼与原点的距离。
这个酒鬼走了2000步（每步0.5米），向前走一步记为1，向后走一步记为-1，当计算距原点的距离时，就是将所有的步数进行累计求和。
## 2. 实验步骤
（1）使用random模块来随机生成2000个0,1的值（掷硬币值）（两个结果任选一个），利用函数使0变成-1
（2）使用cumsum()函数步数累计和，显示酒鬼每一步距原点的距离
（3）找出酒鬼离原点正向最远、反向最远距离
（4）当酒鬼距原点的距离大于或等于15米时，总共走了多少步？如果没有走到15米，请输出：'酒鬼最远也没走到15米'
## 3. 代码示例
```python
import numpy as np

# 使用random模块来随机生成2000个0,1的值（掷硬币值）（两个结果任选一个），利用函数使0变成-1
vector = np.random.choice([0,1],2000)
equal_to_zero = (vector==0)
vector[equal_to_zero] = -1

# 使用cumsum()函数步数累计和，显示酒鬼每一步距原点的距离
cum_sum = vector.cumsum()*0.5
print('酒鬼每一步距原点的距离(米):\n',cum_sum)

# 找出酒鬼离原点正向最远、反向最远距离
print('酒鬼距离原点正向最远的距离：{}米'.format(cum_sum[cum_sum.argmax()]))
print('酒鬼距离原点反向最远的距离：{}米'.format(cum_sum[cum_sum.argmin()]))

# 当酒鬼距原点的距离大于或等于15米时，总共走了多少步？如果没有走到15米，请输出：'酒鬼最远也没走到15米'
count = 0
if all(abs(i) < 15 for i in cum_sum):
    print('酒鬼最远也没走到15米')
else:
    for i in cum_sum:
        count += 1
        if i == 15 or i == -15:
            break
    print('首次大于或等于15米时，所走的步数：',count)
```
