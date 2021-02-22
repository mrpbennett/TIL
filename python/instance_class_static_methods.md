# Instance, Class, and Static Methods

```python
class MyClass:
    def method(self):
        return 'instance method called', self

    @classmethod
    def classmethod(cls):
        return 'class method called', cls

    @staticmethod
    def staticmethod():
        return 'static method called'
```

## Instance Methods

The first method on `MyClass`, called method, is a regular instance method. That’s the basic, no-frills method type you’ll use most of the time. You can see the method takes one parameter, self, which points to an instance of MyClass when the method is called (but of course instance methods can accept more than just one parameter).

Through the self parameter, instance methods can freely access attributes and other methods on the same object. This gives them a lot of power when it comes to modifying an object’s state.

Not only can they modify object state, instance methods can also access the class itself through the self.__class__ attribute. This means instance methods can also modify class state.

```python
obj = MyClass()
obj.method()

# ('instance method called', <MyClass instance at 0x10205d190>)
```
This confirmed that method (the instance method) has access to the object instance (printed as <MyClass instance>) via the self argument.

When the method is called, Python replaces the self argument with the instance object, obj. 


## Class Methods
Let’s compare that to the second method, MyClass.classmethod. I marked this method with a @classmethod decorator to flag it as a class method.

Instead of accepting a self parameter, class methods take a cls parameter that points to the class—and not the object instance—when the method is called.

Because the class method only has access to this cls argument, it can’t modify object instance state. That would require access to self. However, class methods can still modify class state that applies across all instances of the class.

```python
obj.classmethod()
# ('class method called', <class MyClass at 0x101a2f4c8>)
```
Calling `classmethod()` showed us it doesn’t have access to the `<MyClass instance>` object, but only to the `<class MyClass>` object, representing the class itself (everything in Python is an object, even classes themselves).

Notice how Python automatically passes the class as the first argument to the function when we call MyClass.classmethod()

## Static Methods
The third method, MyClass.staticmethod was marked with a @staticmethod decorator to flag it as a static method.

This type of method takes neither a self nor a cls parameter (but of course it’s free to accept an arbitrary number of other parameters).

Therefore a static method can neither modify object state nor class state. Static methods are restricted in what data they can access - and they’re primarily a way to namespace your methods.

```python
obj.staticmethod()
# 'static method called'
```
Did you see how we called staticmethod() on the object and were able to do so successfully?

This confirms that static methods can neither access the object instance state nor the class state. They work like regular functions but belong to the class’s (and every instance’s) namespace.

Above references link [link](https://realpython.com/instance-class-and-static-methods-demystified/#instance-methods)
