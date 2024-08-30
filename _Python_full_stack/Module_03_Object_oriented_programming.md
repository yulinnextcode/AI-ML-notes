

<details>
<summary><h1>0. Object_Oriented&Network&Cocurrent Programming</h1></summary>

## 0.1 Module 3 introduction

This module will include following three parts:

![Python_File_Operation](/_Python_full_stack/imgs/Module_3.jpg)

- Object-oriented: Python support two programming method - Functional programming and Object oriented programming
  - Functional programming
    ```python
    # define function
    def func():
      print("This is a function")

    # execute function
    func()
    ```
  - Object oriented programming
    ```python
    # define class
    class Foo(object):
      # define a method in class
      def func(self):
        print("This is a method")

    obj = Foo() # instanclize class
    obj.func()  # execute class's method
    ```
- Network programming
- Concurrent programming

## 0.2 Introduction
![Python_File_Operation](/_Python_full_stack/imgs/Module_3_1_1.png)
- Introduction to Object-Oriented Programming
- Three Main Features of Object-Oriented Programming
  - Encapsulation
  - Inheritance
  - Ploymorphism
- Revisiting Data Types

</details>

<details>
<summary><h1>1. Object Oriented</h1></summary>

To implement a specific function or functions using object-oriented programming, you need to follow two steps:
- Define a class: Within the class, define methods to implement the specific functionality.
- Instantiate the class: Create an object from the class, and use the object to call and execute the methods.

```python
class Message:

    def send_email(self, email, content):
        data = "给{}发邮件，内容是：{}".format(email,content)
        print(data)


msg_object = Message() # 实例化一个对象 msg_object，创建了一个一块区域。
msg_object.send_email("wupeiqi@live.com","注册成功")
```
> [!NOTE]
> Class names should start with an uppercase letter and use camel case naming.
> In Python 3 and later, all classes inherit from object by default.
> Functions written within a class are called methods.
> The first parameter of every method is self.

## 1.1 Object and self
In each class, you can define a special method called __init__ (initialization method). This method is automatically executed when an object is created from the class, i.e., object = Class().

> [!TIP]
> The function of __init__.py file?
> In Python, the __init__.py file is used to mark a directory as a Python package. It is used to initialize the package when it is imported. The __init__.py file can contain code that will be executed when the package is imported, as well as function definitions and variable assignments.

···python
class Message:

    def __init__(self, content):
        self.data = content   # self means the obejct which are using this method

    def send_email(self, email):
        data = "给{}发邮件，内容是：{}".format(email, self.data)
        print(data)

    def send_wechat(self, vid):
        data = "给{}发微信，内容是：{}".format(vid, self.data)
        print(data)
        
msg_object = Message("Register Success")   # When this is run, it will automatically run __init__ method, it can be considered running following two steps
  1. create a object based on class, one space in memory
  2. When the __init__ method is executed, the module passes the memory address of the created area as the self parameter.
···

> [!IMPORTANT]
> Key information users need to know to achieve their goal.
> self is essentially a parameter. This parameter is provided internally by Python and essentially refers to the object that is calling the current method.
> An object is a “block of memory” instantiated from a class, which initially contains no data. Through the class’s __init__ method, some data can be initialized in this memory.

## 1.2 Common members

When writing object-oriented code, the most common members are:
- Instance Variables: Belong to the object and can only be accessed through the object.
- Bound Methods: Belong to the class and can be called through the object or the class.

```python
class Person:

    def __init__(self, n1, n2):
        # Instance Variables
        self.name = n1
        self.age = n2
	
    # Bound Methods
    def show(self):
        msg = "我叫{}，今年{}岁。".format(self.name, self.age)
        print(msg)

    def all_message(self):
        msg = "我是{}人，我叫{}，今年{}岁。".format(Person.country, self.name, self.age)
        print(msg)

    def total_message(self):
        msg = "我是{}人，我叫{}，今年{}岁。".format(self.country, self.name, self.age)
        print(msg)
```
How to run execute boundary methods?
p1 = Person("武沛齐",20)
p1.show()
或
p1 = Person("武沛齐",20)
Person.show(p1)



## 1.3 Application example

- 1.Encapsulating data into an object makes it easier to use later.
```python
   class UserInfo:
       def __init__(self, name, pwd,age):
           self.name = name
           self.password = pwd
           self.age = age
   
   
   def run():
       user_object_list = []
       # 用户注册
       while True:
           user = input("用户名：")
           if user.upper() == "Q":
               break
           pwd = input("密码")
           
           # user_object对象中有：name/password
           user_object = UserInfo(user, pwd,19)
           # user_dict = {"name":user,"password":pwd}
           
           user_object_list.append(user_object)
        # user_object_list.append(user_dict)
   
       # 展示用户信息
       for obj in user_object_list:
           print(obj.name, obj.password)

   ```
