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



















