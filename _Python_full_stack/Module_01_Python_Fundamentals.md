# 1. Computer Fundamentals and Environments
## 1.1 Computer Operation System
- windows
  - xp
  - win7
  - win10
- linux
  - centos
  - ubuntu
  - redhat
- max
## 1.2 Programming Languages
- Interpreter: Python, PHP, JavaScript, Ruby
- Compiler: C, C++, Go, Java
## 1.3 Python Introduction
Python interpreter includes:
- CPython (most popular), developed by C
- Jython, developed by Java
- IronPython
- RubyPython
- PyPy
## 1.4 CPython Interpreter Version
- 2.x, latest version is Python2.7.18 (not supported after 2020)
- 3.x
## 1.5 Coding(code book)
There are many code books in programing, such as utf-8 coding and gbk coding. When we use different code books, the 0/1 binaries saved in hard drive will be different.
>[!NOTE]
>**Users must use the same code book as the files are saved to open them. If user save a file with UTF-8 and open it with GBK, then it will become garbled characters.**
# 2. Programming with Python
Python interpreter will open python file with UTF-8 by default. If user want to change Python interpreter coding, user can add following at the file beginning
```python3
# -*- coding:gbk -*-
```
## 2.1 Output
- print output will add 、\n at the end by default.
- If user do not want to add \n, try following
  ```python3
  print("test", end="")
  ```
## 2.2 Data type
- int
- str
- bool
## 2.3 Variable
- only include character, number or _
- can not start with number
- avoid Python keyword
## 2.4 Comments
- comment single line
```
# comment signel line
```
- comment multiple lines
```
"""
line a
line b
"""
```
## 2.5 Input
```
name = input("Please input user name")
```
## 2.6 If statement
- if else
- if elif else
# 3. loop statement
- while loop
- for loop
- break
> [!IMPORTANT]
> break only break current loop statement
- continue. Exit current interation and start next interation
- while else
# 4. string formatting
## 4.1 %
```
test="I'm %s, and I'm %d years old" % (name, age)
test="I'm %(name)s, and I'm %(age)d years old" % ("name":"Joe", "age":"24")
```
## 4.2 format
```
text="I'm {}, {} years old, real name is {}".format("Joe", 24, "Joe")
text="I'm {0}, {1} years old, real name is {0}".format("Joe", 24)
text="I'm {a1}, {a2} years old, real name is {a1}".format(a1="Joe", a2=24)
```
## 4.3 f
This is supported after Python3.6
```
text=f"I'm {'Joe'}, I like reading"

name='Joe'
text=f"I'm {name}, I like reading"
```
# 5. operator
- arithmetic operators
- comparision operators
- assignment operators
- membership operators
- logical operators
# 6. System and coding
- Binary system
- Octal system
- Decimal system (int type, others are string type)
- Hexadecimal system
## 6.1 units in computer
- b (bit)
- B (byte), 1 B=8 bit
- KB (kilobyte)
- M
- G
- T
## 6.2 coding
### 6.2.1 ascii 
ascii requires to use 1 byte to present the relationship between binary system and character
### 6.2.2 gb-2312 
### 6.2.3 unicode
ucs2 requires 2 byte to present a character, uc4 requires 4 byte to present a character.
### 6.2.4 utf-8 
This is the most popular method. It can represent all characters and will not waste spaces.
## 6.3 coding in Python
```
test = "This is a test"
data = test.encode("utf-8")
file_object=open("log.txt", mode="wb")
file_object.write(data)
file_object.close()
```
# 7. Data type
| Data Type | Definition | Distinct Function | Common Function | Type Convert | Others |
| --- | --- | --- | --- | --- | --- |
| int | age=24 | NA | add, minus, multiple, divide | n1=int(True), v1=int("186", base=10) | Python3: int(unlimited), v1=9/2=4.5, Python2: int, long int, v1=9/2=4 |
| bool | data=False | NA | v1=True+True=2 | **Conver 0/""/[]/()/{}/None to bool will get False.** Convert others will get True | automaticly convert to bool at if or while statement |
| str | v1="test" | v1.startswith("a"), v1.endswith("a"), v1.isdecimal(), v1.strip(), v1.lstrip(), v1.rstrip(), v1.upper(), v1.lower(), v1.replace(), v1.split('-'), v1.rsplit(), v1.join(), v1.format(), v1.encode(), v1.decode(), v1.center(), v1.rjust(), v1.zfill() | v1="a"+"b", v1="a"*3, len(v1), v1[0:2] | Only int can convert to string | String can not be edited |
| list | a=[1,5,9,17] | str can't be edited directly: a='Joe', b=a.upper() list can be edited directly: a=[3,9], a.append(17), a.insert(), a.remove(), a.pop(), a.clear(), a.sort(), a.index(), a.reverse() | ['a','b']+['c','d'], ['a','b']*2, len(a), a[0:2] | int and bool can not be convert to list. name="Joe", data=list(name), data is ["J","o","e"]; v1=(1,2,3), a1=list(v1); v2={"Joe","Lee"}, b2=list(v2) | list inside another list. **order, editable,** store different types.
| tuple | v1=(1,2,3) | NA | a=("A","B"), b=("C","D"), c=a+b, d=a*2, len(a), a[0:1] | only string and list can be convert to tuple | **order, uneditable,** store different types. |
| set | v1={11,33,"Joe"} | v1.add(), v1.discard(), v1.intersection(), v1.union(), v1.difference() | v2=v1-v2, v3=v1&v2, v3=v1/v2, len(v1) | int, list, tuple, dict can be converted to set | **Orderless, editable, not repeat, elements must be hashable but set is not hashable, elements must be hashable int\bool\str\tuple, list\set\dict are not hashable, very high efficiency in finding elements** |
| dict | test={"a":1,"b":2}, keys must hashable **(int/bool/str/tuple hashable, list/set/dict not hashable)** | test.get("a"), test.keys(), test.values(), test.items(), test.setdefault(), test.update(), test.pop(), test.popitem() | t3=t1\t2, len(test), test["a"]=9 | v=dict(("t1","t2"),["a1","a2"]) | high efficiency in finding elements |
# 8. Others
- Code specifications [PEP 8](https://peps.python.org/pep-0008/)
- Difference between is and ==? == compares two values, is compares two address.
![python_fundamentals_roadmap](/_Python_full_stack/imgs/Fundamentals.jpg)








