> [!IMPORTANT]
> Encapsulate data into an object to retrieve it later.
> Standardize data (constraints)

- 2.Encapsulate data into an object and process the raw data within methods.
```python
   user_list = ["用户-{}".format(i) for i in range(1,3000)]
   
   # 分页显示，每页显示10条
   while True:
       page = int(input("请输入页码："))
   
       start_index = (page - 1) * 10
       end_index = page * 10
   
       page_data_list = user_list[start_index:end_index]
       for item in page_data_list:
           print(item)
# Above method is not good.
```

```python
   class Pagination:
       def __init__(self, current_page, per_page_num=10):
           self.per_page_num = per_page_num
           
           if not current_page.isdecimal():
               self.current_page = 1
               return
           current_page = int(current_page)
           if current_page < 1:
               self.current_page = 1
               return
           self.current_page = current_page
   
       def start(self):
           return (self.current_page - 1) * self.per_page_num
   
       def end(self):
           return self.current_page * self.per_page_num
   
   
   user_list = ["用户-{}".format(i) for i in range(1, 3000)]
   
   # 分页显示，每页显示10条
   while True:
       page = input("请输入页码：")
   	
       # page，当前访问的页码
       # 10，每页显示10条数据
   	   # 内部执行Pagination类的init方法。
       pg_object = Pagination(page, 20)
       page_data_list = user_list[ pg_object.start() : pg_object.end() ]
       for item in page_data_list:
           print(item)
```
> [!IMPORTANT]
> Encapsulate data into an object and then retrieve the data from the object.

- 3.Create multiple objects from a class and modify the data within the objects using methods.

   ```python
   class Police:
       """警察"""
   
       def __init__(self, name, role):
           self.name = name
           self.role = role
           if role == "队员":
               self.hit_points = 200
           else:
               self.hit_points = 500
   
       def show_status(self):
           """ 查看警察状态 """
           message = "警察{}的生命值为:{}".format(self.name, self.hit_points)
           print(message)
   
       def bomb(self, terrorist_list):
           """ 投炸弹，炸掉恐怖分子 """
           for terrorist in terrorist_list:
               terrorist.blood -= 200
               terrorist.show_status()
   
   """
   p1 = Police("武沛齐","队员")
   p1.show_status()
   p1.bomb(["alex","李杰"])
   
   p2 = Police("日天","队长")
   p2.show_status()
   p2.bomb(["alex","李杰"])
   """
   
   
   
   class Terrorist:
       """ 恐怖分子 """
   
       def __init__(self, name, blood=300):
           self.name = name
           self.blood = blood
   
       def shoot(self, police_object):
           """ 开枪射击某个警察 """
           police_object.hit_points -= 5
           police_object.show_status()
           
           self.blood -= 2
   
       def strafe(self, police_object_list):
           """ 扫射某些警察 """
           for police_object in police_object_list:
               police_object.hit_points -= 8
               police_object.show_status()
   
       def show_status(self):
           """ 查看恐怖分子状态 """
           message = "恐怖分子{}的血量值为:{}".format(self.name, self.blood)
           print(message)
   
   """
   t1 = Terrorist('alex')
   t2 = Terrorist('李杰',200)
   """
           
   def run():
       # 1.创建3个警察
       p1 = Police("武沛齐", "队员")
       p2 = Police("苑昊", "队员")
       p3 = Police("于超", "队长")
   
       # 2.创建2个匪徒
       t1 = Terrorist("alex")
       t2 = Terrorist("eric")
       
   
       # alex匪徒射击于超警察
       t1.shoot(p3)
   
       # alex扫射
       t1.strafe([p1, p2, p3])
   
       # eric射击苑昊
       t2.shoot(p2)
   
       # 武沛齐炸了那群匪徒王八蛋
       p1.bomb([t1, t2])
       
       # 武沛齐又炸了一次alex
       p1.bomb([t1])
   
   
   if __name__ == '__main__':
       run()
   ```

> [!IMPORTANT]
> When to use OOP?
> Data Encapsulation Only
> Encapsulate Data + Process Data with Methods.
> Create Multiple Objects with Similar Functionality.
</details>





<details>
<summary><h1>2. Three main characteristics</h1></summary>

Object-oriented programming exists in many languages, and this programming paradigm has three main features: encapsulation, inheritance, and polymorphism.

## 2.1 Encapsulation

Encapsulation is mainly reflected in two aspects:
- Encapsulating similar methods into a class: For example, in the above example, methods related to terrorists are written in the Terrorist class, and methods related to police are written in the Police class.
- Encapsulating data into objects: When instantiating an object, you can encapsulate some data in the object using the __init__ initialization method, making it easier to use later.

## 2.2 Inheritance
In object-oriented programming, there is also the concept that a subclass can inherit methods and class variables from its parent class (it doesn’t copy them; the parent class still owns them, and the subclass can just inherit them).

