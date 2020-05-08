---
layout: post
title:  "关于Python的分数类及运算符重载"
categories: Python学习
tags: Python 类 运算符重载 
author: Fomo
---

## 运算符重载

> 一种根据所使用的操作数更改Python中运算符的含义的做法。Python操作系统适用于内置类。
> 但同一运算符的行为在不同的类型有所不同。例如；"+"运算符将对两个数字执行算术加法，合并两个列表并连接两个字符串。Python中的这个功能，允许相同的操作符根据上下文的不同，其含义称为运算符重载。

下面以分数为例做运算符重载的示例：
### rational.py

```python
class Rational:
    '''
    Rational number
    denominator 分母
    numeration 分子
    '''
    def __init__(self,numerator,denominator):
        self.__numeration = numerator
        self.__denominator = denominator
        self.__reduce = self.__reduce__()

    def get_denominator(self):
        return self.__denominator

    def set_denominator(self,denominator):
        if not isinstance(denominator,float) and not isinstance(denominator,int):
            print("必须是浮点型或者整型")
            return
        self.__denominator = denominator

    def get_numeration(self):
        return self.__numeration

    def set_numeration(self,numeration):
        self.__numeration = numeration

    def gcd(a,b):# 求a和b的最大公约数
        if a is b:
            return a
        elif a.__numeration == b.__numeration and \
                a.__denominator == b.__denominator:
            return a
        else:
            # 求分子最大公约数
            if a.__numeration > b.__numeration:
                smaller = b.__numeration
            else:
                smaller = a.__numeration
            for i in range(1, smaller + 1):
                if ((a.__numeration % i == 0) and (b.__numeration % i == 0)):
                    new_nu = i
            # 求分母最小公倍数
            if a.__denominator > b.__denominator:
                greater = a.__denominator
            else:
                greater = b.__denominator
            while (True):
                if ((greater % a.__denominator == 0) and (greater % b.__denominator == 0)):
                    new_de = greater
                    break
                greater += 1
            new_rational = Rational(new_nu, new_de)
            return new_rational

    def __reduce__(self):# 对有理数进行约分，最简分数
        for i in range(2, self.__denominator):
            while (self.__denominator % i == 0 and self.__numeration % i == 0):
                self.__denominator = self.__denominator // i
                self.__numeration = self.__numeration // i
        return self.__numeration,self.__denominator

    def __str__(self):
        return str(self.__numeration)+'/'+str(self.__denominator)

    '''
    __add__     +
    __sub__     -
    __mul__     *
    __div__     /
    __mod__     %
    
    == __eq__   != __ne__   < __lt__    <= __le__   > __gt__    >= __ge__
    '''

    def __add__(self, other): # 加法
        new_nu = self.__numeration * other.__denominator + self.__denominator * other.__numeration
        new_de = self.__denominator * other.__denominator
        new_rational = Rational(new_nu,new_de)
        return new_rational

    def __sub__(self, other):# 减法
        new_nu = self.__numeration * other.__denominator - self.__denominator * other.__numeration
        new_de = self.__denominator * other.__denominator
        new_rational = Rational(new_nu, new_de)
        return new_rational

    def __mul__(self, other):# 乘法
        new_nu = self.__numeration * other.__numeration
        new_de = self.__denominator * other.__denominator
        new_rational = Rational(new_nu, new_de)
        return new_rational

    def __truediv__(self, other):# 除法
        new_nu = self.__numeration * other.__denominator
        new_de = self.__denominator * other.__numeration
        new_rational = Rational(new_nu, new_de)
        return new_rational

    def __eq__(self, other):# 是否相等
        if self is other:
            return True
        elif type(self) != type(other):
            return False
        elif self.__numeration == other.__numeration and \
                self.__denominator == other.__denominator:
            return True
        else:
            return self.__numeration * other.__denominator == self.__denominator * other.__numeration

    def __lt__(self,other): # 小于
        if self.__numeration * other.__denominator < self.__denominator * other.__numeration:
            return True
        else:
            return False

    def __ne__(self, other): # 不等于
        if self.__numeration * other.__denominator != self.__denominator * other.__numeration:
            return True
        else:
            return False

```

### rational_test.py

```python
from overload.rational import Rational

r1 = Rational(3,6)
r2 = Rational(6,7)

print(r1)
print(r2)
print(r1+r2)
print(r1-r2)
print(r1*r2)
print(r1/r2)
print(Rational.gcd(r1,r2))
print("r1==r2?",r1==r2)
print("r1<r2?",r1<r2)
print("r1!=r2?",r1!=r2)
```

