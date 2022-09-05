# OOP concept
We write programes using classess and objects utilising features of OOPs such as **Abstraction**, **Encapsulation**,
**Inheritance**, **Polymorphism**

## Class and Object
- Class holds **data**, has **methods** that interact with that data and is used to **instantiate objects**
- Object is an instance of a class

## Abstraction
- A process of hiding irrelevant details from user
EG: when you send an SMS you just type the message, select the contract and click send, the phone shows you that the
message has been sent, what acctually happens in background when you click send is hidden from you as it's not relevant
to you.

## Encapsulation
- A process of combining data and function into a single unit like capsule. This is to advoid the access of private data
members from outside the class.
- We make all data members of class private and create public functions, using them we can get the values from these
  data members or set the value to these data members.
All methods, no matter the access control, can be accessed within the class. But what about outside callers?
Public methods enforce no access control -- they can be called in any scope.
Protected methods are only accessible to other objects of the same class.
Private methods are only accessible within the context of the current object.

## Inheritance
- A feature using which an object of child class acquires the properties of parent class

```ruby
class Animal
  def walk
  end
end

class Dog < Anima
  def balk
  end
end
```

## Polymorphism
Function overloading or operator overloading
- Function overloading: we can have more than one function with same name but different numbers, type or sequence of
  arguments.
A feature using which an object behaves differently in different situation
