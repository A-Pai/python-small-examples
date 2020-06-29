### 一、Python基础

`Python基础`主要总结Python常用内置函数；Python独有的语法特性、关键词`nonlocal`, ` global`等；内置数据结构包括：列表(list),  字典(dict),  集合(set),  元组(tuple) 以及相关的高级模块`collections`中的`Counter`,  `namedtuple`, `defaultdict`，`heapq`模块。目前共有`90`个小例子。

### 二、Python字符串和正则

字符串无所不在，字符串的处理也是最常见的操作。本章节将总结和字符串处理相关的一切操作。主要包括基本的字符串操作；高级字符串操作之正则。目前共有`25`个小例子

#### 91 反转字符串

```python
st="python"
#方法1
''.join(reversed(st))
#方法2
st[::-1]
```

#### 92 字符串切片操作

```python
字符串切片操作——查找替换3或5的倍数
In [1]:[str("java"[i%3*4:]+"python"[i%5*6:] or i) for i in range(1,15)]
OUT[1]:['1',
 '2',
 'java',
 '4',
 'python',
 'java',
 '7',
 '8',
 'java',
 'python',
 '11',
 'java',
 '13',
 '14']
```
#### 93 join串联字符串
```python
In [4]: mystr = ['1',
   ...:  '2',
   ...:  'java',
   ...:  '4',
   ...:  'python',
   ...:  'java',
   ...:  '7',
   ...:  '8',
   ...:  'java',
   ...:  'python',
   ...:  '11',
   ...:  'java',
   ...:  '13',
   ...:  '14']

In [5]: ','.join(mystr) #用逗号连接字符串
Out[5]: '1,2,java,4,python,java,7,8,java,python,11,java,13,14'
```

#### 94 字符串的字节长度

```python
def str_byte_len(mystr):
    return (len(mystr.encode('utf-8')))


str_byte_len('i love python')  # 13(个字节)
str_byte_len('字符')  # 6(个字节)
```



以下是正则部分

```python
import re
```

#### 95 查找第一个匹配串

```python
s = 'i love python very much'
pat = 'python' 
r = re.search(pat,s)
print(r.span()) #(7,13)
```

#### 96 查找所有1的索引

```python
s = '山东省潍坊市青州第1中学高三1班'
pat = '1'
r = re.finditer(pat,s)
for i in r:
    print(i)

# <re.Match object; span=(9, 10), match='1'>
# <re.Match object; span=(14, 15), match='1'>
```

#### 97 \d 匹配数字[0-9]
findall找出全部位置的所有匹配
```python
s = '一共20行代码运行时间13.59s'
pat = r'\d+' # +表示匹配数字(\d表示数字的通用字符)1次或多次
r = re.findall(pat,s)
print(r)
# ['20', '13', '59']
```

#### 98 匹配浮点数和整数

?表示前一个字符匹配0或1次
```python
s = '一共20行代码运行时间13.59s'
pat = r'\d+\.?\d+' # ?表示匹配小数点(\.)0次或1次，这种写法有个小bug，不能匹配到个位数的整数
r = re.findall(pat,s)
print(r)
# ['20', '13.59']

# 更好的写法：
pat = r'\d+\.\d+|\d+' # A|B，匹配A失败才匹配B
```
#### 99 ^匹配字符串的开头

```python
s = 'This module provides regular expression matching operations similar to those found in Perl'
pat = r'^[emrt]' # 查找以字符e,m,r或t开始的字符串
r = re.findall(pat,s)
print(r)
# [],因为字符串的开头是字符`T`，不在emrt匹配范围内，所以返回为空
IN [11]: s2 = 'email for me is guozhennianhua@163.com'
re.findall('^[emrt].*',s2)# 匹配以e,m,r,t开始的字符串，后面是多个任意字符
Out[11]: ['email for me is guozhennianhua@163.com']

```
#### 100 re.I 忽略大小写

```python
s = 'That'
pat = r't' 
r = re.findall(pat,s,re.I)
In [22]: r
Out[22]: ['T', 't']
```
#### 101 理解compile的作用
如果要做很多次匹配，可以先编译匹配串：
```python
import re
pat = re.compile('\W+') # \W 匹配不是数字和字母的字符
has_special_chars = pat.search('ed#2@edc') 
if has_special_chars:
    print(f'str contains special characters:{has_special_chars.group(0)}')

###输出结果: 
 # str contains special characters:#   

### 再次使用pat正则编译对象 做匹配
again_pattern = pat.findall('guozhennianhua@163.com')
if '@' in again_pattern:
    print('possibly it is an email')

```

#### 102 使用()捕获单词，不想带空格
使用`()`捕获
```python
s = 'This module provides regular expression matching operations similar to those found in Perl'
pat = r'\s([a-zA-Z]+)'  
r = re.findall(pat,s)
print(r) #['module', 'provides', 'regular', 'expression', 'matching', 'operations', 'similar', 'to', 'those', 'found', 'in', 'Perl']
```
看到提取单词中未包括第一个单词，使用`?`表示前面字符出现0次或1次，但是此字符还有表示贪心或非贪心匹配含义，使用时要谨慎。
```python
s = 'This module provides regular expression matching operations similar to those found in Perl'
pat = r'\s?([a-zA-Z]+)'  
r = re.findall(pat,s)
print(r) #['This', 'module', 'provides', 'regular', 'expression', 'matching', 'operations', 'similar', 'to', 'those', 'found', 'in', 'Perl']
```

#### 103 split分割单词
使用以上方法分割单词不是简洁的，仅仅是为了演示。分割单词最简单还是使用`split`函数。
```python
s = 'This module provides regular expression matching operations similar to those found in Perl'
pat = r'\s+'  
r = re.split(pat,s)
print(r) # ['This', 'module', 'provides', 'regular', 'expression', 'matching', 'operations', 'similar', 'to', 'those', 'found', 'in', 'Perl']

### 上面这句话也可直接使用str自带的split函数：
s.split(' ') #使用空格分隔

### 但是，对于风格符更加复杂的情况，split无能为力，只能使用正则

s = 'This,,,   module ; \t   provides|| regular ; '
words = re.split('[,\s;|]+',s)  #这样分隔出来，最后会有一个空字符串
words = [i for i in words if len(i)>0]
```

#### 104 match从字符串开始位置匹配
注意`match`,`search`等的不同：
1) match函数
```python
import re
### match
mystr = 'This'
pat = re.compile('hi')
pat.match(mystr) # None
pat.match(mystr,1) # 从位置1处开始匹配
Out[90]: <re.Match object; span=(1, 3), match='hi'>
```
2) search函数
search是从字符串的任意位置开始匹配
```python
In [91]: mystr = 'This'
    ...: pat = re.compile('hi')
    ...: pat.search(mystr)
Out[91]: <re.Match object; span=(1, 3), match='hi'>
```

#### 105 替换匹配的子串
`sub`函数实现对匹配子串的替换
```python
content="hello 12345, hello 456321"    
pat=re.compile(r'\d+') #要替换的部分
m=pat.sub("666",content)
print(m) # hello 666, hello 666
```

