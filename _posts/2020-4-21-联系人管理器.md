---
layout: post
title:  "用Python列表和字典完成一个简单的通讯录管理器"
categories: Python学习
tags: Python 字典 列表
author: Fomo
---

* content
{:toc}

## 1.实验内容
通讯录管理器是一款生活实用软件，用来协助管理手机中的所有联系人。这个案例要求使用函数完成通讯录中联系人数据的增删改查等各种功能。根据键盘的输入来选择对应的函数要完成的功能。
## 2.实验步骤
（1）实现显示通讯录的功能菜单函数。
（2）完成获取用户输入信息的函数。
（3）完成“1.添加联系人”、“2.删除联系人”、“3.修改联系人”、“4.查询联系人”、“5.显示所有联系人”和“6.保存数据”功能的函数。数据以字符串的形式保存在文本文件中。
（4）主函数中利用循环，不断要等待用户输入1~6的数字，利用选择结构实现当用户输入不同的数字时，调用对应的函数，完成相应的功能。当用户输入0时，利用break语句退出循环，结束程序。
（5）添加函数recover_info，当启动程序时，将已经保存在文件中的数据读取出来，作为初始数据放入全局变量中。
## 3.列表代码

```python
# 读取初始数据
def recover_info(list):
    f = open("data.txt", 'r')
    line = f.readline()
    while line != "":
        line0 = line.rstrip()
        line1 = line0.split()
        list.append(line1)
        line = f.readline()

# 添加联系人函数
def add(list):
    name = input("请输入姓名：")
    flag = False # 判断是否已经存储，默认未存储
    for i in range(len(list)):
        if list[i][0] == name:
            print("该联系人已存在，请重新输入！")
            flag = True
            break
    if not flag:
        phone = input("请输入手机号码：")
        addlist = [name,phone]
        list.append(addlist)
        print("输入完成")

# 删除联系人函数
def delete(list):
    name = input("请输入需要删除的联系人：")
    flag = False # 判断是否已经存在，默认不存在
    for i in range(len(list)):
        if list[i][0] == name:
            del list[i]
            flag = True
            print("已删除成功")
            break
    if not flag:
        print("没有该联系人记录！")

# 修改联系人函数
def update(list):
    name = input("请输入需要修改的联系人：")
    flag = False  # 判断是否已经存在，默认不存在
    for i in range(len(list)):
        if list[i][0] == name:
            phone = input("请输入需要修改的电话号码：")
            list[i][1] = phone
            flag = True
            print("修改完成")
            break
    if not flag:
        print("没有该联系人记录！")

# 查询联系人函数
def search(list):
    name = input("请输入需要查询的联系人：")
    flag = False  # 判断是否已经存在，默认不存在
    for i in range(len(list)):
        if list[i][0] == name:
            print("您所查询的联系人的电话号码是：{}".format(list[i][1]))
            flag = True
            break
    if not flag:
        print("没有该联系人记录！")

# 显示所有联系人函数
def showall(list):
    print("*********************")
    for i in list:
        print("联系人信息：\t%s\t\t%s"%(i[0],i[1]))
    print("*********************")

# 保存数据
def savedata(list):
    f = open("data.txt", 'w')
    for i in range(len(list)):
        s = str(list[i]).replace('[', '').replace(']', '')  # 去除[],这两行按数据不同，可以选择
        s = s.replace("'", '').replace(',', '') + '\n'  # 去除单引号，逗号，每行末尾追加换行符
        f.write(s)
    f.close()
    print("保存文件成功")

# 菜单切换
def run(list):
    while True:
        order = input("请输入指令：")
        if order not in ['0','1','2','3','4','5','6']:
            print("指令输错误，请重新输入！")
        else:
            if order == '1':
                add(list)
            elif order == '2':
                delete(list)
            elif order == '3':
                update(list)
            elif order == '4':
                search(list)
            elif order == '5':
                showall(list)
            elif order == '6':
                savedata(list)
            elif order == '0':
                print("程序结束，谢谢使用！")
                break

# 主函数
def main():
    # 创建空列表
    list = []
    info = '''
******通讯录命令菜单******
1.添加联系人   2.删除联系人
3.修改联系人   4.查询联系人
5.所有联系人   6.保存数据
     输入0时，结束程序
*************************'''
    print(info)
    recover_info(list)
    run(list)


if __name__=="__main__":
    main()




```
## 4.字典代码
```python
# 读取初始数据
def recover_info(dict):
    f = open("data.txt", 'r')
    list1 = []
    line = f.readline()
    while line != "":
        line0 = line.rstrip()
        line1 = line0.split()
        list1.append(line1)
        line = f.readline()
    for i in list1:
        dict[i[0]] = i[1]

# 添加联系人函数
def add(dict):
    name = input("请输入姓名：")
    flag = False  # 判断是否已经存储，默认未存储
    for i in range(len(dict)):
        if name in dict:
            print("该联系人已存在，请重新输入！")
            flag = True
            break
    if not flag:
        phone = input("请输入手机号码：")
        dict[name] = phone
        print("输入完成")

# 删除联系人函数
def delete(dict):
    name = input("请输入需要删除的联系人：")
    flag = False # 判断是否已经存在，默认不存在
    if name in dict:
        del dict[name]
        flag = True
        print("已删除成功")
    if not flag:
        print("没有该联系人记录！")

# 修改联系人函数
def update(dict):
    name = input("请输入需要修改的联系人：")
    flag = False  # 判断是否已经存在，默认不存在
    if name in dict:
        phone = input("请输入需要修改的电话号码：")
        dict[name] = phone
        flag = True
        print("修改完成")
    if not flag:
        print("没有该联系人记录！")

# 查询联系人函数
def search(dict):
    name = input("请输入需要查询的联系人：")
    flag = False  # 判断是否已经存在，默认不存在
    if name in dict:
        print("您所查询的联系人的电话号码是：%s"%dict[name])
        flag = True
    if not flag:
        print("没有该联系人记录！")

# 显示所有联系人函数
def showall(dict):
    print("*********************")
    for key, value in dict.items():
        print('联系人信息：'+ key + ' ' + value)
    print("*********************")

# 保存数据
def savedata(dict):
    f = open("data.txt", 'w')
    for k, v in dict.items():  # 遍历字典中的键值
        s2 = str(v)  # 把字典的值转换成字符型
        f.write(k + " "+ s2 + '\n')
    f.close()
    print("保存文件成功")

# 菜单切换
def run(dict):
    while True:
        order = input("请输入指令：")
        if order not in ['0','1','2','3','4','5','6']:
            print("指令输错误，请重新输入！")
        else:
            if order == '1':
                add(dict)
            elif order == '2':
                delete(dict)
            elif order == '3':
                update(dict)
            elif order == '4':
                search(dict)
            elif order == '5':
                showall(dict)
            elif order == '6':
                savedata(dict)
            elif order == '0':
                print("程序结束，谢谢使用！")
                break

# 主函数
def main():
    # 创建空列表
    dict = {}
    info = '''
******通讯录命令菜单******
1.添加联系人   2.删除联系人
3.修改联系人   4.查询联系人
5.所有联系人   6.保存数据
     输入0时，结束程序
*************************'''
    print(info)
    recover_info(dict)
    run(dict)


if __name__=="__main__":
    main()




```