![Python_File_Operation](/_Python_full_stack/imgs/Module_3_2_2.png)

```python
class Base:

    def func(self):
        print("Base.func")

class Son(Base):
    
    def show(self):
        print("Son.show")
        
s1 = Son()
s1.show()
s1.func() # 优先在自己的类中找，自己没有才去父类。

s2 = Base()
s2.func()
```

> [!IMPORTANT]
> Inhenitanec summary
> When executing object.method, it first looks in the class associated with the current object. If not found, it then looks in its parent class.
> Python supports multiple inheritance: it inherits from the left first, then from the right.
> What is self? It refers to the class corresponding to self to get members. If not found, it follows the inheritance hierarchy upwards.

## 2.3 Ploymorphism

Polymorphism, literally translated, means “many forms.”

- Polymorphism in other programming languages
- Polymorphism in Python

Because Python has no restrictions on data types, it naturally supports polymorphism.

```python
def func(arg):
    v1 = arg.copy() # 浅拷贝
    print(v1)
    
func("武沛齐")
func([11,22,33,44])
```

```python
class Email(object):
    def send(self):
        print("发邮件")

        
class Message(object):
    def send(self):
        print("发短信")
        
        
        
def func(arg):
    v1 = arg.send()
    print(v1)
    

v1 = Email()
func(v1)

v2 = Message()
func(v2)
```

## 2.4 Three main characteristics Summary

- Encapsulation: Encapsulate methods into a class or encapsulate data into an object to facilitate future use.
- Inheritance: Extract common methods from classes into a base class to implement them.
- Polymorphism: Python inherently supports polymorphism (this approach is called duck typing). The simplest example is the following code.
  ```python
  def func(arg):
      v1 = arg.copy() # 浅拷贝
      print(v1)
      
  func("武沛齐")
  func([11,22,33,44])
  ```
</details>

<details>
<summary><h1>3. Extra: Review Data Type</h1></summary>

![Python_File_Operation](/_Python_full_stack/imgs/Module_3_3_1.png)

After getting a basic understanding of object-oriented programming, let’s revisit what we learned earlier: str, list, dict, etc. These are all classes, and you can create different objects from these classes.


```python
# 实例化一个str类的对象v1
v1 = str("武沛齐") 

# 通过对象执行str类中的upper方法。
data = v1.upper()

print(data)
```

</details>


<details>
<summary><h1>4. Summary</h1></summary>

- 1. Relationship between Classes and Objects:
A class is a blueprint for creating objects. An object is an instance of a class. For example, if Person is a class, then person1 and person2 are objects (instances) of that class.
- 2. Common Members in Object-Oriented Programming:
  - Bound Methods: Methods that belong to the class and can be called through the object or the class itself.
  - Instance Variables: Variables that belong to the object and can only be accessed through the object.
- 3. What is self?:
self is a reference to the current instance of the class. It is used to access variables and methods associated with the object.
- 4. Three Main Features of Object-Oriented Programming:
  - Encapsulation: Bundling data and methods that operate on the data into a single unit (class).
  - Inheritance: A mechanism where a new class inherits attributes and methods from an existing class.
  - Polymorphism: The ability to present the same interface for different underlying data types.
- 5. Applications of Object-Oriented Programming:
  - Data Encapsulation: Encapsulating data within an object for easier management and access.
  - Encapsulating Data + Processing with Methods: Encapsulating data and using methods to process and manipulate the data.
  - Creating Multiple Objects with Similar Functionality: Creating multiple objects from the same class, each with the same methods and attributes.
- 6. Additional Information:
  - In Python 3, all classes inherit from object by default, even if it is not explicitly stated. This makes them “new-style” classes.
  - In Python 2, there is a distinction between “classic” classes (which do not inherit from object) and “new-style” classes (which do).

</details>

<details>
<summary><h1>5. OOP Advanced</h1></summary>

This module will include following three parts:

![Python_File_Operation](/_Python_full_stack/imgs/Module_03_5_1.png)

Today’s Summary:

- Members
  - Variables
    - Instance Variables
    - Class Variables
  - Methods
    - Bound Methods
    - Class Methods
    - Static Methods
  - Properties
- Member Modifiers (Public/Private)
- “Object Nesting”
- Special Members

</details>

<details>
<summary><h1>6. Members</h1></summary>
	
- Variables
  - Instance Variables
  - Class Variables
- Methods
  - Bound Methods
  - Class Methods
  - Static Methods
- Properties

### 6.1 Variables
- Instance Variables: Belong to an object, with each object maintaining its own data.
- Class Variables: Belong to the class and can be shared by all objects, generally used to provide common data to objects (similar to global variables).

