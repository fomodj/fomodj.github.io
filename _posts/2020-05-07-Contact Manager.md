---
layout: post
title:  "Python 特殊的加密/解密程序"
categories: Python学习
tags: Python 加密 解密 
author: Fomo
---

## 文件加密/解密程序：
设计和编写一个程序，允许用户在加密或解密文件之间交互选择:
* 使用以下描述的加密方案加密用户指定的文本文件（.txt扩展名），以生成具有相同名称的新编码文件（.zzz扩展名除外）。
* 使用用户指定的关键字解密用户指定的加密文件（.zzz扩展名）以生成具有相同名称的新文本文件
（.txt扩展名除外）。
加密方案是一种基于用户指定的正整数密钥和以下78个字符序列的替换密码形式：(注：“K”和“l”之间的空白字符)
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9zMS5heDF4LmNvbS8yMDIwLzA1LzA3L1laZFo2Si5wbmc?x-oss-process=image/format,png#pic_center)

整数密钥用于只对消息中的第一个字母进行加密，方法是将上述序列按该数量“移动”。 例如，一个整数密钥5将‘B’移动到加密的‘d’。移过序列的右端，就会回到序列的开头。例如，从‘Z’移动5位将加密为‘b’。

消息中第一个字母的序列位置用作消息中第二个字母的移位量。如果第一个字母是‘B’，其索引位置为4 (从‘a'位于位置0，‘A'位于位置1开始，等等)，那么消息中的第二个字母向右移动4位，即‘e’移动4位到‘F’。消息中第二个字母的序列位置用作消息中第三个字母的移位量，例如第二个字母为‘e’，而‘e’的索引位置为12，那么第三个字母‘ ’(空白字符)，移位量12就到了字符‘/’，等等。任何不在上述序列中的消息字符(例如，' < '，'{')被复制到加密的消息中，而不需要修改前一个移位量，该移位量转移到要加密的消息中的下一个字母中。考虑下面这个密钥5的例子:

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9zMS5heDF4LmNvbS8yMDIwLzA1LzA3L1laZGVYOS5wbmc?x-oss-process=image/format,png#pic_center)
## 程序要求：
* 程序要求能够提示三个选项：加密、解密还是退出。
* 验证密钥是否为正整数的正确性。
* 验证要加密/解密的用户指定文件是否存在，并强制用户重新输入，直到他们指定一个现有文件。您可以通过列出适当类型的所有文件(即.txt扩展用于加密或.zzz扩展用于解密)。
* 检查输出文件(加密的.zzz扩展名或解密的.txt扩展名)是否存在，然后打开它进行写入。 如果它已经存在，询问用户是否同意它被删除。如果不是，请他们选择一个不同的文件名来接收输出。

## 代码：
### hw2.py

