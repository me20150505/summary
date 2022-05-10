
# 准备前
```shell
# 模块安装
python3 -m pip install --user pygame
pip3 install pygame
pip3 install pygame==2.1.2

# 显示模块信息
pip3 show pygame

#列出安装模块
pip3 list
```

# 数据类型

- Number：-----数字：------int(), float()
- String：------字符串：-----str()
- Tuple：------元组：-------tuple()
- List：--------列表：------list()
- Set：--------集合：------set()
- Dictionary：---字典：------dict()

# 字符串

- str(number)
- string[start:end:step]
- string.title()         每个单词首字母大写
- string.upper()     转换大写
- string.lower()      转换小写
- string.rstrip()      删除字符串末尾空白
- string.lstrip()      删除字符串开头空白
- string.strip()       删除字符串首尾空白
- string.split(str)  切片字符串，不传 str 实参则以空格为分隔符分割
```python
# f字符串 python>=3.6
first_name = "aa";
last_name = "bb";
name = f"{first_name} {last_name}";
print(name);

# 字符串拼接
print('test string break line break line break line'
    ' break line break line break line - 2'
    ' break line - 3'
    '');
# 字符串格式化输出
print('''test string break line break line break line
    break line break line break line - 2
    break line - 3
    ''');

str = '{1}, {0}, {1}'.format('hello', 'world'); # world, hello, world
str = '{} is position {{0}}'.format('world'); # world is position {0}
str = '{:,}'.format(42348329423.4554); # 42,348,329,423.4554
```

# 数字

- % 求模运算
- // 求整除运算（对商向下取整）
- **  幂运算
- int(string)
- float(string)
```python
# Python之禅
import this

largeNum = 1_000;
print(largeNum);

# 初始化多个变量
x, y, z = 0, 10, 100;

int(round(105.56, -1)); # 110
```

# 列表

- lists.append(item)
- lists.insert(index, item)
- del lists[index]
- lists.pop(index)  index可为空，返回被删除元素
- lists.remove(item)
- lists.sort(reverse=True)  按字母表倒序排列，修改原列表，参数可为空
- lists.reverse()
- lists.count(item)  统计 item 出现次数
- sorted(lists, reverse=True)  按字母表倒序排列，不修改原列表
- len(lists)
- enumerate(lists)
- min(numlists)
- max(numlists)
- sum(numlists)

-------------------------------------

- range(start, end, step)  指定一个参数时为 [0, end)
- list(range)
- list[start:end:step]  列表切片，start 与 end 都省略则复制列表

-------------------------------------

- 列表解析：将 for 循环和创建新元素的代码合并成一行，并自动附加新元素
- **元组：由不可变的值所组成的不可变的列表，使用圆括号表示**
```python
# 访问最后一个和倒数第二个元素
languages = ['python', 'java', 'c'];
print(languages[-1], languages[-2]);

for index, item in enumerate(languages):
    if item != 'javascript' and \
        item != 'c++' and \
        item != 'java':
        print(f'index: {index}, item: {item}');

languages.reverse();
print(languages);

testList = list(range(1, 5));
print(testList);

# 反转列表
test_list = [1, 2, 3, 4, 5];

test_list.reverse();
print('反转原列表', test_list);

print('反转新列表', test_list[::-1]);

# 列表解析
squares = [num**2 for num in range(1, 11)];
print(squares);
# 列表切片
slices1 = squares[-3:];
print(slices1);
slices2 = squares[:5:2];
print(slices2);

# 元组
ar = (1, 45, 56);
print(ar);
```

# 字典

- del dicts[key]  不存在对应的键则报错
- dicts.get(key, defaultvalue)  获取字典键对应的值，不存在则返回默认值，不指定默认值返回 None
- dicts.items()
- dicts.keys()  遍历字典时会默认遍历所有的键，输出排序默认为添加的顺序
- dicts.values()
```python
dictTest = {'f': 100, 'a': 1, 'b': 'val'};
print(dictTest['b']);
print(dictTest.get('c', 'default value'));

for k, v in dictTest.items():
    print(f'{k}-{v}');
# 遍历字典时会默认遍历所有的键
for k in dictTest:
    print(f'key: {k}');
for k in sorted(dictTest.keys()):
    print(f'key: {k}');
```

# 集合

- set(lists)  元素唯一性
```python
# 创建集合
testSet = {'a', 'b', 'f'};
```

# 交互

- input(message)

# 语句

- for 语句
- if 语句
   - if-elif-else 结构
- while 语句

备注：

- 逻辑运算符：and, or, not
- 成员运算符：in, not in。检查特定值在不在列表中或字符串中使用 in 或 not in
- 身份运算符：is, is not
```python
# for 循环遍历列表时需保证列表长度不变，如果在 for 循环里删除元素，需遍历列表的副本
for item in items.copy():
    if item > 10:
        items.remove(item);

languages = ['python', 'java', 'c'];
for i in languages:
  print(i);

for i in range(1, 5):
    print(f'this is all num: {i}');
    if i % 2 == 0:
        print(f'this is double num: {i}');
    else:
        print(f'this is odd num: {i}');
        
# 检查空列表
testlist = [];
targetlist = [];
if testlist:
    for i in testlist:
        targetlist.append(i);
else:
    print('this is a empty list');
print(f'result list is: {targetlist}');

# while 语句
num = 1;
while num <= 5:
    print(f'num: {num}');
    num += 1;
```

# 注释

- 单行注释  #
- 多行注释  '''''' 或 """""" 文档字符串


# 函数

