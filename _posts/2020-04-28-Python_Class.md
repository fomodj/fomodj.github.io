# 实验：类的定义与使用
## 一．实验目的
1. 学会定义类
2. 学会根据类创建对象
3. 掌握对象的使用
4. 掌握对象的序列化与反序列化
## 二．实验原理
类的定义：利用所提供的功能描述，用代码实现类的定义及使用
## 三．实验步骤与内容
1. 编写一个名为Employee的类，其中包含关于员工的以下数据：姓名、ID号码、部门和职位。
2. 完成Employee类定义后，编写一个程序创建三个Employee对象来保存以下数据, 程序应将这些数据存储在三个对象中，然后在屏幕上显示每个员工的数据：

|姓名(name)|ID号码 (id)|部门(department)|职位(position)|
| ------ | ------ | ------ |------ |
|Susan Meyers|47899|Accounting|Vice President|
|Mark Johns|39119|IT|Programmer|
|Joy Rogers|81774|Manufacturing|Engineer|

3. 编写一个程序，将三个对象的数据存储在一个名为employee.data的文件中。
4. 编写一个程序，在employee.data文件中的数据反序列化，并将每个人的信息保存在一个字典中，所有人的信息放在一个列表中。

## 四：代码
### employee.py
```python
class Employee:
    def __init__(self,name,id,department,position):
        self.__name = name
        self.__id = id
        self.__department = department
        self.__position = position
    def __str__(self):
        return 'Name:'+self.__name+'\nId:'+self.__id+'\nDepartment:'+self.__department+\
            '\nPosition:'+self.__position
```
### employee_test.py
```python
from employee import Employee
def main():
    # 数据
    people01 = Employee('Susan Meyers','47899','Accouting','Vice President')
    people02 = Employee('Mark John','39119','IT','Programmer')
    people03 = Employee('Joy Rogers','81774','Manufacturing','Engineer')

    print(people01.__str__()+'\n')
    print(people02.__str__()+'\n')
    print(people03.__str__())
main()
```
### pickle_employee.py

```python
import pickle
from employee import Employee
FILENAME = 'employee.data'
def main():

    # 创建并打开文件
    output_file = open(FILENAME, 'wb')
    # 数据
    people01 = Employee('Susan Meyers', '47899', 'Accouting', 'Vice President')
    people02 = Employee('Mark John', '39119', 'IT', 'Programmer')
    people03 = Employee('Joy Rogers', '81774', 'Manufacturing', 'Engineer')
    # 序列化
    pickle.dump(people01,output_file)
    pickle.dump(people02,output_file)
    pickle.dump(people03,output_file)
    output_file.close()
    print("The data was written to" + FILENAME)
main()
```
### unpickle_employee.py
```python
import pickle
from employee import Employee
FILENAME = 'employee.data'
def main():
    end_of_file = False
    # 打开文件
    input_file = open(FILENAME,'rb')
    # 一直读取到文件的最后
    while not end_of_file:
        try:
            # 反序列化并转成字典
            people01 = pickle.load(input_file).__dict__
            people02 = pickle.load(input_file).__dict__
            people03 = pickle.load(input_file).__dict__
            print(people01)
            print(people02)
            print(people03)
            # 所有信息放入一个列表
            list = [people01,people02,people03]
            print(list)
        except EOFError:
            end_of_file = True
            input_file.close()
main()
```