#### 106 贪心捕获
(.*)表示捕获任意多个字符，尽可能多的匹配字符
```python
content='<h>ddedadsad</h><div>graph</div>bb<div>math</div>cc'
pat=re.compile(r"<div>(.*)</div>")  #贪婪模式
m=pat.findall(content)
print(m) #匹配结果为： ['graph</div>bb<div>math']
```
#### 107 非贪心捕获
仅添加一个问号(`?`)，得到结果完全不同，这是非贪心匹配，通过这个例子体会贪心和非贪心的匹配的不同。
```python
content='<h>ddedadsad</h><div>graph</div>bb<div>math</div>cc'
pat=re.compile(r"<div>(.*?)</div>")
m=pat.findall(content)
print(m) # ['graph', 'math']
```
非贪心捕获，见好就收。

#### 108 常用元字符总结

    . 匹配任意字符  
    ^ 匹配字符串开始位置 
    $ 匹配字符串中结束的位置 
    * 前面的原子重复0次、1次、多次 
    ? 前面的原子重复0次或者1次 
    + 前面的原子重复1次或多次
    {n} 前面的原子出现了 n 次
    {n,} 前面的原子至少出现 n 次
    {n,m} 前面的原子出现次数介于 n-m 之间
    ( ) 分组,需要输出的部分

#### 109 常用通用字符总结

    \s  匹配空白字符 
    \w  匹配任意字母/数字/下划线 
    \W  和小写 w 相反，匹配任意字母/数字/下划线以外的字符
    \d  匹配十进制数字
    \D  匹配除了十进制数以外的值 
    [0-9]  匹配一个0-9之间的数字
    [a-z]  匹配小写英文字母
    [A-Z]  匹配大写英文字母

#### 110 密码安全检查

密码安全要求：1)要求密码为6到20位; 2)密码只包含英文字母和数字

```python
pat = re.compile(r'\w{6,20}') # 这是错误的，因为\w通配符匹配的是字母，数字和下划线，题目要求不能含有下划线
# 使用最稳的方法：\da-zA-Z满足`密码只包含英文字母和数字`
pat = re.compile(r'[\da-zA-Z]{6,20}')
```
选用最保险的`fullmatch`方法，查看是否整个字符串都匹配：
```python
pat.fullmatch('qaz12') # 返回 None, 长度小于6
pat.fullmatch('qaz12wsxedcrfvtgb67890942234343434') # None 长度大于22
pat.fullmatch('qaz_231') # None 含有下划线
pat.fullmatch('n0passw0Rd')
Out[4]: <re.Match object; span=(0, 10), match='n0passw0Rd'>
```

#### 111 爬取百度首页标题

```python
import re
from urllib import request

#爬虫爬取百度首页内容
data=request.urlopen("http://www.baidu.com/").read().decode()

#分析网页,确定正则表达式
pat=r'<title>(.*?)</title>'

result=re.search(pat,data)
print(result) <re.Match object; span=(1358, 1382), match='<title>百度一下，你就知道</title>'>

result.group() # 百度一下，你就知道
```

#### 112 批量转化为驼峰格式(Camel)

数据库字段名批量转化为驼峰格式

分析过程

```python
# 用到的正则串讲解
# \s 指匹配： [ \t\n\r\f\v]
# A|B：表示匹配A串或B串
# re.sub(pattern, newchar, string): 
# substitue代替，用newchar字符替代与pattern匹配的字符所有.
```



```python
# title(): 转化为大写，例子：
# 'Hello world'.title() # 'Hello World'
```



```python
# print(re.sub(r"\s|_|", "", "He llo_worl\td"))
s = re.sub(r"(\s|_|-)+", " ",
           'some_database_field_name').title().replace(" ", "")  
#结果： SomeDatabaseFieldName
```



```python
# 可以看到此时的第一个字符为大写，需要转化为小写
s = s[0].lower()+s[1:]  # 最终结果
```

 

整理以上分析得到如下代码：

```python
import re
def camel(s):
    s = re.sub(r"(\s|_|-)+", " ", s).title().replace(" ", "")
    return s[0].lower() + s[1:]

# 批量转化
def batch_camel(slist):
    return [camel(s) for s in slist]
```

测试结果：

```python
s = batch_camel(['student_id', 'student\tname', 'student-add'])
print(s)
# 结果
['studentId', 'studentName', 'studentAdd']
```



#### 113 str1是否为str2的permutation

排序词(permutation)：两个字符串含有相同字符，但字符顺序不同。

```python
from collections import defaultdict


def is_permutation(str1, str2):
    if str1 is None or str2 is None:
        return False
    if len(str1) != len(str2):
        return False
    unq_s1 = defaultdict(int)
    unq_s2 = defaultdict(int)
    for c1 in str1:
        unq_s1[c1] += 1
    for c2 in str2:
        unq_s2[c2] += 1

    return unq_s1 == unq_s2
```

这个小例子，使用python内置的`defaultdict`，默认类型初始化为`int`，计数默次数都为0. 这个解法本质是 `hash map lookup`

统计出的两个defaultdict：unq_s1，unq_s2，如果相等，就表明str1、 str2互为排序词。

下面测试：

```python
r = is_permutation('nice', 'cine')
print(r)  # True

r = is_permutation('', '')
print(r)  # True

r = is_permutation('', None)
print(r)  # False

r = is_permutation('work', 'woo')
print(r)  # False

```

以上就是使用defaultdict的小例子，希望对读者朋友理解此类型有帮助。

#### 114 str1是否由str2旋转而来

`stringbook`旋转后得到`bookstring`,写一段代码验证`str1`是否为`str2`旋转得到。

**思路**

转化为判断：`str1`是否为`str2+str2`的子串

```python
def is_rotation(s1: str, s2: str) -> bool:
    if s1 is None or s2 is None:
        return False
    if len(s1) != len(s2):
        return False

    def is_substring(s1: str, s2: str) -> bool:
        return s1 in s2
    return is_substring(s1, s2 + s2)
```

**测试**

```python
r = is_rotation('stringbook', 'bookstring')
print(r)  # True

r = is_rotation('greatman', 'maneatgr')
print(r)  # False
```

#### 115 正浮点数

从一系列字符串中，挑选出所有正浮点数。

该怎么办？

玩玩正则表达式，用正则搞它！

关键是，正则表达式该怎么写呢？

有了！

`^[1-9]\d*\.\d*$`

`^` 表示字符串开始

`[1-9]` 表示数字1,2,3,4,5,6,7,8,9

`^[1-9]` 连起来表示以数字 `1-9` 作为开头

`\d` 表示一位 `0-9` 的数字

`*` 表示前一位字符出现 0 次，1 次或多次

`\d*` 表示数字出现 0 次，1 次或多次 

`\.` 表示小数点

`\$` 表示字符串以前一位的字符结束

`^[1-9]\d*\.\d*$` 连起来就求出所有大于 1.0 的正浮点数。

