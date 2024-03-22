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
