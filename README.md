
# Instance Methods

## Introduction
Now that you know what classes and instances are, you can start to talk about instance methods. Instance methods are almost the same as regular functions in python. The key difference is that an instance method is defined inside of a class and bound to instance objects of that class. Instance methods can be thought of as an attribute of an instance object. The difference between an instance method and another attribute of an instance is that instance methods are `callable`, meaning they execute a block of code. This may seem a bit confusing, but try to think about instance methods as functions defined in a class that are really just attributes of an instance object from that class.

## Objectives

You will be able to:

* Compare instance methods and attributes
* Define and call an instance method
* Define instance attributes
* Explain the `self` variable and its relation to instance objects
* Create an instance of a class

## Instance Methods as Attributes

When you think of methods and functions, you think about what kind of action they perform. The same goes for instance methods, however, the action being performed is scoped directly to that instance object. Remember, classes are kind of like the blueprints for their instance objects. So, let's take the example of a **Dog** class. What are the things that all dogs do? They can bark, beg to go for a walk, chase squirrels, etc. So, you consider these more or fewer attributes of a dog -- the same as their name, breed, weight, age, etc. When you create a new dog instance object, the dog should be able to automatically bark, beg, and chase squirrels.

Let's see how you would create a single dog, `rex`, and get him to bark:


```python
class Dog():
    pass 
rex = Dog()
rex
```




    <__main__.Dog at 0x1086a2a58>




```python
rex.bark()
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-2-d177611622ff> in <module>()
    ----> 1 rex.bark()
    

    AttributeError: 'Dog' object has no attribute 'bark'


Okay, here you have an instance of the dog class, but as you can see rex cannot bark yet... Let's see if you can fix that. You said instance methods are basically functions that are ***callable*** attributes, like functions, of an instance object. So, let's write a function that returns the string `"bark!"`, and assign it as an attribute of a dog.

> **Note:** Dictionary object attributes are accessed using the bracket (`[]`) notation. However, instance object attributes are accessed using the dot (`.`) notation. 


```python
def make_a_bark():
    return "ruff ruff!"

rex.bark = make_a_bark
rex.bark
```




    <function __main__.make_a_bark()>



Here you can see that you successfully added the `bark` attribute to `rex` and assigned it the function `make_a_bark`. Note that the return value of `rex.bark` is simply a function signature since you have not yet executed the function, and although this looks like an instance method it is not quite.

> **Note:** Although you may hear and see the terms method and function used interchangeably, there are slight differences. You know that a function is essentially an object that contains a block of code and it can optionally take in data or objects as explicit parameters, operate on them, and optionally return a value. A method is, simply put, a function that is bound to a class or instances of that class. Instance methods, thus, are functions that are available/bound to instance objects of the class in which they are defined. However, a key difference between the two is that a method is *implicitly* passed to the object on which it is called, meaning the first parameter for the method is the object. Don't worry if this is confusing as you will dive more into this later.


```python
rex.bark()
```




    'ruff ruff!'



Great, now you see that once you **called** the `bark` attribute, you executed the function and now see our return value, `"ruff ruff!"`. 

This is a great first step. However, since `make_a_bark` is not actually defined inside our class, you are able to call it without our dog instance object, `rex`, and as you just covered, that's not really how instance methods work... 


```python
make_a_bark()
```




    'ruff ruff!'



Alright, so, how do you turn this into a real instance method? Well, the first thing you need to do is define it inside of our class. So, let's take a look at how you can do that below:

## Defining an Instance Method


```python
class Dog():
    
    def bark():
        return "I'm an instance method! Oh and... ruff ruff!"
```


```python
new_rex = Dog()

new_rex.bark
```




    <bound method Dog.bark of <__main__.Dog object at 0x1087375c0>>



Here, you have re-defined our Dog class, but this time you actually defined an *instance method* `bark`. Now, whenever you create a new instance of this new Dog class, that instance will have the bark instance method as an attribute. 

Notice that the signature that is returned by the unexecuted method says **bound `method`** instead of function, as was the case with our first `rex`'s bark. 

There is **one** issue with our instance method. Let's try calling it and see what happens:


```python
new_rex = Dog()
new_rex.bark()
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-8-6e1d350549b7> in <module>()
          1 new_rex = Dog()
    ----> 2 new_rex.bark()
    

    TypeError: bark() takes 0 positional arguments but 1 was given


Uh oh! `TypeError: bark() takes 0 positional arguments but 1 was given`. This error is telling you that the method, `bark` was defined to take 0 arguments, but when you executed it, you gave it an argument. 

Remember that one of the key differences between functions and methods is that a method is bound to an object and **implicitly** passes the object as an argument. That is what is causing this error. Effectively, what is happening when you try to call the instance method is this:

```python
# the instance object, new_rex, is implicitly passed in as the first argument upon execution
new_rex.bark(new_rex)
```

So, how do you fix this error? Well, if instance methods will always require a default argument of the instance object, you will need to define our instance methods with an *explicit* first parameter.

>**Note:** Parameters are the variable names you give to our method or function's future data. They are called parameters when you talk about the definition of a method or function, but once you have the data they are arguments. 

```python
def function_example(parameter1, parameter2):
    return parameter1 + parameter2
# since you are defining the function, the variables, parameter1 and parameter2, are called parameters

function_example("Argument1", "Argument2") 
# here the strings passed in, "Argument1" and "Argument2", are arguments since you are executing the function
```

Okay, so let's see if when you define our function with a parameter, you can get rid of the error. You'll also define another function `who_am_i` to help further understand what's happening here.

## Introducing `self`


```python
class Dog():
    
    def bark(self):
        return "I am actually going to bark this time. bark!"
        
    def who_am_i(self):
        return self
        
newest_rex = Dog()
newest_rex.bark()
```




    'I am actually going to bark this time. bark!'



Awesome! It works. Again, since instance methods implicitly pass in the object itself as an argument during execution, you need to define our method with at least 1 parameter. 

Notice that the parameter is named `self`. As with any function or method, you can name the parameters however you want, but the convention in Python is to name this first parameter `self`, which makes sense since it is the object on which you are calling the method. If that sounds confusing, don't worry. The concept of `self` is a little confusing. Let's play around with it and see if you can get a better idea.

Let's first see what the return of the who_am_i method is:


```python
fido = Dog()
print("1.", fido.who_am_i()) # checking return value of method
print("2.", fido) # comparing return of the fido instance object 
```

    1. <__main__.Dog object at 0x108737cf8>
    2. <__main__.Dog object at 0x108737cf8>


As you can see our who_am_i method is returning the same instance object as fido, which makes sense because you called this method **on** fido, and if you look at the method all it does is return the first argument (`self`), which you said is the instance object on which the method was called. 


```python
print("3.", fido == fido.who_am_i()) 
print("4.", newest_rex == newest_rex.who_am_i()) 
# again asserting that `self` is equal to the instance object on which who_am_i was called
```

    3. True
    4. True


Again, don't worry if `self` still seems a bit confusing. It will become clearer through practice. For now, you can just go forward with the knowledge that **to define an instance method and later call it on an instance object, you will need to include at least one parameter in the method definition**.

## Summary

In this lab, you were introduced to a lot. You looked at instance methods and added functions as attributes of objects. Then you looked at the differences between functions and instance methods. You learned that instance methods are bound to objects and they always use the object on which they are called as their first argument. Since instance methods use their object as an argument you looked at how to properly define an instance method by introducing the concept of `self` in object-oriented programming.