那 0.0 到 1.0 之间的正浮点数，怎么求，干嘛不直接汇总到上面的正则表达式中呢？

这样写不行吗：`^[0-9]\d*\.\d*$`

OK!

那我们立即测试下呗

```python
In [85]: import re

In [87]: recom = re.compile(r'^[0-9]\d*\.\d*$')

In [88]: recom.match('000.2')
Out[88]: <re.Match object; span=(0, 5), match='000.2'>
```

结果显示，正则表达式 `^[0-9]\d*\.\d*$` 竟然匹配到 `000.2 `，认为它是一个正浮点数~~~！！！！

晕！！！！！！

所以知道为啥要先匹配大于 1.0 的浮点数了吧！

如果能写出这个正则表达式，再写另一部分就不困难了！

0.0 到 1.0 间的浮点数：`^0\.\d*[1-9]\d*$`

两个式子连接起来就是最终的结果：

`^[1-9]\d*\.\d*|0\.\d*[1-9]\d*$`

如果还是看不懂，看看下面的正则分布剖析图吧：

<img src="./img/image-20200220225043053.png" width="50%"/>

### 三、Python文件、日期和多线程

Python文件IO操作涉及文件读写操作，获取文件`后缀名`，修改后缀名，获取文件修改时间，`压缩`文件，`加密`文件等操作。

Python日期章节，由表示大日期的`calendar`, `date`模块，逐渐过渡到表示时间刻度更小的模块：`datetime`, `time`模块，按照此逻辑展开。

Python`多线程`希望透过5个小例子，帮助你对多线程模型编程本质有些更清晰的认识。

一共总结最常用的`26`个关于文件和时间处理模块的例子。

#### 116 获取后缀名

```python
import os
file_ext = os.path.splitext('./data/py/test.py')
front,ext = file_ext
In [5]: front
Out[5]: './data/py/test'

In [6]: ext
Out[6]: '.py'
```

#### 117 文件读操作

```python
import os
# 创建文件夹

def mkdir(path):
    isexists = os.path.exists(path)
    if not isexists:
        os.mkdir(path)
# 读取文件信息

def openfile(filename):
    f = open(filename)
    fllist = f.read()
    f.close()
    return fllist  # 返回读取内容
```

#### 118  文件写操作

```python
# 写入文件信息
# example1
# w写入，如果文件存在，则清空内容后写入，不存在则创建
f = open(r"./data/test.txt", "w", encoding="utf-8")
print(f.write("测试文件写入"))
f.close

# example2
# a写入，文件存在，则在文件内容后追加写入，不存在则创建
f = open(r"./data/test.txt", "a", encoding="utf-8")
print(f.write("测试文件写入"))
f.close

# example3
# with关键字系统会自动关闭文件和处理异常
with open(r"./data/test.txt", "w") as f:
    f.write("hello world!")
```

#### 119 路径中的文件名

```python
In [11]: import os
    ...: file_ext = os.path.split('./data/py/test.py')
    ...: ipath,ifile = file_ext
    ...:

In [12]: ipath
Out[12]: './data/py'

In [13]: ifile
Out[13]: 'test.py'
```

#### 120 批量修改文件后缀

**批量修改文件后缀**

本例子使用Python的`os`模块和 `argparse`模块，将工作目录`work_dir`下所有后缀名为`old_ext`的文件修改为后缀名为`new_ext`

通过本例子，大家将会大概清楚`argparse`模块的主要用法。

导入模块

```python
import argparse
import os
```

定义脚本参数

```python
def get_parser():
    parser = argparse.ArgumentParser(
        description='工作目录中文件后缀名修改')
    parser.add_argument('work_dir', metavar='WORK_DIR', type=str, nargs=1,
                        help='修改后缀名的文件目录')
    parser.add_argument('old_ext', metavar='OLD_EXT',
                        type=str, nargs=1, help='原来的后缀')
    parser.add_argument('new_ext', metavar='NEW_EXT',
                        type=str, nargs=1, help='新的后缀')
    return parser
```

后缀名批量修改

```python
def batch_rename(work_dir, old_ext, new_ext):
    """
    传递当前目录，原来后缀名，新的后缀名后，批量重命名后缀
    """
    for filename in os.listdir(work_dir):
        # 获取得到文件后缀
        split_file = os.path.splitext(filename)
        file_ext = split_file[1]
        # 定位后缀名为old_ext 的文件
        if old_ext == file_ext:
            # 修改后文件的完整名称
            newfile = split_file[0] + new_ext
            # 实现重命名操作
            os.rename(
                os.path.join(work_dir, filename),
                os.path.join(work_dir, newfile)
            )
    print("完成重命名")
    print(os.listdir(work_dir))
```

实现Main

```python
def main():
    """
    main函数
    """
    # 命令行参数
    parser = get_parser()
    args = vars(parser.parse_args())
    # 从命令行参数中依次解析出参数
    work_dir = args['work_dir'][0]
    old_ext = args['old_ext'][0]
    if old_ext[0] != '.':
        old_ext = '.' + old_ext
    new_ext = args['new_ext'][0]
    if new_ext[0] != '.':
        new_ext = '.' + new_ext

    batch_rename(work_dir, old_ext, new_ext)
```



#### 121 xls批量转换成xlsx

```python
import os


def xls_to_xlsx(work_dir):
    """
    传递当前目录，原来后缀名，新的后缀名后，批量重命名后缀
    """
    old_ext, new_ext = '.xls', '.xlsx'
    for filename in os.listdir(work_dir):
        # 获取得到文件后缀
        split_file = os.path.splitext(filename)
        file_ext = split_file[1]
        # 定位后缀名为old_ext 的文件
        if old_ext == file_ext:
            # 修改后文件的完整名称
            newfile = split_file[0] + new_ext
            # 实现重命名操作
            os.rename(
                os.path.join(work_dir, filename),
                os.path.join(work_dir, newfile)
            )
    print("完成重命名")
    print(os.listdir(work_dir))


xls_to_xlsx('./data')

# 输出结果：
# ['cut_words.csv', 'email_list.xlsx', 'email_test.docx', 'email_test.jpg', 'email_test.xlsx', 'geo_data.png', 'geo_data.xlsx',
'iotest.txt', 'pyside2.md', 'PySimpleGUI-4.7.1-py3-none-any.whl', 'test.txt', 'test_excel.xlsx', 'ziptest', 'ziptest.zip']
```



#### 122 定制文件不同行


比较两个文件在哪些行内容不同，返回这些行的编号，行号编号从1开始。

定义统计文件行数的函数

```python
# 统计文件个数
    def statLineCnt(statfile):
        print('文件名：'+statfile)
        cnt = 0
        with open(statfile, encoding='utf-8') as f:
            while f.readline():
                cnt += 1
            return cnt
```



统计文件不同之处的子函数：

