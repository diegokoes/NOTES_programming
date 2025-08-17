# PYTHON > OOP

everything comes from object.
inheritance can be multiple, unlike [[java_oop|java's inheritance model]]
## SUMMARY
> [!summary]
>
## THEORY

## QUESTIONS
> [!tip]- Question
> Answer

> [!warning]- Question
> Answer

> [!danger]- Question
> Answer


# OOP PRINCIPLES

This reading introduces you to the OOP principles in more detail using some examples.

The object oriented paradigm was introduced in the 1960s by Alan Kay. At the time, the paradigm was not the best computing solution given the small scalability of software developed then. As the complexity of software and real-life applications improved, object oriented principles became a better solution.

You previously encountered the four main pillars of object oriented programming. These are: encapsulation, polymorphism, inheritance and abstraction. Let's look at a few examples that demonstrate how these principles translate when using Python.

## ENCAPSULATION

The idea of encapsulation is to have methods and variables within the bounds of a given unit. In the case of Python, this unit is called a class. And the members of a class become locally bound to that class. These concepts are better understood with scope, such as global scope (which in simple terms is the files I am working with), and local scope (which refers to the method and variables that are 'local' to a class). Encapsulation thus helps in establishing these scopes to some extent.

For example, the Little Lemon company may have different departments such as inventory, marketing and accounts. And you may be required to deal with the data and operations for each of them separately. Classes and objects help in encapsulating and in turn restrict the different functionalities.

Encapsulation is also used for hiding data and its internal representation. The term for this is information hiding. Python has a way to deal with it, but it is better implemented in other programming languages such as Java and C++. Access modifiers represented by keywords such as public, private and protected are used for information hiding. The use of single and double underscores for this purpose in Python is a substitute for this practice. For example, let's examine an example of protected members in Python.

class Alpha:

def __init__(self):

self._a = 2. # Protected member ‘a’

self.__b = 2. # Private member ‘b’

self._a is a protected member and can be accessed by the class and its subclasses.

Private members in Python are conventionally used with preceding double underscores: __. self.__b is a private member of the class Alpha and can only be accessed from within the class Alpha.

It should be noted that these private and protected members can still be accessed from outside of the class by using public methods to access them or by a practice known as name mangling. Name mangling is the use of two leading underscores and one trailing underscore, for example:

__class_identifier

Class is the name of the class and identifier is the data member that I want to access.

## POLYMORPHISM

Polymorphism refers to something that can have many forms. In this case, a given object. Remember that everything in Python is inherently an object, so when I talk about polymorphism, it can be an operator, method or any object of some class. I can illustrate the case for polymorphism using built-in functions and operations, for example:

string = "poly"

num = 7

sequence = [1,2,3]

new_str = string * 3

new_num = 7 * 3

new_sequence = sequence * 3

  

print(new_str, new_num, new_sequence)

The output is:

polypolypoly 21 [1, 2, 3, 1, 2, 3, 1, 2, 3]

In this case, the * operator exhibits polymorphism. It works differently depending on the type of object it's applied to:

- When applied to a string, it repeats the string.
    
- When applied to an integer, it performs multiplication.
    
- When applied to a list, it repeats the list.
    

This is an example of polymorphism because the same operator * is behaving differently depending on the type of object.

Let's examine one more example.

string = "poly"

sequence = [1,2,3]

print(len(string))

print(len(sequence))

The output is:

4

3

In this example, the len() function is polymorphic because it calculates the length of different types of objects:

- For the string "poly", it returns 4 because there are 4 characters in the string.
    
- For the list [1, 2, 3], it returns 3 because there are 3 elements in the list.
    

Despite len() being the same function, it works differently depending on the type of the object passed to it.

## INHERITANCE

Inheritance in Python will be covered later in the course, but the basic template for it is as follows:

class Parent:

Members of the parent class

  

class Child(Parent):

Inherited members from parent class

Additional members of the child class

As the structure of inheritance gets more complicated, Python adheres to something called the **Method Resolution Order (MRO)** that determines the flow of execution. MRO is a set of rules, or an algorithm, that Python uses to implement **monotonicity**, which refers to the order or sequence in which the interpreter will look for the variables and functions to implement. This also helps in determining the scope of the different members of the given class.

### ABSTRACTION

Abstraction can be seen both as a means for hiding important information as well as unnecessary information in a block of code. The core of abstraction in Python is the implementation of something called abstract classes and methods, which can be implemented by inheriting from something called the abc module. "abc" here stands for abstract base class. It is first imported and then used as a parent class for some class that becomes an abstract class. Its simplest implementation can be done as below.

from abc import ABC,

class ClassName(ABC):

pass

You will cover abstraction in a little more detail later in the course.

# INHERITANCE AND MULTIPLE INHERITANCE

Let’s say there are two classes, namely class A and class B. If you have to perform simple inheritance, it can be done as follows:

5

If class A is the parent class and class B is inheriting from it, then class A is passed inside class B as a parameter. This will allow class B to directly access the attributes and methods inside class A.

## **MULTIPLE INHERITANCE**

You have learned about single inheritance so far, but Python also gives us the ability to perform multiple inheritance between classes.

Here is a simple example of how that can be done.

6

7

8

9

10

11

12

The output is:

1

First, two classes called A and B are created and then variables a and b respectively are initialized with values. A new class C is then defined and classes A and B are passed to it. This is how multiple inheritance is done in Python. The order of classes is important, but not in this specific example. I then instantiate an object ‘c’ of class C. The values of the a and b variables are printed over the object c of class C even though a and b are not present inside class C.

The code above is an example of multiple inheritance. There are also other types of inheritance that fall under the umbrella of multiple inheritance. Let's examine an example.

## **MULTI-LEVEL INHERITANCE**

1

2

3

4

5

6

7

8

9

10

11

The output is 2 because C derives from the immediate super class of C, and that's B. Even though class A is the top-level base class, class C does not directly inherit from A but instead inherits the attribute a from B and B has the attribute value of its member a = 2.

The example above demonstrates multi-level inheritance where the derived class C inherits from class B. The class B is in turn a derived class of base class A. Class B here is an intermediate derived class. There are three levels of inheritance in this case, but it could be extended to many levels, though it may become impractical after a while.

## **BUILT-IN FUNCTIONS**

There are two built-in functions that can come in handy when trying to find the relationship between different classes and objects: issubclass() and isinstance().

The first one, issubclass() is demonstrated below.

1

Two classes are passed as arguments to this function and a Boolean result is returned. The above example can be extended as follows.

1

2

The output is:

1

2

This illustrates how the child class is passed as the first argument. To avoid confusion, this can be read as: “Is B subclass of A?“ You can see the result is "True" in the second case where child B is the subclass.

Another built-in function similar to this one is **isinstance()** that determines if some object is an instance of some class. So if I write:

1

2

3

4

5

6

7

8

The output that I will get is "True".

Now that you know how classes can be extended from other classes, let's look at another useful built-in function called the super() function.

The super() function is a built-in function that can be called inside the derived class and gives access to the methods and variables of the parent classes or sibling classes. Sibling classes are the classes that share the same parent class. When you call the super() function, you get an object that represents the parent class in return.

The super() function plays an important role in multiple inheritance and helps drive the flow of the code execution. It helps in managing or determining the control of from where I can draw the values of my desired functions and variables.

If you change anything inside the parent class, there is a direct retrieval of changes inside the derived class. This is mainly used in places where you need to initialize the functionalities present inside the parent class in the child class as well. You can then add additional code in the child class.

Here is an example.

1

2

3

4

5

6

7

8

9

10

11

The output is:

1

2

In the code above, if I had commented out the line for super() function, the output is:

1

This happened because when you initialize the child class, you don’t initialize the base class with it. super() function helps you to achieve this and add the initialization of base class with the derived class.

## ABSTRACCT CLASSES
cant create an instance
python doesnt support abstraction, module
methods defined before being implemented
methods must be implemented in child class
super()?


## MODULES 
from abc import ABC, abstractmethod
class SomeAbstractClass(ABC):
	 @abstractmethod
	 dedf someabstractmethod(self):
		pass
## METHOD RESOLUTION ORDER

types of inheritance in python

What about in an example where classes is inheriting from two classes,

old dfs, now c3? 

c3 adheres to monotonicity, follows inhjeritance graph, visits super class after local classes.

class A:
    def c(self):
        return "Function inside A"

class B:
    def c(self):
        return "Function inside B"

class C(A, B):
    def c(self):
        return "Function inside C"

class D(A, C):
    pass

d = D()
print(d.c())


Traceback (most recent call last):
  File "/Users/intropython/PycharmProjects/practicePython/inherit.py", line 10, in <module>
    class D(A, C):
TypeError: Cannot create a consistent method resolution
order (MRO) for bases A, C

Note that this throws an error. In the code above, class D inherits from both class A and class C.

Class C is its immediate superclass, but since this is multiple inheritance, the rules are more complicated and it also has to check the classes passed to it for precedence.

In this particular case, class D is unable to resolve the order that should be followed, while resolving the value for the variable in cases where the variable is not present in the class of the given object.

It results in a TypeError because it's unable to create method resolution order (MRO). MRO is Python’s way of resolving the order of precedence of classes while dealing with inheritance.

Let's examine one final example.

class A:
    def c(self):
        return "Function inside A"

class B(A):
    def c(self):
        return "Function inside B"

class C(A,B):
    pass

class D(C):
    pass

d = D()
print(d.a)

Error on line 9:
    class C(A,B):
TypeError: Cannot create a consistent method resolution
order (MRO) for bases A, B

class A:
    pass

class B(A):
    pass

class C(B):
    pass


c = C()
print(c.a())

Error on line 12:
    print(c.a())
AttributeError: 'C' object has no attribute 'a


mro() and help()