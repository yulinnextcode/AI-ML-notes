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
def outer(origin):
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


# 6. Build-in functions and comprehensions
![Python_Function_Advanced](/_Python_full_stack/imgs/Build-in_function_and_comprehensions.PNG)