```python
# more表示含有更多行数的文件
        def diff(more, cnt, less):
            difflist = []
            with open(less, encoding='utf-8') as l:
                with open(more, encoding='utf-8') as m:
                    lines = l.readlines()
                    for i, line in enumerate(lines):
                        if line.strip() != m.readline().strip():
                            difflist.append(i)
            if cnt - i > 1:
                difflist.extend(range(i + 1, cnt))
            return [no+1 for no in difflist]
```



主函数：

```python
# 返回的结果行号从1开始
# list表示fileA和fileB不同的行的编号

def file_diff_line_nos(fileA, fileB):
    try:
        cntA = statLineCnt(fileA)
        cntB = statLineCnt(fileB)
        if cntA > cntB:
            return diff(fileA, cntA, fileB)
        return diff(fileB, cntB, fileA)

    except Exception as e:
        print(e)
```

比较两个文件A和B，拿相对较短的文件去比较，过滤行后的换行符`\n`和空格。

暂未考虑某个文件最后可能有的多行空行等特殊情况

使用`file_diff_line_nos` 函数：

```python
if __name__ == '__main__':
    import os
    print(os.getcwd())

    '''
    例子：
    fileA = "'hello world!!!!''\
            'nice to meet you'\
            'yes'\
            'no1'\
            'jack'"
    fileB = "'hello world!!!!''\
            'nice to meet you'\
            'yes' "
    '''
    diff = file_diff_line_nos('./testdir/a.txt', './testdir/b.txt')
    print(diff)  # [4, 5]
```

关于文件比较的，实际上，在Python中有对应模块`difflib` , 提供更多其他格式的文件更详细的比较，大家可参考：

> https://docs.python.org/3/library/difflib.html?highlight=difflib#module-difflib



#### 123 获取指定后缀名的文件

```python
import os

def find_file(work_dir,extension='jpg'):
    lst = []
    for filename in os.listdir(work_dir):
        print(filename)
        splits = os.path.splitext(filename)
        ext = splits[1] # 拿到扩展名
        if ext == '.'+extension:
            lst.append(filename)
    return lst

r = find_file('.','md') 
print(r) # 返回所有目录下的md文件
```


#### 124 批量获取文件修改时间

```python
# 获取目录下文件的修改时间
import os
from datetime import datetime

print(f"当前时间：{datetime.now().strftime('%Y-%m-%d %H:%M:%S')}")

def get_modify_time(indir):
    for root, _, files in os.walk(indir):  # 循环D:\works目录和子目录
        for file in files:
            absfile = os.path.join(root, file)
            modtime = datetime.fromtimestamp(os.path.getmtime(absfile))
            now = datetime.now()
            difftime = now-modtime
            if difftime.days < 20:  # 条件筛选超过指定时间的文件
                print(f"""{absfile}
                    修改时间[{modtime.strftime('%Y-%m-%d %H:%M:%S')}]
                    距今[{difftime.days:3d}天{difftime.seconds//3600:2d}时{difftime.seconds%3600//60:2d}]"""
                      )  # 打印相关信息


get_modify_time('./data')
```

    打印效果：
    当前时间：2019-12-22 16:38:53
    ./data\cut_words.csv
                        修改时间[2019-12-21 10:34:15]
                        距今[  1天 6时 4]
    当前时间：2019-12-22 16:38:53
    ./data\cut_words.csv
                        修改时间[2019-12-21 10:34:15]
                        距今[  1天 6时 4]
    ./data\email_test.docx
                        修改时间[2019-12-03 07:46:29]
                        距今[ 19天 8时52]
    ./data\email_test.jpg
                        修改时间[2019-12-03 07:46:29]
                        距今[ 19天 8时52]
    ./data\email_test.xlsx
                        修改时间[2019-12-03 07:46:29]
                        距今[ 19天 8时52]
    ./data\iotest.txt
                        修改时间[2019-12-13 08:23:18]
                        距今[  9天 8时15]
    ./data\pyside2.md
                        修改时间[2019-12-05 08:17:22]
                        距今[ 17天 8时21]
    ./data\PySimpleGUI-4.7.1-py3-none-any.whl
                        修改时间[2019-12-05 00:25:47]
                        距今[ 17天16时13]

#### 125 批量压缩文件


```python
import zipfile  # 导入zipfile,这个是用来做压缩和解压的Python模块；
import os
import time


def batch_zip(start_dir):
    start_dir = start_dir  # 要压缩的文件夹路径
    file_news = start_dir + '.zip'  # 压缩后文件夹的名字

    z = zipfile.ZipFile(file_news, 'w', zipfile.ZIP_DEFLATED)
    for dir_path, dir_names, file_names in os.walk(start_dir):
        # 这一句很重要，不replace的话，就从根目录开始复制
        f_path = dir_path.replace(start_dir, '')
        f_path = f_path and f_path + os.sep  # 实现当前文件夹以及包含的所有文件的压缩
        for filename in file_names:
            z.write(os.path.join(dir_path, filename), f_path + filename)
    z.close()
    return file_news


batch_zip('./data/ziptest')


```

#### 126 32位加密

```python
import hashlib
# 对字符串s实现32位加密


def hash_cry32(s):
    m = hashlib.md5()
    m.update((str(s).encode('utf-8')))
    return m.hexdigest()


print(hash_cry32(1))  # c4ca4238a0b923820dcc509a6f75849b
print(hash_cry32('hello'))  # 5d41402abc4b2a76b9719d911017c592
```

#### 127 年的日历图

```python
import calendar
from datetime import date
mydate = date.today()
year_calendar_str = calendar.calendar(2019)
print(f"{mydate.year}年的日历图：{year_calendar_str}\n")
```

打印结果：

```python
2019

      January                   February                   March
Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su
    1  2  3  4  5  6                   1  2  3                   1  2  3
 7  8  9 10 11 12 13       4  5  6  7  8  9 10       4  5  6  7  8  9 10
14 15 16 17 18 19 20      11 12 13 14 15 16 17      11 12 13 14 15 16 17
21 22 23 24 25 26 27      18 19 20 21 22 23 24      18 19 20 21 22 23 24
28 29 30 31               25 26 27 28               25 26 27 28 29 30 31

       April                      May                       June
Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su
 1  2  3  4  5  6  7             1  2  3  4  5                      1  2
 8  9 10 11 12 13 14       6  7  8  9 10 11 12       3  4  5  6  7  8  9
15 16 17 18 19 20 21      13 14 15 16 17 18 19      10 11 12 13 14 15 16
22 23 24 25 26 27 28      20 21 22 23 24 25 26      17 18 19 20 21 22 23
29 30                     27 28 29 30 31            24 25 26 27 28 29 30

        July                     August                  September
Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su
 1  2  3  4  5  6  7                1  2  3  4                         1
 8  9 10 11 12 13 14       5  6  7  8  9 10 11       2  3  4  5  6  7  8
15 16 17 18 19 20 21      12 13 14 15 16 17 18       9 10 11 12 13 14 15
22 23 24 25 26 27 28      19 20 21 22 23 24 25      16 17 18 19 20 21 22
29 30 31                  26 27 28 29 30 31         23 24 25 26 27 28 29
                                                    30

      October                   November                  December
Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su
    1  2  3  4  5  6                   1  2  3                         1
 7  8  9 10 11 12 13       4  5  6  7  8  9 10       2  3  4  5  6  7  8
14 15 16 17 18 19 20      11 12 13 14 15 16 17       9 10 11 12 13 14 15
21 22 23 24 25 26 27      18 19 20 21 22 23 24      16 17 18 19 20 21 22
28 29 30 31               25 26 27 28 29 30         23 24 25 26 27 28 29
                                                    30 31
```