- 位置实参
- 关键字实参
- 剩余实参：* 的 args 为元组，** 的 kwargs 为字典
- 形参默认值：没有默认值的形参列在前面
- 存储函数或类于模块中
   - import module_name
   - import module_name **as** other_module_name
   - from module_name import *：直接使用模块的具体函数名即可，可能会覆盖现有的函数或变量
   - from module_name import func_name_1, func_name_2
   - from module_name import func_name **as** other_func_name
```python
# 位置实参
def testfunc(a, b=99):
    print(f'a is: {a}, b is: {b}');
testfunc(10, 20);

# 关键字实参 key word
testfunc(a=10, b=20);

# 剩余实参
def testfunc(*args):
    print(f'param: {args}');
testfunc(1);
testfunc(1, 3, 5);

def testfunc(name, age, **kwargs):
    kwargs['name'] = name;
    kwargs['age'] = age;
    return kwargs;
xm = testfunc('xiaoming', 18, loc='BJ', job='engineer');
print(xm);
```

# 类
```python
# 创建类的实例
class Car:
    def __init__(self, make, model, year):
        self.make = make;
        self.model = model;
        self.year = year;
        self.meter_reading = 0;
    
    def get_full_name(self):
        return f'{self.make}-{self.model}-{self.year}';
        
my_car = Car('Audi', 'A4', 2019);
print(my_car.make, my_car.get_full_name());

class Battery:
    def __init__(self, battery_size=75):
        self.battery_size = battery_size;
        
    def describe_battery(self):
        return f'This car has a {self.battery_size} kwh battery';

# 类的继承
class ElecCar(Car):
    def __init__(self, make, model, year):
        super().__init__(make, model, year);
        self.battery = Battery();
        
my_tesla = ElecCar('tesla', 'model s', 2019);
print(my_tesla.get_full_name());
print(my_tesla.battery.describe_battery());
```

# 文件

- open(filename, mode)
   - mode 示例说明
   - ab+：a(append) b(binary) +(update(read write))
   - rb+：r(read)
- file_object.read(size)
- file_object.close()
- file_object.readline()
- file_object.readlines(size)
- file_object.write(string)
```python
# 读取文件 使用 with 读取文件不用关心何时关闭文件
with open('./test.txt', encoding='utf-8') as f:
    contents = f.read();
print(contents.rstrip());

# 逐行读取
with open('./text.ts') as f:
    for line in f:
        print(line);
        
# 写入空文件
with open('./test.ts', 'w') as f:
    f.write('hello\n');
    f.write('world');
```

# 异常

- try-except-else 代码块
- pass 空语句
```python
print('Give me two numbers, and I will divide them');
print('Enter "q" to quit\n');

while True:
    f_num = input('First number: ');
    if f_num == 'q':
        break;
    s_num = input('Second number: ');
    if s_num == 'q':
        break;
    try:
        result = int(f_num) / int(s_num);
    except ZeroDivisionError:
        # 提示错误
        # print('division by zero error');
        
        # 静默失败
        pass;
    else:
        print(result);
```

# 常用模块
## os 模块
```python
import os

temp_path = os.path.join(os.path.dirname(__file__), os.path.pardir);
print(temp_path);
```

## sys 模块

- sys.exit()

## datetime 模块

- 实参格式
   - %A  星期英文全称
   - %B  月份英文全称
   - %Y  四位数年份
   - %y   两位数年份
   - %m 月份, 01-12
   - %d  日期, 01-31
   - %H  小时24制, 00-23
   - %I    小时12制, 01-12
   - %M  分钟, 00-59
   - %S   秒, 00-61, 60/61 为闰秒
   - %p   am/pm
```python
from datetime import datetime;

date_str = '2018-02-02 11:10:10';
date_t = datetime.strptime(date_str, '%Y-%m-%d %H:%M:%S');
print(date_t);
```

## json 模块

- json.load(file_object)
- json.dump(data, file_object, indent=4)
```python
import json

numbers = [1, 3, 4, 5, 6];
with open('./test.json', 'w') as f:
    json.dump(numbers, f);
    
with open('./test.json') as f:
    getdata = json.load(f);
print(getdata);
```

## csv 模块

- csv.reader(file_object)
- next(reader)

## unittest 模块

- 方法
   - setUp()
- 断言方法
   - assertEqual(a, b)
   - assertNotEqual(a, b)
   - assertTrue(a)
   - assertFalse(a)
   - assertIn(item, list)
   - assertNotIn(item, list)
```python
import unittest

import name_function

class NamesTestCase(unittest.TestCase):
    '''
    测试 name_function.py
    '''
    def setUp(self):
        self.first_name = 'jeans';
        self.last_name = 'jop';
        self.middle_name = 'step';
        
        self.combine_first_last_name = 'Jeans Jop';
        self.combine_first_middle_last_name = 'Jeans Step Jop';
    
    # 测试函数名称必须以 test_ 开始
    def test_first_last_name(self):
        formated_name = name_function.get_formated_name(self.first_name, self.last_name);
        self.assertEqual(formated_name, self.combine_first_last_name);
        
    def test_first_middle_last_name(self):
        formated_name = name_function.get_formated_name(self.first_name, self.last_name, self.middle_name);
        self.assertEqual(formated_name, self.combine_first_middle_last_name);

# 如果本文件作为主程序执行，__name__ 将被设置为 __main__
if __name__ == '__main__':
    unittest.main();
```

## matplotlib 模块
```shell
# 数据可视化分析工具之一
python3 -m pip install --user matplotlib
```