```python
class Person(object):
    country = "中国"

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def show(self):
        # message = "{}-{}-{}".format(Person.country, self.name, self.age)
        message = "{}-{}-{}".format(self.country, self.name, self.age)  # same output as above
        print(message)

print(Person.country) # 中国


p1 = Person("武沛齐",20)
print(p1.name)
print(p1.age)
print(p1.country) # 中国

p1.show() # 中国-武沛齐-20
```

Q1: Pay attention to the difference between read and write as follows

```python
class Person(object):
    country = "中国"

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def show(self):
        message = "{}-{}-{}".format(self.country, self.name, self.age)
        print(message)

print(Person.country) # 中国

p1 = Person("武沛齐",20)
print(p1.name) # 武沛齐
print(p1.age) # 20
print(p1.country) # 中国
p1.show() # 中国-武沛齐-20

p1.name = "root"     # 在对象p1中讲name重置为root
p1.num = 19          # 在对象p1中新增实例变量 num=19
p1.country = "china" # 在对象p1中新增实例变量 country="china"

print(p1.country)   # china
print(Person.country) # 中国
```

```python
class Person(object):
    country = "中国"

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def show(self):
        message = "{}-{}-{}".format(self.country, self.name, self.age)
        print(message)

print(Person.country) # 中国

Person.country = "美国"


p1 = Person("武沛齐",20)
print(p1.name) # 武沛齐
print(p1.age) # 20
print(p1.country) # 美国
```

Q2: Read and write for inheritance

```python
class Base(object):
    country = "中国"


class Person(Base):

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def show(self):
        message = "{}-{}-{}".format(Person.country, self.name, self.age)
        # message = "{}-{}-{}".format(self.country, self.name, self.age)
        print(message)


# 读
print(Base.country) # 中国
print(Person.country) # 中国

obj = Person("武沛齐",19)
print(obj.country) # 中国

# 写
Base.country = "china"
Person.country = "泰国" # add a class variable within Person class
obj.country = "日本"    # add a instance variable for object obj
```
Interview Questions

```python
class Parent(object):
    x = 1


class Child1(Parent):
    pass


class Child2(Parent):
    pass


print(Parent.x, Child1.x, Child2.x) # 1 1 1

Child1.x = 2
print(Parent.x, Child1.x, Child2.x) # 1 2 1

Parent.x = 3
print(Parent.x, Child1.x, Child2.x) # 3 2 3
```

### 6.2 Methods

- Bound Methods: Have a default self parameter and are called by an object (in this case, self refers to the object calling the method). [Callable by both objects and classes]
- Class Methods: Have a default cls parameter and can be called by either a class or an object (in this case, cls refers to the class calling the method). [Callable by both objects and classes]
- Static Methods: Have no default parameters and can be called by both classes and objects. [Callable by both objects and classes]

```python
class Foo(object):

    def __init__(self, name,age):
        self.name = name
        self.age = age

    def f1(self):
        print("绑定方法", self.name)

    @classmethod
    def f2(cls):
        print("类方法", cls)

    @staticmethod
    def f3():
        print("静态方法")
        
# bounding method （object）
obj = Foo("武沛齐",20)
obj.f1() # Foo.f1(obj)-> not used like this in general, this use bounding method through class


# class method
Foo.f2()  # cls就是当前调用这个方法的类。（类） cls is Foo class
obj.f2()  # cls就是当前调用这个方法的对象的类。 cls is Foo class


# static method
Foo.f3()  # 类执行执行方法（类）
obj.f3()  # 对象执行执行方法
```
In Python, methods are quite flexible and can be called by both objects and classes. However, in languages like Java and C#, bound methods can only be called by objects, while class methods or static methods can only be called by classes.

```python
import os
import requests

class Download(object):

    def __init__(self, folder_path):
        self.folder_path = folder_path

    @staticmethod
    def download_dou_yin():
        # 下载抖音
        res = requests.get('.....')

        with open("xxx.mp4", mode='wb') as f:
            f.write(res.content)

    def download_dou_yin_2(self):
        # 下载抖音
        res = requests.get('.....')
        path = os.path.join(self.folder_path, 'xxx.mp4')
        with open(path, mode='wb') as f:
            f.write(res.content)


obj = Download("video")
obj.download_dou_yin()

```

### 6.3 Property

Properties are actually created by combining bound methods with special decorators, allowing us to call methods without parentheses in the future. For example:
Property = Bounding Method + Decorator
```python
class Foo(object):

    def __init__(self, name):
        self.name = name

    def f1(self):
        return 123

    @property
    def f2(self):
        return 123


obj = Foo("武沛齐")

v1 = obj.f1()
print(v1)

v2 = obj.f2  # No need to use ()
print(v2)
```