#### 128 判断是否为闰年

```python
import calendar
from datetime import date

mydate = date.today()
is_leap = calendar.isleap(mydate.year)
print_leap_str = "%s年是闰年" if is_leap else "%s年不是闰年\n"
print(print_leap_str % mydate.year)
```

打印结果：

```python
2019年不是闰年
```

#### 129 月的日历图

```python
import calendar
from datetime import date

mydate = date.today()
month_calendar_str = calendar.month(mydate.year, mydate.month)

print(f"{mydate.year}年-{mydate.month}月的日历图：{month_calendar_str}\n")
```

打印结果：

```python
December 2019
Mo Tu We Th Fr Sa Su
                   1
 2  3  4  5  6  7  8
 9 10 11 12 13 14 15
16 17 18 19 20 21 22
23 24 25 26 27 28 29
30 31
```

#### 130 月有几天

```python
import calendar
from datetime import date

mydate = date.today()
weekday, days = calendar.monthrange(mydate.year, mydate.month)
print(f'{mydate.year}年-{mydate.month}月的第一天是那一周的第{weekday}天\n')
print(f'{mydate.year}年-{mydate.month}月共有{days}天\n')
```

打印结果：

```python
2019年-12月的第一天是那一周的第6天

2019年-12月共有31天
```



#### 131 月第一天

```python
from datetime import date
mydate = date.today()
month_first_day = date(mydate.year, mydate.month, 1)
print(f"当月第一天:{month_first_day}\n")
```

打印结果：

```python
# 当月第一天:2019-12-01
```



#### 131 月最后一天

```python
from datetime import date
import calendar
mydate = date.today()
_, days = calendar.monthrange(mydate.year, mydate.month)
month_last_day = date(mydate.year, mydate.month, days)
print(f"当月最后一天:{month_last_day}\n")
```

打印结果：

```python
当月最后一天:2019-12-31
```



#### 132 获取当前时间

```python
from datetime import date, datetime
from time import localtime

today_date = date.today()
print(today_date)  # 2019-12-22

today_time = datetime.today()
print(today_time)  # 2019-12-22 18:02:33.398894

local_time = localtime()
print(strftime("%Y-%m-%d %H:%M:%S", local_time))  # 转化为定制的格式 2019-12-22 18:13:41
```



#### 133 字符时间转时间

```python
from time import strptime

# parse str time to struct time
struct_time = strptime('2019-12-22 10:10:08', "%Y-%m-%d %H:%M:%S")
print(struct_time) # struct_time类型就是time中的一个类

# time.struct_time(tm_year=2019, tm_mon=12, tm_mday=22, tm_hour=10, tm_min=10, tm_sec=8, tm_wday=6, tm_yday=356, tm_isdst=-1)
```



#### 134 时间转字符时间

```python
from time import strftime, strptime, localtime

In [2]: print(localtime()) #这是输入的时间
Out[2]: time.struct_time(tm_year=2019, tm_mon=12, tm_mday=22, tm_hour=18, tm_min=24, tm_sec=56, tm_wday=6, tm_yday=356, tm_isdst=0)

print(strftime("%m-%d-%Y %H:%M:%S", localtime()))  # 转化为定制的格式
# 这是字符串表示的时间：   12-22-2019 18:26:21
```



#### 135 默认启动主线程

一般的，程序默认执行只在一个线程，这个线程称为主线程，例子演示如下：

导入线程相关的模块 `threading`:

```python
import threading
```

threading的类方法 `current_thread()`返回当前线程：

```python
t = threading.current_thread()
print(t) # <_MainThread(MainThread, started 139908235814720)>
```

所以，验证了程序默认是在`MainThead`中执行。

`t.getName()`获得这个线程的名字，其他常用方法，`getName()`获得线程`id`,`isAlive()`判断线程是否存活等。

```python
print(t.getName()) # MainThread
print(t.ident) # 139908235814720
print(t.isAlive()) # True
```

以上这些仅是介绍多线程的`背景知识`，因为到目前为止，我们有且仅有一个"干活"的主线程

#### 136 创建线程

创建一个线程：

```python
my_thread = threading.Thread()
```

创建一个名称为`my_thread`的线程：

```python
my_thread = threading.Thread(name='my_thread')
```

创建线程的目的是告诉它帮助我们做些什么，做些什么通过参数`target`传入，参数类型为`callable`，函数就是可调用的：

```python
def print_i(i):
    print('打印i:%d'%(i,))
my_thread = threading.Thread(target=print_i,args=(1,))
```

`my_thread`线程已经全副武装，但是我们得按下发射按钮，启动start()，它才开始真正起飞。

```python
my_thread().start()
```

打印结果如下，其中`args`指定函数`print_i`需要的参数i，类型为元祖。

```python
打印i:1
```

至此，多线程相关的核心知识点，已经总结完毕。但是，仅仅知道这些，还不够！光纸上谈兵，当然远远不够。

接下来，聊聊应用多线程编程，最本质的一些东西。

**3 交替获得CPU时间片**

为了更好解释，假定计算机是单核的，尽管对于`cpython`，这个假定有些多余。

开辟3个线程，装到`threads`中:

```python
import time
from datetime import datetime
import threading


def print_time():
    for _ in range(5): # 在每个线程中打印5次
        time.sleep(0.1) # 模拟打印前的相关处理逻辑
        print('当前线程%s,打印结束时间为:%s'%(threading.current_thread().getName(),datetime.today()))


threads = [threading.Thread(name='t%d'%(i,),target=print_time) for i in range(3)]
```

启动3个线程：

```python
[t.start() for t in threads]
```

打印结果如下，`t0`,`t1`,`t2`三个线程，根据操作系统的调度算法，轮询获得CPU时间片，注意观察，`t2`线程可能被连续调度，从而获得时间片。

```python
当前线程t0,打印结束时间为:2020-01-12 02:27:15.705235
当前线程t1,打印结束时间为:2020-01-12 02:27:15.705402
当前线程t2,打印结束时间为:2020-01-12 02:27:15.705687
当前线程t0,打印结束时间为:2020-01-12 02:27:15.805767
当前线程t1,打印结束时间为:2020-01-12 02:27:15.805886
当前线程t2,打印结束时间为:2020-01-12 02:27:15.806044
当前线程t0,打印结束时间为:2020-01-12 02:27:15.906200
当前线程t2,打印结束时间为:2020-01-12 02:27:15.906320
当前线程t1,打印结束时间为:2020-01-12 02:27:15.906433
当前线程t0,打印结束时间为:2020-01-12 02:27:16.006581
当前线程t1,打印结束时间为:2020-01-12 02:27:16.006766
当前线程t2,打印结束时间为:2020-01-12 02:27:16.007006
当前线程t2,打印结束时间为:2020-01-12 02:27:16.107564
当前线程t0,打印结束时间为:2020-01-12 02:27:16.107290
当前线程t1,打印结束时间为:2020-01-12 02:27:16.107741
```

