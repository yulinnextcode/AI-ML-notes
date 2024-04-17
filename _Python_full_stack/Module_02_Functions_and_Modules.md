# 1. Function and Module
- Function, a code block specifically designed to implement a certain functionality, and it's resuable.
  - Build-in function: len, bin
  - Custom function
- Module, a collection of functions.
  - Build-in module: import random
  - Custom module
  - Third-party module, download online completed by others

![Python_File_Operation](/_Python_full_stack/imgs/File_Operation.jpg)

# 2. File Operation
String can be categorized as follows
- Textual string format (str), it's used to represent texrual information. They are essentially biary in Unicode encoding
```
text_str="Juventus is a great team."
```
- Byte string format (byte), it used to store binaries.
  - Can represent text information. Essentially are binary in utf-8 or gbk encoding.
  - Can represent original binary， like pictures or files.
```
byte_str=b"I love Juventus"
```
> [!NOTE]
> If user use the b"XXX" method to create a byte string, the content within the string must be characters found in the ASCII table. Otherwise it will return an error "SyntaxError: bytes can only contain ASCII literal characters."

## 2.1 Read file
Open file
  - Directories:
      - Relative path: 'info.txt'
      - Absolute path (The only thing to be careful with on raw strings is that they can't end with \):
          - '/Users/test/info.txt' (Linux)
          - 'C:\\mydir' (Windows)
          - r'C:\mydir' (Windows)
          - os.path.join(mydir, myfile) (Windows)

'''python
import os
file_path="/Users/test/info.txt"
exists = os.path.exists(file_path)
if exists:
  file_object=open('intest.txt', mode='rt', encoding='utf-8')
  data=file_object.read()
  file_object.close()
  print(data)
else:
  print("文件不存在")
···

## 2.2 Write file
- write text file
```python
file_object1=("test.txt", mode='wb')   # wb mode
file_object1.write('Testing'.encode("utf-8"))
file_object1.close()

file_object2=("test.txt", mode='wb')   # wb mode
file_object2.write('Testing')
file_object2.close()
```
- write binary file
```python
f1 = open("a1.png", mode='rb')
content = f1.read()
f1.close()

f2 = open("a1.png", mode='wb')
f2.write(content)
f2.close()
```

## 2.3 File operation mode
- Mode:
  - "r" Read, default value. Opens a file for reading, error if the file does not exist
  - "a" Append, opens a file for appending, create the file then append if it does not exist
  - "w" Write, opens a file for writing, create the file if it does not exist
> [!NOTE]
> If file not exists, w mode will create a new one. If file exists, w model will empty file and then write.
  - "x" Create, creates the specified file, returns an error if the file exist
  - "+" open a disk file for updating (reading and writing)
  - "r+", "rt+", "rb+" cursor at the beginning
  - "w+", "wt+", "wb+" cursor at the beginning
  - "x+", "xt+", "xb+" cursor at the beginning
  - "a+", "at+", "ab+" cursor at the ending
- In addition you can specify the file should be handled as binary or text mode
  - "t" Text, default value. Text mode
  - "b" Binary, binary mode (e.g. images)

## 2.4 Common functions
- read, read all or read speficif characters
- readline, read one line
- readlines, read all lines
- read large file
```python
f = open("info.txt", mode="r", encoding="utf-8")
for line in f:
  print(line.strip())
f.close()
```
- write
```python
f = open("info.txt", mode="a", encoding="utf-8")
f.write("test")
f.close()
```
- flush
```
f = open("info.txt", mode="a", encoding="utf-8")
while True:
  f.write("test")
  f.flush()
f.close()
```
- move cursor
```
f = open("info.txt", mode="r+", encoding="utf-8")
f.seek(3)
f.write("test")
f.close()
```
- get cursor position
```
f = open("info.txt", mode="r", encoding="utf-8")
p1=f.tell()
print(p1)   # 0
f.read(3)
p2=f.tell()
print(p2)   # 9
f.close()
```
- with open
```python
with open("xxx.txt", mode='rb') as file_object:
  data = file_object.read()
  print(data)
```

## 2.5 csv format file
Comma-Separated Values, CSV. Store numbers and text.
```python
import os
import requests
with open("files/mv.csv", mode="r", encoding="utf-8") as file_object:
  file_object.readline()
  for line in file_object:
    id, username=line.strip().split(',')
```
## 2.6 ini format file
```python
import configparser
config = configparser.ConfigParser()
config.read('files/test.ini', encoding='utf-8')
ret = config.sections()
itel_list = config.items("test")
```
## 2.7 XML format file
```python
from xml.etree import ElementTree as ET
tree = ET.XML(content)
print(tree)
```
## 2.8 Excel format file
- read excel sheet
```python
from openpyxl import load_workbook
wb = load_workbook("files/p1.xlsx")
print(wb.sheetnames)
```
- write excel sheet
```python
from openpyxl import load_workbook
wb = load_workbook('files/test.xlsx')
sheet = wb.worksheets[0]
```
## 2.9 Archieve file
```python
import shutil
shutil.make_archive(base_name=r"test", format="zip", root_dir=r"files")
shutil.unpack_archive(filename=r'test.zip', extract_dir=r'files/test', format='zip')
```
## 2.10 Directories
windows use \, linux use /. Sepcifically, if you use D:\nxxx\txxx\x1, then it will return an error because \n and \t are special character for Python. Therefore, programming in windows we use following two methods:
- "D:\\nxxx\\txxx\\test1"
- r"D:\nxxx\txxx\test2"
```python
import os
abs = os.path.abspath(__file__) # get current py file's location /Users/test/info.py
path = os.path.dirname(abs) # get current py file's directory    /Users/test
```
```python
import os
base_dir=os.path.dirname(os.path.abspath(__file__))
file_path=os.path.join(base_dir, "files", "info.txt")
print(file_path)
if os.path.exists(file_path):
  file_object=open(file_path, mode="r", encoding="utf-8")
  data=file_object.read()
  file_object.close()
  print(data)
else:
  print("File directory not exists")
```
```python
import shutil
import os

# Get current directory  
abs_path=os.path.abspath(__file__)

# Get current file's upper level directory
base_path=os.path.dirname(os.path.abspath(__file__))

# Join directory
test1 = os.path.join(base_path, "xx")

# Check if directory exists
exists = os.path.exists(test1)

# Create directory
os.makedirs(test1)

# Check if is it a folder
is_dir = os.path.isdir(file_path)

# Delete file or folder
os.remove(path)

# Copy file
shutil.copy("/Users/folder1/test.png","/Users/folder2/test.png")

# Rename file or folder name
shutil.move("/Users/folder/test1.png","/Users/folder/test2.png")
```
> [!IMPORTANT]
> File relative directory.
> File absolute directory (recommend). Do not write fixed directory, use os module to get absolute directory to avoid error when transferring codes to other computers.
> "D:\\nxxx\\txxx\\test1" or r"D:\nxxx\txxx\test2" or os.path.join

# 3. Custom functions

![function_introduction](/_Python_full_stack/imgs/Functions.PNG)

## 3.1 Parameters
- formal parameters
···python
def func(a1, a2, a3)
···
- actual parameters
'''python
def func(11,22,33)
'''
- parameters by location
```python
def add(n1,n2):
  print n1+n2

add(23,67)
```
- parameters by keyword
```python
def add(n1,n2):
  print n1+n2

add(n1=23,n2=67)
```
# 3.2 Dynamic parameters
- *, pass parameters by location
```python
def func(*args):
  print(args)  # tuple, (22,56,89)

func(23,56,89) 
```
- **, pass parameters by keyword
```python
def func(**kwargs):
  print(kwargs)  # dictionary, {"n1":"Jason", "age":"23", "salary":"8700"}

func(n1="Jason", age=23, salary=8700)
```
- *,** ()
```python
def func(*args,**kwargs):
  print(args,kwargs)  # dictionary, (23,89) {"n1":"Jason", "age":"23", "salary":"8700"}

func(23,89,n1="Jason", age=23, salary=8700)
```
> [!IMPORTANT]
> ** must be placed after *
```python
def func(t1,t2,t3, t4=23, *args, t5=89, **kwargs):
  print(t1,t2,t3,t4,t5,args,kwargs)

func(11,22,33,44,55,66,77,t5=66,t9=77)
```
# 3.3 Function return value
- Function return value can be any type. If return value is not specified, the it will return None.
- If return values are seperated by "," that will be tuple
```python
def func():
  return 7,8,9

value=func()
print(value)  #(7,8,9)
```
- Functions will be terminated as once when return is executed.

# 4. Function next step
![Python_Function_Advanced](/_Python_full_stack/imgs/Function_next_step.PNG)
When functions transfer parameters, they are transferring memoray address. The return values are also returning the meory address.
function names are variables. Function name can be assigned to other variables.

# 5. Advanced functions
![Python_Function_Advanced](/_Python_full_stack/imgs/Function_advanced.PNG)
## 5.1 Nested functions
First find inside its own domain, then find upper domain.
## 5.2 Function closure
Python closure is a nested function that allows us to access variables of the outer function even after the outer function is closed.
```python
#1st example of function closure
def task(arg):
  def inner():
    print(arg)
  return inner

v1=task(11)
v2=task(22)
v3=task(33)
```
```python
#2nd example of function closure

from concurrent.futures.thread import ThreadPoolExecutor

def task():
  pass

def done(arg):
  content=arg.result()

POOL = ThreadPoolExecutor(10)

video_list=[("a", "http1"),("b", "http2")]

for item in video_list:
  print(item):
  future = POOL.submit(task, url=item[1])
  fugure.add_down_callback(done)

for item in video_list:
  res
```

## 5.3 Decorator
https://python-3-patterns-idioms-test.readthedocs.io/en/latest/PythonDecorators.html
- Implementation principle: Based on the syntax "@" and function closures, encapsulate the original function within a closure, then assign the function to a new function (inner function), and exucute the function within the inner function to perform the operation contained within the closure.
- Implementation effect: Without altering the internal code of the original function or its invocation method, the implementation enables additional functionalities to be executed before or after the function's execution.
- Applicable scenario: When multiple functions within a system need to uniformly incorporate custom functionalities before and after execution.
- Code example
```python
import functools

def decorator(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        # Before function
        # Do something
        value = func(*args, **kwargs)
        # After function
        # Do something
        return value
    return wrapper

@decorator
def some_func():
    pass

some_func()
```
> [!IMPORTANT]
> @decorator atcually equals to some_func = decorator(some_func)

Here is another example
```python
def outer(func):
    def inner():
        print('Before func()..')
        func()
        print('After func()..')
    return inner

@outer
def hi():
    print('Hi World')

hi()

# output:
# Before func()..
# Hi World
# After func()..
```
> [!IMPORTANT]
> @outer atcually equals to hi = outer(hi)
One more example
```python
import functools

def outer(origin):
  @functools.wraps(origin)   # inner.__name__=origin.__name__, inner.__doc__=origin.__doc__
  def inner(*args, **kwargs):
    print('Before func()..') # before function
    res = origin(*args, **kwargs)
    print('After func()..') # after function
    return res
  return inner

@outer
def func():
  pass

func()
```
> [!IMPORTANT]
> @outer atcually equals to func = outer(func)
> Based on my understanding:  func, outer anbd inner the parameters should be consistent
> If a decorator comes with a parameter like @timeit(10), that means double = timeit(10)(double)


# 6. Build-in functions and comprehensions
![Python_Function_Advanced](/_Python_full_stack/imgs/Build-in_function_and_comprehensions.jpg)
## 6.1 Anonymous Functions (lambda expression)
Anonymous function is based on lambda expression. It can define a function without name.
```python
data_list = [lambda x:x+100, lambda x:x+200, lambda x:x+900]
print(data_list[0])
```
```python
f1 = lambda x:x+250

res=f1(50)
print(res)
```
The format of Lambda function is: lambda parameters:function_body
- parameters, can be any data type
```python
lambda x1,x2: function_body
lambda *args, **kwargs: function_body
```
- function_body, can only support single line code
- return value, return the single line code's result by default.
- Anonymous function is suitable to process simple functions.
## 6.2 Generator
Generator is composed by **function + yield kewword**, this can help save memory under some conditions.
```python
def func():
  print(111)
  yield 1
```
Assuem you are assigned to create 300w random 4-digit numbers and print out. There are two methods.
- Create 300w numbers one time in memory
```python
import random

data_list=[]
for i in range(300000000):
  val=random.randint(1000,9999)
  data_list.append(val)    #get from data_list when use it
```
- Create dyanmically, one by one
```python
import random

def gen_random_num(max_count):
  count=0
  while conter<max_count:
    yield randome.randint(1000,9999)
    counter+=1

data_list=gen_randome_num(300000000)  #get from data_list when use it
```
Therefore when we need to create a lot of data in memory, we can use generator to create one by one to save memory.
## 6.3 Build-in functions
- abs, pow, sum, divmod, round
- min, max, all, any
- bin, oct, hex
- ord, chr
- int, float, str (unicode), bytes (utf-8, gbk), bool, list, dict, tuple, set
```python
v1 = "Jason" # text string
v2 = v1.encode('utf-8') # byte string
v3 = bytes(v1.encoding='utf-8') # byte string
```
- len, print, input, open, type, range, enumrate, id, hash, help, zip, callable, sorted
## 6.4 Comprehensions
Comprehensions are a very conveient method, which use one line code to create list\dict\tuple\set and initialize values. Assume we need to create a list and initialize from 0 to 299.
```python
data = []
for i in range(300):
  data.append(i)

num_list=[i for i in range(10)] #Comprehensions in list
num_set={i for i in range(10)} #Comprehensions in set
num_dict={i:i for i in range(10)} #Comprehensions in dict
data=(i for i in range(10)) #Comprehensions in tuple
```
# 7 Modules
![Modules](/_Python_full_stack/imgs/Modules.PNG)
## 7.1 Custom modules
We can call a single py file a **module**, we can call a folder containing many py files a **package**. Follow commons is a package, and run.py is a module.
```
|------commons (package)
|         |------convert.py  
|         |------page.py  
|         |------utils.py  
|------run.py (module)
```
> [!NOTE]
> Under package folder, there should be a __init__.py file by default. This file is to describe current package's information. When modules are imported under this package, this will also be loaded automatically.
## 7.2 Import
After you create a module or package, and you want to use some of its functions, you need to import first and then use. Import is actually load module or package into memory, then grab from the memory when use it.
If you want to import any module or package, you have to add the path then this can be founded. You can also add to specified path with sys.path
```python
import sys
sys.path.append("path A")

import XXX # import xxx.py under path A
```
- Do not use build-in modules' names
- user can add current project path to sys.path manually
```python
import os
import sys

sys.path.append(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))
```
- pycharm can add project directory to sys.path by default
Import is actually load modules or packages to memory, then grab from memory when use it. There are two methods to import:
- import module or import package
- from xxx import xxx
- from xxx.xx import xxxx as xo
> [!NOTE]
> Under package folder, there should be a __init__.py file by default. This file is to describe current package's information. When modules are imported under this package, this will also be loaded automatically.
> [!IMPORTANT]
> Please note when executing a py file and importing a py file is different
- execute a py file
```python
__name__="__main__"
```
- import a py file
```python
__name__="module name"
```
For example, following is a file structure
```python
|------commons (package)
|         |------__init__.py 
|         |------convert.py  
|         |------page.py  
|         |------utils.py
|         |------tencent
|         |         |------__init__.py
|         |         |------sms.py
|------run.py (module)
|------many.py

import many
from commons import page
from commons import utils

def start():
  v1=many.show()
  v2=utils.test()

if__name__=='__main__':
  start()
```
Run is the startpoint file. When we execute run.py, other py files are funcitonal codes. When executing run.py directly, __name__ becomes __main__. Therefore, start function can only be run when we execute run.py directly. When we import these files, start function can not be run.

## 7.3 Third-party modules
- Using pip install third-party modules.
- If you can not find installation in pypi.org or can not pip install. You can download source codes and install
  - Download source codes, unzip
  - Enter directory
  - run installation
- wheel. This is a file format for Python's third-party modules.

# 8 Build-in modules
## 8.1 os
```python
import os

abs_path=os.path.abspath(__file__)                    # 1. get current script's absolute path
base_path=os.path.dirname(os.path.dirname(path))      # 2. get current folder's upper directory
p1=os.path.join(base_path,'xx','oo','a1.png')         # 3. contact path
exists=os.path.exists(p1)                             # 4. check exist or not  
os.makedirs(path)                                     # 5. create directory
is_dir=os.path.isdir(file_path)                       # 6. whether is folder
os.remove(path)                                       # 7. delete file or folder
data=os.listdir("/User/commons")                      # 8. get folder's all files
data=os.walk("/User/commons/mp4")                     # 9. Current folder's all mp4
```
## 8.2 shutil
```python
import shutil

path=os.path.join(base_path, 'xx')    # 1. delete folder
shutil.rmtree(path)

shutil.copytree('/Users/files')       # 2. copy folder

shutil.copy('/Users/files1/1.png','/Users/files2/1.png')    # 3. copy file

shutil.move('/Users/files/1.png','/Users/files/2.png')      # rename folder or file

shutil.make_archive

shutil.unpack_archive
```
## 8.3 sys
```python
import sys

print(sys.version)
print(sys.version_info)
print(sys.path)
```
## 8.4 random
```python
import random

v=random.randint(10,20)
v=random.uniform(1,10)
v=random.choice([1,2,3])
v=random.sample([1,2,3,4,5])
random.shuffle([1,2,3,4,5])
```
## 8.5 hashlib
## 8.6 configparser
## 8.7 xml
## 8.8 json
josn is a python build-in module. Can convert python data format to json, or json data format to python data format.
| Python  | JSON |
| ------------- | ------------- |
| dict  | object  |
| list, tuple  | array  |
| str  | string  |
| int,flost  | number  |
| True  | true  |
| False  | false  |
| None  | null  |
```python
json.dumps
json.loads
json.dump
json.load
```
- Python data type -> json format (serizlization)
```python
import json

data = [
  {"id":1, "name": "Alex", "age":23},
  {"id":2, "name": "John", "age":18},
]

res = json.dumps(data)
print(res)

res = json.dumps(data, ensure_ascii=False)
print(res)
```
- json format -> Python data type (anti-serialization)
```python
import json

data_string = '[{"id": 1, "name": "Alex", "age": 18}, {"id": 2, "name": "John", "age": 27}]'
data_list = json.loads(data_string)
print(data_list)
```
## 8.9 time/datetime
```python
import time
v1=time.time()

from datetime import datetime, timezone, timedelta
v1=datetime.now()

v2=time.timezone

time.sleep(5)
```
There are three data formats for time as follows:
- datetime
```python
from datetime import datetime, timezone, timedelta

v1=datetime.now()
v2=datetime.utcnow()
```
- string
```python
text = "2020-11-11"
v1=datetime.strptime(text, '%Y-%m-%d')
```
- timestamp
```python
ctime=time.time()
v1=datetime.formtimestamp(ctime)
```
## 8.10 About regular expression
### 8.10.1 regular expression
- 1. process characters
```python
[abc]
[^abc]
[a-z]
.
\w
\d
\s
```
- 2. process numbers
```python
*
+
?
{n}
{n,}
{n,m}
```
- 3. group
- 4. starts (^) and ends ($)
```python
import re

test="34235234@gmail.com"
email_list=re.findall("^\w+@\w+.\w+$", text, re.ASCII)
```
- 5. special character
Pay attentions to special characters in regular expression * . \ { } ( )
```python
import re

text="This is {9} times"
data=re.findall("\{9\}", "exp1")
```
### 8.10.2 re module
- findall
```python
import re

data_list=re.findall("", text)
```
- match
```python
import re

data=re.match("", text)
```
- search
```python
import re

data=re.search("", text)
```
- sub
```python
import re

data=re.sub("", "", text)
```
- split
```python
import re

data=re.split("", text)
```python
- finditer
```python
import re

data=re.finditer("", text)
```

# 9 Project development specification
## 9.1 Single file development
```python
"""
File description
"""

import re
import random
import requests
from openpyxl import load_workbook

def do_something():
  """ Function description """
  # TODO, will be implemented in next step
  for i in range(10):
    pass

def run():
  """ Function description """
  # Function code explaintaion
  text=input(">>>")
  print(text)

if __name__ == '__main__':
  run()
```
![Python_development_specification](/_Python_full_stack/imgs/Python_development_specification.png)

## 9.2 Single executable file
Create a new project called crm. The file structure is showns as follows:
```python
crm
 |---app.py     file, mail file of the program (as concise as possible)
 |---config.py  file, configuration file (store configuration related information, code will read configuration form here)
 |---db         folder, store data
 |---files      folder, store files
 |---src        package, process functions
 |---utils      package, common functions
```
![Single_python_executable_specification](/_Python_full_stack/imgs/single_executable.png)
## 9.3 Multiple executable files
Create a new project killer, the file structure and folders can be shonw here
```python
killer
  |---bin                   folder, can store multiple files(executable)
  |    |---app1.py          
  |    |---app2.py
  |---config                package, configuration files
  |---db                    folder, store data
  |---files                 folder, store files
  |---src                   package, functional codes  
  |    |---__init__.py
  |---utils                 package, common functions
       |---__init__.py    
```
![Multiple_python_executable_specification](/_Python_full_stack/imgs/multiple_executable.png)










# 9 Summary
![Python_File_Operation](/_Python_full_stack/imgs/Module_2_summary.PNG)










