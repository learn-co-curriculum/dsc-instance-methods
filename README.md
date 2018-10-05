
# Instance Methods

## Introduction
Now that we know what classes and instances are, we can start to talk about instance methods. Instance methods are almost the same as regular functions in python. The key difference is that an instance method is defined inside of a class and bound to intance objects of that class. Instance methods can be thought of an attribute of an instance object. The difference bethween an instance method and another attribute of an instance, is that instance methods are `callable`, meaning they execute a block of code. This may seem a bit confusing, but try to think about instance methods as functions defined in a class that are really just attributes of an instance object from that class.

## Objectives

You will be able to:

* Understand instance methods as attributes
* Define an instance method
* Introduce self

## Instance Methods as Attributes

When we think of methods and functions, we think about what kind of action they perform. The same goes for instance methods, however, the action being performed is scoped directly to that instance object. Remember, classes are kind of like the blue prints for its instance objects. So, let's take the example of a **Dog** class. What are things that all dogs do? They can bark, beg to go for a walk, chase squirrels, etc. So, we consider these more or less attributes of a dog -- the same as their name, breed, weight, age etc. When we create a new dog instance object, the dog should be able to automatically bark, beg, and chase squirrels.

Let's see how we would create a single dog, `rex`, and get him to bark:


```python
class Dog():
    pass 
rex = Dog()
rex
```


```python
rex.bark()
```

Okay, here we have an instance of the dog class, but as we can see rex cannot bark yet... Let's see if we can fix that. We said instance methods are basically functions that are ***callable*** attributes, like functions, of an instance object. So, let's write a function that returns the string `"bark!"`, and assign it as an attribute of a dog.

> **Note:** Dictionary object attributes are accessed using the bracket (`[]`) notation. However, instance object attributes are accessed using the dot (`.`) notation. 


```python
def make_a_bark():
    return "ruff ruff!"

rex.bark = make_a_bark
rex.bark
```

Here we can see that we successfully added a the `bark` attribute to `rex` and assigned it the function `make_a_bark`. Note that the return value of `rex.bark` is simply a function signature since we have not yet executed the function, and although this looks like an instance method it is not quite.

> **Note:** Although you may hear and see the terms method and function used interchangeably, there are slight differences. We know that function is essentially an object that contains a block of code and it can optionally take in data or objects as explicit parameters, operate on them, and optionally return a value. A method is, simply put, a function that is bound to a class or instances of that class. Instance methods, thus, are functions that are available/bound to instance objects of the class in which they are defined. However, a key difference between the two is that a method is *implicitly* passed the object on which it is called, meaning the first parameter for the method is the object. Don't worry if this is confusing as we will dive more into this later.


```python
rex.bark()
```

Great, now we see that once we **called** the `bark` attribute, we executed the function and now see our return value, `"ruff ruff!"`. 

This is a great first step. However, since `make_a_bark` is not actually defined inside our class, we are able to call it without our dog instance object, `rex`, and as we just covered, that's not really how instance methods work... 


```python
make_a_bark()
```

Alright, so, how do we turn this into a real instance method? Well, the first thing we need to do is define it inside of our class. So, let's take a look at how we can do that below:

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

Here, we have re-defined our Dog class, but this time we actually defined an *instance method* `bark`. Now, whenever we create a new instance of this new Dog class, that instance will have the bark instance method as an attribute. 

Notice that the signature that is returned by the unexecuted method says **bound `method`** instead of function, as was the case with our first `rex`'s bark. 

There is **one** issue with our instance method. Let's try calling it and see what happens:


```python
new_rex = Dog()
new_rex.bark()
```

Uh oh! `TypeError: bark() takes 0 positional arguments but 1 was given`. This error is telling us that the method, `bark` was defined to take 0 arguments, but when we executed it, we gave it an argument. 

Remember when we said one of the key differences between functions and methods is that a method is bound to an object and **implicitly** passes the object as a argumet? Well, that is what is causing our error. Effectively, what is happening when we try to call the instance method is this:

```python
# the instance object, new_rex, is implicitly passed in as the first argument upon execution
new_rex.bark(new_rex)
```

So, how do we fix this error? Well, if instance methods will always require a default argument of the instance object, we will need to define our instance methods with an *explicit* first parameter.

>**Note:** Parameters are the variable names we give to our method or function's future data. They are called parameters when we talk about the defining of a method or function, but once we have the data they are arguments. 

```python
def function_example(parameter1, parameter2):
    return parameter1 + parameter2
# since we are defining the function, the variables, parameter1 and parameter2, are called parameters

function_example("Argument1", "Argument2") 
# here the strings passed in, "Argument1" and "Argument2", are arguments since we are executing the function
```

Okay, so let's see if when we define our function with a parameter, we can get get rid of the error. We'll also define another function `who_am_i` to help further understand what's happening here.

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

Awesome! It works. Again, since instance methods implicitly pass in the object itself as an argument during execution, we need to define our method with at least 1 parameter. 

Notice that the parameter is named `self`. As with any function or method, we can name the parameters however we want, but the convention in Python is to name this first parameter `self`, which makes sense since it is the object on which we are calling the method. If that sounds confusing, don't worry. The concept of self is a little confusing. Let's play around with it and see if we can get a better idea.

Let's first see what the return of the who_am_i method is:


```python
fido = Dog()
print("1.", fido.who_am_i()) # checking return value of method
print("2.", fido) # comparing return of the fido instance object 
```

As we can see our who_am_i method is returning the same instance object as fido, which makes sense because we called this method **on** fido, and if we look at the method all it does is return the first argument (`self`), which we said is the instance object on which the method was called. 


```python
print("3.", fido == fido.who_am_i()) 
print("4.", newest_rex == newest_rex.who_am_i()) 
# again asserting that `self` is equal to the instance object on which who_am_i was called
```

Again, don't worry if `self` still seems a bit confusing. It will become clearer through practice and it will also be very useful as we get further into object oriented programming. For now, we can just go forward with the knowledge that **to define an instance method and later call it on an instance object, we will need to include at least one parameter in the method definition**.

## Summary

In this lab, we introduced a lot. We looked at instance methods and adding functions as attributes of objects. Then we looked at the differences between functions and instance methods. We learned that instance methods are bound to objects and they always use the object on which they are called as their first argument. Since instance methods use their object as an argument we looked at how to properly define an instance method by introducting the conept of `self` in object oriented programming. 