There are two ways to write properties:
- Method 1, use decorator
```python
  class C(object):
      
      @property
      def x(self):
          pass
      
      @x.setter          # very rare to use
      def x(self, value):
          pass
      
      @x.deleter
      def x(self):
  		pass
          
  obj = C()
  
  obj.x
  obj.x = 123
  del obj.x
```
- Method 2, use defineing variables
```python
  class C(object):
      
      def getx(self): 
  		pass
      
      def setx(self, value): 
  		pass
          
      def delx(self): 
  		pass
          
      x = property(getx, setx, delx, "I'm the 'x' property.")
      
  obj = C()
  
  obj.x
  obj.x = 123
  del obj.xa
```


Django源码一撇：

```python
class WSGIRequest(HttpRequest):
    def __init__(self, environ):
        script_name = get_script_name(environ)
        # If PATH_INFO is empty (e.g. accessing the SCRIPT_NAME URL without a
        # trailing slash), operate as if '/' was requested.
        path_info = get_path_info(environ) or '/'
        self.environ = environ
        self.path_info = path_info
        # be careful to only replace the first slash in the path because of
        # http://test/something and http://test//something being different as
        # stated in https://www.ietf.org/rfc/rfc2396.txt
        self.path = '%s/%s' % (script_name.rstrip('/'),
                               path_info.replace('/', '', 1))
        self.META = environ
        self.META['PATH_INFO'] = path_info
        self.META['SCRIPT_NAME'] = script_name
        self.method = environ['REQUEST_METHOD'].upper()
        # Set content_type, content_params, and encoding.
        self._set_content_type_params(environ)
        try:
            content_length = int(environ.get('CONTENT_LENGTH'))
        except (ValueError, TypeError):
            content_length = 0
        self._stream = LimitedStream(self.environ['wsgi.input'], content_length)
        self._read_started = False
        self.resolver_match = None

    def _get_scheme(self):
        return self.environ.get('wsgi.url_scheme')

    def _get_post(self):
        if not hasattr(self, '_post'):
            self._load_post_and_files()
        return self._post

    def _set_post(self, post):
        self._post = post

    @property
    def FILES(self):
        if not hasattr(self, '_files'):
            self._load_post_and_files()
        return self._files

    POST = property(_get_post, _set_post)
```

obj.POST             # run _get_post
obj.POST = ...       # run _set_post

</details>

<details>
<summary><h1>7. Members Decorators</h1></summary>

In Python, member modifiers refer to public and private.

- Public: These members can be accessed from anywhere.
- Private: These members can only be accessed within the class itself (members that start with two underscores are considered private).

```python
class Foo(object):

    def __init__(self, name, age):
        self.__name = name      # Private, can be used internally, can not be used externally
        self.age = age          # Public

    def get_data(self):
        return self.__name

    def get_age(self):
        return self.age


obj = Foo("武沛齐", 123)


# 公有成员
print(obj.age)
v1 = self.get_age()
print(v1)

# 私有成员
# print(obj.__name) # 错误，由于是私有成员，只能在类中进行使用。
v2 = obj.get_data()
print(v2)
```

Example 2:
```python
class Foo(object):

    def get_age(self):
        print("公有的get_age")

    def __get_data(self):             # Private Method, can only be used within class
        print("私有的__get_data方法")

    def proxy(self):
        print("公有的proxy")
        self.__get_data()


obj = Foo()
obj.get_age()

obj.proxy()
```

Example 3:
```python
class Foo(object):

    @property
    def __name(self):
        print("公有的get_age")

    @property
    def proxy(self):
        print("公有的proxy")
        self.__name
        return 1

obj = Foo()
v1 = obj.proxy
print(v1)

```
> [!IMPORTANT]
> Special reminder: Private members in the parent class cannot be inherited by the subclass.

> [!IMPORTANT]
> In conclusion, private members are generally not accessible from outside the class. However, with some special syntax, it is possible (this is seen in the Flask source code, but it is not recommended to write code this way).

When to use private or public? Can members be exposed as independent functions for external use?
- Yes, use public.
- No, use private and serve as an auxiliary function within the class.

</details>

<details>
<summary><h1>8. Object Nesting</h1></summary>

Example 1:
```python
class Student(object):
    """ 学生类 """

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def message(self):
        data = "我是一名学生，我叫:{},我今年{}岁".format(self.name, self.age)
        print(data)

s1 = Student("武沛齐", 19)
s2 = Student("Alex", 19)
s3 = Student("日天", 19)

class Classes(object):
    """ 班级类 """

    def __init__(self, title):
        self.title = title
        self.student_list = []

    def add_student(self, stu_object):
        self.student_list.append(stu_object)

    def add_students(self, stu_object_list):
        for stu in stu_object_list:
            self.add_student(stu)

    def show_members(self):
        for item in self.student_list:
            # print(item)
            item.message()

c1 = Classes("三年二班")
c1.add_student(s1)
c1.add_students([s2, s3])

print(c1.title)
print(c1.student_list)
```