#### 137 多线程抢夺同一个变量

多线程编程，存在抢夺同一个变量的问题。

比如下面例子，创建的10个线程同时竞争全局变量`a`:


```python
import threading


a = 0
def add1():
    global a    
    a += 1
    print('%s  adds a to 1: %d'%(threading.current_thread().getName(),a))
    
threads = [threading.Thread(name='t%d'%(i,),target=add1) for i in range(10)]
[t.start() for t in threads]
```

执行结果：

```python
t0  adds a to 1: 1
t1  adds a to 1: 2
t2  adds a to 1: 3
t3  adds a to 1: 4
t4  adds a to 1: 5
t5  adds a to 1: 6
t6  adds a to 1: 7
t7  adds a to 1: 8
t8  adds a to 1: 9
t9  adds a to 1: 10
```

结果一切正常，每个线程执行一次，把`a`的值加1，最后`a` 变为10，一切正常。

运行上面代码十几遍，一切也都正常。

所以，我们能下结论：这段代码是线程安全的吗？

NO！

多线程中，只要存在同时读取和修改一个全局变量的情况，如果不采取其他措施，就一定不是线程安全的。

尽管，有时，某些情况的资源竞争，暴露出问题的概率`极低极低`：

本例中，如果线程0 在修改a后，其他某些线程还是get到的是没有修改前的值，就会暴露问题。



但是在本例中，`a = a + 1`这种修改操作，花费的时间太短了，短到我们无法想象。所以，线程间轮询执行时，都能get到最新的a值。所以，暴露问题的概率就变得微乎其微。

#### 138 代码稍作改动，叫问题暴露出来

只要弄明白问题暴露的原因，叫问题出现还是不困难的。

想象数据库的写入操作，一般需要耗费我们可以感知的时间。

为了模拟这个写入动作，简化期间，我们只需要延长修改变量`a`的时间，问题很容易就会还原出来。

```python
import threading
import time


a = 0
def add1():
    global a    
    tmp = a + 1
    time.sleep(0.2) # 延时0.2秒，模拟写入所需时间
    a = tmp
    print('%s  adds a to 1: %d'%(threading.current_thread().getName(),a))
    
threads = [threading.Thread(name='t%d'%(i,),target=add1) for i in range(10)]
[t.start() for t in threads]
```

重新运行代码，只需一次，问题立马完全暴露，结果如下：

```python
t0  adds a to 1: 1
t1  adds a to 1: 1
t2  adds a to 1: 1
t3  adds a to 1: 1
t4  adds a to 1: 1
t5  adds a to 1: 1
t7  adds a to 1: 1
t6  adds a to 1: 1
t8  adds a to 1: 1
t9  adds a to 1: 1
```

看到，10个线程全部运行后，`a`的值只相当于一个线程执行的结果。

下面分析，为什么会出现上面的结果：

这是一个很有说服力的例子，因为在修改a前，有0.2秒的休眠时间，某个线程延时后，CPU立即分配计算资源给其他线程。直到分配给所有线程后，根据结果反映出，0.2秒的休眠时长还没耗尽，这样每个线程get到的a值都是0，所以才出现上面的结果。



以上最核心的三行代码：

```python
tmp = a + 1
time.sleep(0.2) # 延时0.2秒，模拟写入所需时间
a = tmp
```

#### 139 加上一把锁，避免以上情况出现

知道问题出现的原因后，要想修复问题，也没那么复杂。

通过python中提供的锁机制，某段代码只能单线程执行时，上锁，其他线程等待，直到释放锁后，其他线程再争锁，执行代码，释放锁，重复以上。

创建一把锁`locka`:

```python
import threading
import time


locka = threading.Lock()
```

通过 `locka.acquire()` 获得锁，通过`locka.release()`释放锁，它们之间的这些代码，只能单线程执行。

```python
a = 0
def add1():
    global a    
    try:
        locka.acquire() # 获得锁
        tmp = a + 1
        time.sleep(0.2) # 延时0.2秒，模拟写入所需时间
        a = tmp
    finally:
        locka.release() # 释放锁
    print('%s  adds a to 1: %d'%(threading.current_thread().getName(),a))
    
threads = [threading.Thread(name='t%d'%(i,),target=add1) for i in range(10)]
[t.start() for t in threads]
```

执行结果如下：

```python
t0  adds a to 1: 1
t1  adds a to 1: 2
t2  adds a to 1: 3
t3  adds a to 1: 4
t4  adds a to 1: 5
t5  adds a to 1: 6
t6  adds a to 1: 7
t7  adds a to 1: 8
t8  adds a to 1: 9
t9  adds a to 1: 10
```

一起正常，其实这已经是单线程顺序执行了，就本例子而言，已经失去多线程的价值，并且还带来了因为线程创建开销，浪费时间的副作用。

程序中只有一把锁，通过 `try...finally`还能确保不发生死锁。但是，当程序中启用多把锁，还是很容易发生死锁。

注意使用场合，避免死锁，是我们在使用多线程开发时需要注意的一些问题。

#### 140 1 分钟掌握 time 模块

time 模块提供时间相关的类和函数

记住一个类：`struct_time`，9 个整数组成的元组

记住下面 5 个最常用函数

首先导入`time`模块

```python
import time
```

**1 此时此刻时间浮点数**

```python
In [58]: seconds = time.time()
In [60]: seconds
Out[60]: 1582341559.0950701
```

**2 时间数组**

```python
In [61]: local_time = time.localtime(seconds)

In [62]: local_time
Out[62]: time.struct_time(tm_year=2020, tm_mon=2, tm_mday=22, tm_hour=11, tm_min=19, tm_sec=19, tm_wday=5, tm_yday=53, tm_isdst=0)
```

**3 时间字符串**

`time.asctime` 语义： `as convert time`

```python
In [63]: str_time = time.asctime(local_time)

In [64]: str_time
Out[64]: 'Sat Feb 22 11:19:19 2020'
```

**4 格式化时间字符串**

`time.strftime` 语义： `string format time`

```python
In [65]: format_time = time.strftime('%Y-%m-%d %H:%M:%S',local_time)

In [66]: format_time
Out[66]: '2020-02-22 11:19:19'
```

**5 字符时间转时间数组**

```python
In [68]: str_to_struct = time.strptime(format_time,'%Y-%m-%d %H:%M:%S')

In [69]: str_to_struct
Out[69]: time.struct_time(tm_year=2020, tm_mon=2, tm_mday=22, tm_hour=11, tm_min=19, tm_sec=19, tm_wday=5, tm_yday=53, tm_isdst=-1)
```

最后再记住常用字符串格式

**常用字符串格式**

