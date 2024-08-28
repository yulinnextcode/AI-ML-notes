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



</details>









