Example 2:
```python
class Student(object):
    """ 学生类 """

    def __init__(self, name, age, class_object):
        self.name = name
        self.age = age
        self.class_object = class_object

    def message(self):
        data = "我是一名{}班的学生，我叫:{},我今年{}岁".format(self.class_object.title, self.name, self.age)
        print(data)


class Classes(object):
    """ 班级类 """

    def __init__(self, title, school_object):
        self.title = title
        self.school_object = school_object


class School(object):
    """ 学校类 """

    def __init__(self, name):
        self.name = name


s1 = School("北京校区")
s2 = School("上海校区")

c1 = Classes("Python全栈", s1)
c2 = Classes("Linux云计算", s2)

user_object_list = [
    Student("武沛齐", 19, c1),
    Student("Alex", 19, c1),
    Student("日天", 19, c2)
]
for obj in user_object_list:
    print(obj.name, obj.class_object.title ,  obj.class_object.school_object.name)

```
</details>

<details>
<summary><h1>9. Special Members</h1></summary>

In Python classes, there are some special methods that follow the __method__ format. These methods have special meanings internally. Next, let’s discuss some common special members:

- '__init__', initialization method
```python
  class Foo(object):
      def __init__(self, name):
          self.name = name
  
  
  obj = Foo("武沛齐")
```
- '__new__', construction method. Was executed before __init__, create a object
  ```python
  class Foo(object):
      def __init__(self, name):
          print("第二步：初始化对象，在空对象中创建数据")
          self.name = name
  
      def __new__(cls, *args, **kwargs):
          print("第一步：先创建空对象并返回")
          return object.__new__(cls)
  
  
  obj = Foo("武沛齐")
  ```
  
- '__call__'
  ```python
  class Foo(object):
      def __call__(self, *args, **kwargs):
          print("执行call方法")
  
  
  obj = Foo()
  obj()    # object + () will trigger __call__ method
  ```
  
- '__str__'

  ```python
  class Foo(object):
  
      def __str__(self):
          return "哈哈哈哈"
  
  
  obj = Foo()
  data = str(obj)
  print(data)
  ```
  
- '__dict__'
get all instance variables and transfer it to dictionary
  ```python
  class Foo(object):
      def __init__(self, name, age):
          self.name = name
          self.age = age
  
  
  obj = Foo("武沛齐",19)
  print(obj.__dict__)
  ```
  
- '__getitem__', '__setitem__', '__delitem__'
  ```python
  class Foo(object):
  
      def __getitem__(self, item):            # support obj['x2']
          pass
  
      def __setitem__(self, key, value):      # support obj['x2'] = 123
          pass
  
      def __delitem__(self, key):             # support del obj['x2']
          pass
  
  
  obj = Foo("武沛齐", 19)
  
  obj["x1"]         # trigger __getitem__
  obj['x2'] = 123   # trigger __setitem__
  del obj['x3']     # trigger __delitem__
  ```
- '__enter__', '__exit__'

  ```python
  class Foo(object):
  
      def __enter__(self):
          print("进入了")
          return 666
  
      def __exit__(self, exc_type, exc_val, exc_tb):
          print("出去了")
  
  
  obj = Foo()
  with obj as data:
      print(data)
  ```
with ... as f, will trigger __enter__ method
when with is completed, will trigger __exit__ method
- '__add__'
```python
  class Foo(object):
      def __init__(self, name):
          self.name = name
  
      def __add__(self, other):
          return "{}-{}".format(self.name, other.name)
  
  
  v1 = Foo("alex")
  v2 = Foo("sb")
  
  # 对象+值，内部会去执行 对象.__add__方法，并将+后面的值当做参数传递过去。
  v3 = v1 + v2
  print(v3)
```
  
- '__iter__'
  - Iterators
    ```python
    # 迭代器类型的定义：
        1.当类中定义了 __iter__ 和 __next__ 两个方法。
        2.__iter__ 方法需要返回对象本身，即：self
        3. __next__ 方法，返回下一个数据，如果没有数据了，则需要抛出一个StopIteration的异常。
    	官方文档：https://docs.python.org/3/library/stdtypes.html#iterator-types
            
    # 创建 迭代器类型 ：
    	class IT(object):
            def __init__(self):
                self.counter = 0
    
            def __iter__(self):
                return self
    
            def __next__(self):
                self.counter += 1
                if self.counter == 3:
                    raise StopIteration()
                return self.counter
    
    # 根据类实例化创建一个迭代器对象：
        obj1 = IT()
        
        # v1 = obj1.__next__()
        # v2 = obj1.__next__()
        # v3 = obj1.__next__() # 抛出异常
        
        v1 = next(obj1) # obj1.__next__()
        print(v1)
    
        v2 = next(obj1)
        print(v2)
    
        v3 = next(obj1)
        print(v3)
    
    
        obj2 = IT()
        for item in obj2:  # 首先会执行迭代器对象的__iter__方法并获取返回值，一直去反复的执行 next(对象) 
            print(item)
            

    ```