%m：月

%M: 分钟

```markdown
    %Y  Year with century as a decimal number.
    %m  Month as a decimal number [01,12].
    %d  Day of the month as a decimal number [01,31].
    %H  Hour (24-hour clock) as a decimal number [00,23].
    %M  Minute as a decimal number [00,59].
    %S  Second as a decimal number [00,61].
    %z  Time zone offset from UTC.
    %a  Locale's abbreviated weekday name.
    %A  Locale's full weekday name.
    %b  Locale's abbreviated month name.
```

#### 141 4G 内存处理 10G 大小的文件

4G 内存处理 10G 大小的文件，单机怎么做？

下面的讨论基于的假定：可以单独处理一行数据，行间数据相关性为零。

方法一：

仅使用 Python 内置模板，逐行读取到内存。

使用 yield，好处是解耦读取操作和处理操作：

```python
def python_read(filename):
    with open(filename,'r',encoding='utf-8') as f:
        while True:
            line = f.readline()
            if not line:
                return
            yield line
```

以上每次读取一行，逐行迭代，逐行处理数据

```python
if __name__ == '__main__':
    g = python_read('./data/movies.dat')
    for c in g:
        print(c)
        # process c
```

方法二：

方法一有缺点，逐行读入，频繁的 IO 操作拖累处理效率。是否有一次 IO ，读取多行的方法？

`pandas` 包 `read_csv` 函数，参数有 38 个之多，功能非常强大。

关于单机处理大文件，`read_csv` 的 `chunksize` 参数能做到，设置为 `5`， 意味着一次读取 5 行。

```python
def pandas_read(filename,sep=',',chunksize=5):
    reader = pd.read_csv(filename,sep,chunksize=chunksize)
    while True:
        try:
            yield reader.get_chunk()
        except StopIteration:
            print('---Done---')
            break
```

使用如同方法一：
```python
if __name__ == '__main__':
    g = pandas_read('./data/movies.dat',sep="::")
    for c in g:
        print(c)
        # process c
```

以上就是单机处理大文件的两个方法，推荐使用方法二，更加灵活。除了工作中会用到，面试中也有时被问到。

### 四、Python三大利器

Python中的三大利器包括：`迭代器`，`生成器`，`装饰器`，利用好它们才能开发出最高性能的Python程序，涉及到的内置模块 `itertools`提供迭代器相关的操作。此部分收录有意思的例子共计`15`例。


#### 142 寻找第n次出现位置

```python
def search_n(s, c, n):
    size = 0
    for i, x in enumerate(s):
        if x == c:
            size += 1
        if size == n:
            return i
    return -1



print(search_n("fdasadfadf", "a", 3))# 结果为7，正确
print(search_n("fdasadfadf", "a", 30))# 结果为-1，正确
```


#### 143 斐波那契数列前n项

```python
def fibonacci(n):
    a, b = 1, 1
    for _ in range(n):
        yield a
        a, b = b, a + b


list(fibonacci(5))  # [1, 1, 2, 3, 5]
```

#### 144 找出所有重复元素

```python
from collections import Counter


def find_all_duplicates(lst):
    c = Counter(lst)
    return list(filter(lambda k: c[k] > 1, c))


find_all_duplicates([1, 2, 2, 3, 3, 3])  # [2,3]
```

#### 145 联合统计次数
Counter对象间可以做数学运算

```python
from collections import Counter
a = ['apple', 'orange', 'computer', 'orange']
b = ['computer', 'orange']

ca = Counter(a)
cb = Counter(b)
#Counter对象间可以做数学运算
ca + cb  # Counter({'orange': 3, 'computer': 2, 'apple': 1})


# 进一步抽象，实现多个列表内元素的个数统计


def sumc(*c):
    if (len(c) < 1):
        return
    mapc = map(Counter, c)
    s = Counter([])
    for ic in mapc: # ic 是一个Counter对象
        s += ic
    return s


#Counter({'orange': 3, 'computer': 3, 'apple': 1, 'abc': 1, 'face': 1})
sumc(a, b, ['abc'], ['face', 'computer'])

```

#### 146 groupby单字段分组

天气记录：

```python
a = [{'date': '2019-12-15', 'weather': 'cloud'},
 {'date': '2019-12-13', 'weather': 'sunny'},
 {'date': '2019-12-14', 'weather': 'cloud'}]
```

按照天气字段`weather`分组汇总：

```python
from itertools import groupby
for k, items in  groupby(a,key=lambda x:x['weather']):
     print(k)
```

输出结果看出，分组失败！原因：分组前必须按照分组字段`排序`，这个很坑~

```python
cloud
sunny
cloud
```

修改代码：

```python
a.sort(key=lambda x: x['weather'])
for k, items in  groupby(a,key=lambda x:x['weather']):
     print(k)
     for i in items:
         print(i)
```

输出结果：

```python
cloud
{'date': '2019-12-15', 'weather': 'cloud'}
{'date': '2019-12-14', 'weather': 'cloud'}
sunny
{'date': '2019-12-13', 'weather': 'sunny'}
```

#### 147 itemgetter和key函数

注意到`sort`和`groupby`所用的`key`函数，除了`lambda`写法外，还有一种简写，就是使用`itemgetter`：

```python
a = [{'date': '2019-12-15', 'weather': 'cloud'},
 {'date': '2019-12-13', 'weather': 'sunny'},
 {'date': '2019-12-14', 'weather': 'cloud'}]
from operator import itemgetter
from itertools import groupby

a.sort(key=itemgetter('weather'))
for k, items in groupby(a, key=itemgetter('weather')):
     print(k)
     for i in items:
         print(i)
```

结果：

```python
cloud
{'date': '2019-12-15', 'weather': 'cloud'}
{'date': '2019-12-14', 'weather': 'cloud'}
sunny
{'date': '2019-12-13', 'weather': 'sunny'}
```

#### 148 groupby多字段分组

`itemgetter`是一个类，`itemgetter('weather')`返回一个可调用的对象，它的参数可有多个：

```python
from operator import itemgetter
from itertools import groupby

a.sort(key=itemgetter('weather', 'date'))
for k, items in groupby(a, key=itemgetter('weather')):
     print(k)
     for i in items:
         print(i)
```

结果如下，使用`weather`和`date`两个字段排序`a`，

```python
cloud
{'date': '2019-12-14', 'weather': 'cloud'}
{'date': '2019-12-15', 'weather': 'cloud'}
sunny
{'date': '2019-12-13', 'weather': 'sunny'}
```

注意这个结果与上面结果有些微妙不同，这个更多是我们想看到和使用更多的。

#### 149 sum函数计算和聚合同时做

Python中的聚合类函数`sum`,`min`,`max`第一个参数是`iterable`类型，一般使用方法如下：

```python
a = [4,2,5,1]
sum([i+1 for i in a]) # 16
```

使用列表生成式`[i+1 for i in a]`创建一个长度与`a`一行的临时列表，这步完成后，再做`sum`聚合。