```python
import os
from os.path import exists

# 读取加密所用的序列
code = open('字符串.txt','r')
sequence = code.readline()

# 读取需要进行加密的信息
def getinfo_encryption():
    global profileName
    profileName = input("请输入需要加密的文件名(无须后缀名)：")
    fileName = profileName +'.txt'
    if not exists(fileName):
        print("该文件", fileName, "不存在！")
    else:
        file = open(fileName,'r')
        global data
        data = ''
        line = file.readline()
        while line:
            data += line
            line = file.readline()
        return data

# 读取需要进行解密的信息
def getinfo_decryption():
    global profileName1
    profileName1 = input("请输入需要解密的文件名(无须后缀名)：")
    fileName = profileName1 +'.zzz'
    if not exists(fileName):
        print("该文件", fileName, "不存在！")
    else:
        file = open(fileName,'r')
        global data1
        data1 = ''
        line = file.readline()
        while line:
            data1 += line
            line = file.readline()
        return data1

# 定义加密函数
def encryption(data):
    while True:
        k = eval(input("请输入用于加密的正整数密钥："))
        if isinstance(k, int) and k > 0:
            print('已确认密钥为',k)
            break
        else:
            print('输入错误')
    str = data
    str1 = ''
    # 对第一个字母进行加密，密钥为K
    str1 += sequence[(sequence.find(str[0])+k)%78]
    for i in range(1,len(str)):
        # 不在序列中的消息字符直接复制进加密信息
        if str[i] not in sequence:
            str1 += str[i]
        else:
            if str[i-1] not in sequence:
            # 倘若需要加密信息的前一个是无需加密的，使用前一个有效移位量
                str1 += sequence[(sequence.find(str[i]) + sequence.find(str[i - 4])) % 78]
            else:
            # 需要加密的信息所使用的移位量
                str1 += sequence[(sequence.find(str[i]) + sequence.find(str[i - 1])) % 78]
    print("加密内容为：")
    print(str1,end='\n')
    if not exists(profileName+'.zzz'):
        with open(profileName+'.zzz','w') as fw:
            fw.write(str1)
    else:
        order=input("同名文件已存在，是否删除？(y/n)\n")
        if order == 'y':
            os.remove(profileName+'.zzz')
            with open(profileName + '.zzz', 'w') as fw:
                fw.write(str1)
        else:
            newname = input("请输入新的文件名(无须后缀)：")
            with open(newname + '.zzz', 'w') as fw:
                fw.write(str1)
    print("数据加密完成！退出请按“q”键！")



# 定义解密函数
def decryption(data1):
    while True:
        k = eval(input("请输入用于解密的正整数密钥："))
        if isinstance(k, int) and k > 0:
            print('已确认密钥为',k)
            break
        else:
            print('输入错误')
    str = data1
    str1 = ''
    # 对第一个字母进行解密，密钥为K
    str1 += sequence[(sequence.find(str[0])-k)%78]
    for i in range(1, len(str)):
        # 不在序列中的消息字符直接复制进解密信息
        if str[i] not in sequence:
            str1 += str[i]
        else:
            if str[i - 1] not in sequence:
                # 倘若需要解密信息的前一个是无需解密的，使用前一个有效移位量
                str1 += sequence[(sequence.find(str[i]) - sequence.find(str1[i - 4])) % 78]
            else:
                # 需要解密的信息所使用的移位量
                str1 += sequence[(sequence.find(str[i]) - sequence.find(str1[i - 1])) % 78]
    print("解密内容为：")
    print(str1, end='\n')
    if not exists(profileName1 + '.txt'):
        with open(profileName1 + '.txt', 'w') as fw:
            fw.write(str1)
    else:
        order = input("同名文件已存在，是否删除原文件？(y/n)\n")
        if order == 'y':
            os.remove(profileName1 + '.txt')
            with open(profileName1 + '.txt', 'w') as fw:
                fw.write(str1)
        else:
            newname = input("请输入新的文件名(无须后缀)：")
            with open(newname + '.txt', 'w') as fw:
                fw.write(str1)
    print("数据加密完成！退出请按“q”键！")

# 菜单切换
def run():
    while True:
        order = input("请输入指令：")
        if order not in ['e','d','q']:
            print("指令输错误，请重新输入！")
        else:
            if order == 'e':
                while getinfo_encryption():
                    encryption(data)
                    break
            elif order == 'd':
                while getinfo_decryption():
                    decryption(data1)
                    break
            elif order == 'q':
                print("程序结束，谢谢使用！")
                break

# 主函数
def main():
    info = '''
****加密/解密程序菜单***
      1.加密请按“e”
      2.解密请按“d”
      3.退出请按“q”
***********************
    '''
    print(info)
    run()

if __name__ == "__main__":
    main()
```
### 字符串.txt

```python
aA0bB1cC2dD3eE4fF5gG6hH7iI8jJ9kK lL,mM.nN?oO/pP;qQ:rR'sS"tT!uU@vV$wW%xX&yY-zZ=
```
### message.txt

```python
Be by the Union at 6 PM!

<We'll study for Data Structures...>
- Sam
```