An iterator object supports value retrieval through the next function. If the retrieval ends, it automatically raises a StopIteration exception. In a for loop, the __iter__ method is first executed to get an iterator object, and then the next function is repeatedly called to retrieve values (the loop terminates if a StopIteration exception is raised).

  - Generators
    ```python
    # 创建生成器函数
        def func():
            yield 1
            yield 2
        
    # 创建生成器对象（内部是根据生成器类generator创建的对象），生成器类的内部也声明了：__iter__、__next__ 方法。
        obj1 = func()
        
        v1 = next(obj1)
        print(v1)
    
        v2 = next(obj1)
        print(v2)
    
        v3 = next(obj1)
        print(v3)
    
    
        obj2 = func()
        for item in obj2:
            print(item)
    
    如果按照迭代器的规定来看，其实生成器类也是一种特殊的迭代器类（生成器也是一个中特殊的迭代器）。
    ```
  - Iterable Objects
    ```python
    # 如果一个类中有__iter__方法且返回一个迭代器对象 ；则我们称以这个类创建的对象为可迭代对象。
    
    class Foo(object):
        
        def __iter__(self):
            return 迭代器对象(生成器对象)
        
    obj = Foo() # obj是 可迭代对象。
    
    # 可迭代对象是可以使用for来进行循环，在循环的内部其实是先执行 __iter__ 方法，获取其迭代器对象，然后再在内部执行这个迭代器对象的next功能，逐步取值。
    for item in obj:
        pass
    ```

    ```python
    class IT(object):
        def __init__(self):
            self.counter = 0
    
        def __iter__(self):
            return self
    
        def __next__(self):
            self.counter += 1
            if self.counter == 3:
                raise StopIteration()
            return self.counter
    
    
    class Foo(object):
        def __iter__(self):
            return IT()
    
    
    obj = Foo() # 可迭代对象
    
    
    for item in obj: # 循环可迭代对象时，内部先执行obj.__iter__并获取迭代器对象；不断地执行迭代器对象的next方法。
        print(item)
    ```

</details>

<details>
<summary><h1>10. Summary</h1></summary>

- Members in Object-Oriented Programming
  - Variables
    - Instance Variables
    - Class Variables
  - Methods
    - Bound Methods
    - Class Methods
    - Static Methods
  - Properties
- Member Modifiers
- Data Nesting in Objects
- Special Members
- Important Concepts:
  - Iterators
  - Generators
  - Iterable Objects

1. What is OOP, and why is it important in Python?
Answer:

OOP, or Object-Oriented Programming, is a programming paradigm used in many languages, including Python. In OOP, you can model real-world concepts using classes and objects. You can break down complex problems into smaller, more manageable parts. This makes the code reusable, maintainable, and scalable. 

In Python, OOP is vital because it allows you to write more structured and efficient code. OOP in Python supports code reusability through inheritance. Using OOP in Python can make your development process more efficient and your code more readable.

2. What is a class in Python?
Answer:

A class in Python is like a blueprint for creating objects. A class defines properties and methods that are common to all objects created from it. 

In Python, you use the class keyword to define a class. 

Classes help organize code by grouping related attributes and functions, promoting reusability and modularity. For instance, if you’re creating a program to manage a library, you might have classes for books, authors, and borrowers, each with its specific attributes and methods.

3. What is an object in Python?
Answer:

An object in Python is an instance of a class. You can think of it as a specific realization of the blueprint provided by the class. 

An object contains data in the form of attributes and code in the form of methods. When you create an object, you are essentially creating a variable that has all the properties and behaviors defined in the class. 

For example, if you have a class for cars, an object could represent a specific car, such as a Honda Civic, with attributes like color and speed, and methods like start and stop.

4. Bounding methods vs Class methods vs Static methods?
Bounding methods: Bound methods have an object associated with them. 
Class methods: Class methods are bound to the class and are usually called on the class itself, but can also be called on an instance of the class. When called on an instance, the instance is automatically passed as the first argument. Class methods are useful when you need to access class-specific data or implement factory methods. 
Static methods: Static methods are not bound to the class or an instance of the class, and they don't have access to either the class or the instance. Static methods are similar to class level methods, but they're bound to the class instead of the class's objects. This means that you can call a static method for a class without needing an object for that class. Static methods are useful for utility functions that don't depend on class or instance state. 

4. How do you create an instance of a class in Python?
Answer:

In Python, you can create a class instance by calling the class as a function and providing necessary arguments. This process is known as instantiation. Once you create an instance, you can access the attributes and methods defined in the class using the instance.

5. What is the constructor in a Python class?
In Python, a constructor is a special method that is automatically called when an object of a class is created. It is used to initialize the object's attributes.
The constructor method in Python is named __init__.
A constructor in Python is a special method called when an object is created. Its purpose is to assign values to the data members within the class when an object is initialized.