试想如果你的数组`a`长度十百万级，再创建一个这样的临时列表就很不划算，最好是一边算一边聚合，稍改动为如下：

```python
a = [4,2,5,1]
sum(i+1 for i in a) # 16
```

此时`i+1 for i in a`是`(i+1 for i in a)`的简写，得到一个生成器(`generator`)对象，如下所示：

```python
In [8]:(i+1 for i in a)
OUT [8]:<generator object <genexpr> at 0x000002AC7FFA8CF0>
```

生成器每迭代一步吐出(`yield`)一个元素并计算和聚合后，进入下一次迭代，直到终点。

#### 150 list分组(生成器版)

```python
from math import ceil

def divide_iter(lst, n):
    if n <= 0:
        yield lst
        return
    i, div = 0, ceil(len(lst) / n)
    while i < n:
        yield lst[i * div: (i + 1) * div]
        i += 1

list(divide_iter([1, 2, 3, 4, 5], 0))  # [[1, 2, 3, 4, 5]]
list(divide_iter([1, 2, 3, 4, 5], 2))  # [[1, 2, 3], [4, 5]]
```

#### 151 列表全展开（生成器版）
```python
#多层列表展开成单层列表
a=[1,2,[3,4,[5,6],7],8,["python",6],9]
def function(lst):
    for i in lst:
        if type(i)==list:
            yield from function(i)
        else:
            yield i
print(list(function(a))) # [1, 2, 3, 4, 5, 6, 7, 8, 'python', 6, 9]
```

#### 152 测试函数运行时间的装饰器
```python
#测试函数执行时间的装饰器示例
import time
def timing_func(fn):
    def wrapper():
        start=time.time()
        fn()   #执行传入的fn参数
        stop=time.time()
        return (stop-start)
    return wrapper
@timing_func
def test_list_append():
    lst=[]
    for i in range(0,100000):
        lst.append(i)  
@timing_func
def test_list_compre():
    [i for i in range(0,100000)]  #列表生成式
a=test_list_append()
c=test_list_compre()
print("test list append time:",a)
print("test list comprehension time:",c)
print("append/compre:",round(a/c,3))

test list append time: 0.0219423770904541
test list comprehension time: 0.007980823516845703
append/compre: 2.749
```

#### 153 统计异常出现次数和时间的装饰器


写一个装饰器，统计某个异常重复出现指定次数时，经历的时长。
```python
import time
import math


def excepter(f):
    i = 0
    t1 = time.time()
    def wrapper(): 
        try:
            f()
        except Exception as e:
            nonlocal i
            i += 1
            print(f'{e.args[0]}: {i}')
            t2 = time.time()
            if i == n:
                print(f'spending time:{round(t2-t1,2)}')
    return wrapper

```

关键词`nonlocal`常用于函数嵌套中，声明变量i为非局部变量；

如果不声明，`i+=1`表明`i`为函数`wrapper`内的局部变量，因为在`i+=1`引用(reference)时,`i`未被声明，所以会报`unreferenced variable`的错误。

使用创建的装饰函数`excepter`, `n`是异常出现的次数。

共测试了两类常见的异常：`被零除`和`数组越界`。

```python
n = 10 # except count

@excepter
def divide_zero_except():
    time.sleep(0.1)
    j = 1/(40-20*2)

# test zero divived except
for _ in range(n):
    divide_zero_except()


@excepter
def outof_range_except():
    a = [1,3,5]
    time.sleep(0.1)
    print(a[3])
# test out of range except
for _ in range(n):
    outof_range_except()

```

打印出来的结果如下：
```python
division by zero: 1
division by zero: 2
division by zero: 3
division by zero: 4
division by zero: 5
division by zero: 6
division by zero: 7
division by zero: 8
division by zero: 9
division by zero: 10
spending time:1.01
list index out of range: 1
list index out of range: 2
list index out of range: 3
list index out of range: 4
list index out of range: 5
list index out of range: 6
list index out of range: 7
list index out of range: 8
list index out of range: 9
list index out of range: 10
spending time:1.01
```


#### 154 测试运行时长的装饰器


```python
#测试函数执行时间的装饰器示例
import time
def timing(fn):
    def wrapper():
        start=time.time()
        fn()   #执行传入的fn参数
        stop=time.time()
        return (stop-start)
    return wrapper

@timing
def test_list_append():
    lst=[]
    for i in range(0,100000):
        lst.append(i)  

@timing
def test_list_compre():
    [i for i in range(0,100000)]  #列表生成式

a=test_list_append()
c=test_list_compre()
print("test list append time:",a)
print("test list comprehension time:",c)
print("append/compre:",round(a/c,3))

# test list append time: 0.0219
# test list comprehension time: 0.00798
# append/compre: 2.749
```

#### 155 装饰器通俗理解

再看一个装饰器：

```python
def call_print(f):
  def g():
    print('you\'re calling %s function'%(f.__name__,))
  return g
```

使用`call_print`装饰器：

```python
@call_print
def myfun():
  pass
 
@call_print
def myfun2():
  pass
```

myfun()后返回：

```python
In [27]: myfun()
you're calling myfun function

In [28]: myfun2()
you're calling myfun2 function
```

**使用call_print**

你看，`@call_print`放置在任何一个新定义的函数上面，都会默认输出一行，你正在调用这个函数的名。

这是为什么呢？注意观察新定义的`call_print`函数(加上@后便是装饰器):

```python
def call_print(f):
  def g():
    print('you\'re calling %s function'%(f.__name__,))
  return g
```

它必须接受一个函数`f`，然后返回另外一个函数`g`.

**装饰器本质**

本质上，它与下面的调用方式效果是等效的：

```
def myfun():
  pass

def myfun2():
  pass
  
def call_print(f):
  def g():
    print('you\'re calling %s function'%(f.__name__,))
  return g
```

下面是最重要的代码：

```
myfun = call_print(myfun)
myfun2 = call_print(myfun2)
```

大家看明白吗？也就是call_print(myfun)后不是返回一个函数吗，然后再赋值给myfun.

再次调用myfun, myfun2时，效果是这样的：

```python
In [32]: myfun()
you're calling myfun function

In [33]: myfun2()
you're calling myfun2 function
```

你看，这与装饰器的实现效果是一模一样的。装饰器的写法可能更加直观些，所以不用显示的这样赋值：`myfun = call_print(myfun)`，`myfun2 = call_print(myfun2)`，但是装饰器的这种封装，猛一看，有些不好理解。

#### 156 定制递减迭代器

```python
#编写一个迭代器，通过循环语句，实现对某个正整数的依次递减1，直到0.
class Descend(Iterator):
    def __init__(self,N):
        self.N=N
        self.a=0
    def __iter__(self):
        return self 
    def __next__(self):
        while self.a<self.N:
            self.N-=1
            return self.N
        raise StopIteration
    
descend_iter=Descend(10)
print(list(descend_iter))
[9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```

核心要点：

1 `__nex__ `名字不能变，实现定制的迭代逻辑

2 `raise StopIteration`：通过 raise 中断程序，必须这样写