6. What is a decorator in Python? How is it related to OOP?
A decorator in Python is a function that you can use to modify or extend the behavior of other functions or methods without changing their code. In the context of OOP, decorators can be used to add functionality to methods within classes, such as logging or access control. 

You can think of decorators as wrappers around a function or method. When you call a decorated function, the decorator’s code runs first, allowing you to perform actions before or after the original function. It helps keep the code clean and adhere to the OOP reusability principle.

7. What is the purpose of the self keyword in methods?
The self keyword in Python is used within a class to refer to the instance of the object itself. When you define a method inside a class, the first parameter is usually named self. It helps you access the attributes and methods of the class within the current object’s context.

8. How is inheritance implemented in Python?
Inheritance allows a class to inherit attributes and methods from another class. You can create a new class – known as the child class – based on an existing class – known as the parent class – and add new features or modify existing ones. 

To implement inheritance in Python, you define the new class and put the name of the parent class in parentheses.

9. What is the use of the super() function?
Answer:

Python’s super() function is used within a class to call a method from a parent class, often within the context of method overriding. 

If you have a method in a child class with the same name as a method in the parent class, you can use super() to call the parent method within the child method. This is particularly useful when you want to extend or modify the behavior of the parent method in the child class. 

By using super(), you ensure that your code follows the inheritance hierarchy. It also makes the code more maintainable, as changes in the parent class can be easily propagated to child classes.

10. What is the purpose of the @property decorator?
Answer:

In Python, the @property decorator allows you to treat a method as a property of the class. By using this, you can create a “getter” method, which enables you to access a class method as though it’s an attribute without needing to write parentheses when you call it. This means you can control how the attribute is accessed without directly exposing it.

12. What is a static method and how is it different from a class method?
Answer:

A static method in Python belongs to a class rather than an instance of the class. You can define it using the @staticmethod decorator. It doesn’t require reference to the class or its instance and can be called on the class itself. Unlike regular instance methods, it doesn’t take a self parameter. 

A class method, on the other hand, is defined with the @classmethod decorator and takes a reference to the class itself as its first parameter – usually named “cls.” It can access and modify class-level attributes, while a static method can’t. 

A class method is more versatile as subclasses can override it, whereas a static method remains unchanged.

13. How do you implement abstract classes and methods in Python?
Answer:

In Python, you can implement abstract classes and methods using the ABC module. You would first import the module and then create a class that inherits from ABC, which stands for abstract base class.

14. How can you prevent method overriding in Python?
Answer:

In Python, method overriding is a common feature that allows a subclass to provide a different implementation of a method defined in its superclass. However, if you want to prevent method overriding, you can do so by defining the method as private. 

By prefixing the method’s name with a double underscore (__), you make it private to the class, and it cannot be overridden in a subclass.

15. How does polymorphism work in Python?
Answer:

Polymorphism in Python allows different objects to be treated as instances of the same class, even if they belong to different classes. You can achieve this through inheritance, where a subclass can have methods with the same name as the methods in the superclass. 

When you call a method on an object, Python will automatically use the correct method based on the object’s class, even if the object is referred to by a variable of the superclass type. 

This enables more flexible and reusable code, as you can write functions that work with different classes, provided they adhere to the same interface or method signature.

16. what is interfaces in python
https://realpython.com/python-interface/
https://stackoverflow.com/questions/2124190/how-do-i-implement-interfaces-in-python
https://medium.com/@shashikantrbl123/interfaces-and-abstract-classes-in-python-understanding-the-differences-3e5889a0746a
https://discuss.python.org/t/difference-between-interface-and-abstracts/33315

17. What is Python design pattern?
设计模式是一套被广泛接受且行之有效的编程经验。它提供了一组通用的解决方案，可以应用于各种编程场景。设计模式的出现是为了解决软件开发中的一些常见问题，如代码重用、系统可扩展性、代码可读性等。
使用设计模式的好处如下：

代码复用：通过使用设计模式，可以将代码分解和组合以实现代码复用。
系统可扩展性：设计模式可以使系统更加灵活，易于扩展，并且能够适应不同的需求。
代码可读性：使用设计模式可以提高代码的可读性，使代码更加清晰。


https://www.testgorilla.com/blog/python-oops-interview-questions/
https://medium.com/@edwinvarghese4442/top-oops-questions-asked-in-python-interviews-8bd01623e6f5
https://www.geeksforgeeks.org/oops-interview-questions/
https://www.mygreatlearning.com/blog/oops-interview-questions/
https://medium.com/@mohsin.shaikh324/50-essential-questions-on-python-object-oriented-programming-oop-98f9605ca95f
https://www.simplilearn.com/tutorials/java-tutorial/oops-interview-questions
https://www.foundit.in/career-advice/python-oops-interview-questions/
https://www.interviewbit.com/oops-interview-questions/

</details>





























