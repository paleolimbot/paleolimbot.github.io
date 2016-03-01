---
title: Python 2
layout: page
---

There are a few more things we need to know how to do before we start to do some of the more useful things Python has to offer, we need to know about a few more things about how Python works. First up: functions.

## Functions

So far we've just been using the built-in Python operators, but there's also functions that are built-in (and we can import others as you'll see later). Functions take **arguments** and compute a **return** value. Let's use the `len()` function to find out how may elements are in our `list` objects.


```python
mylist = ["first element", "second element", "third element"]
len(mylist)
```




    3



The `len()` function also works with `str`, `tuple`, and `dict` objects.


```python
mystring = "my nice long string"
mytuple = ("one", "two", "three", "four", "five")
mydictionary = {'key1': 'value1', 'key2':'value2'}
len(mystring)
```




    19




```python
len(mytuple)
```




    5




```python
len(mydictionary)
```




    2



Here, we're calling the `len()` function with an **argument** as the thing we want to find the length of. In this last case, it was the **mydictionary** variable. Most functions take more than one **argument**, like the `max()` function, which **returns** the maximum of all of its arguments.


```python
max(3, 1, 7, 10, 4)
```




    10



Some functions also take **keyword** arguments, or arguments that we name specifically. Let's use the `sorted()` function as an example, which returns a sorted version of its argument, which can be a list or a tuple.


```python
sorted([8, 2, 7, 5, 10, 100, -8])
```




    [-8, 2, 5, 7, 8, 10, 100]



If we want to sort the list in reverse, however, we need to pass the `reverse` keyword argument as `True`.


```python
sorted([8, 2, 7, 5, 10, 100, -8], reverse=True)
```




    [100, 10, 8, 7, 5, 2, -8]



You can also use the `int()`, `str()`, `float()`, `bool()`, and `list()` built-in function to convert between types.


```python
int("145")
```




    145




```python
str(5.678)
```




    '5.678'




```python
float("3.141596535")
```




    3.141596535




```python
bool('True')
```




    True




```python
mytuple = (1, 6, 7, "fourth element")
list(mytuple)
```




    [1, 6, 7, 'fourth element']



Of course, if an object can't be converted, it will give you an error:


```python
float("three point one four")
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-13-3b58f97747f8> in <module>()
    ----> 1 float("three point one four")
    

    ValueError: could not convert string to float: three point one four


## Objects and Methods

I've been throwing around the term **object** for a while now, and it's time I was a little more explicit about what that means. **Objects** in Python all have a **type**, and different **types** make various **methods** accessible to you as a Python programmer. You can think of a **method** as a **function** attached to an **object**. Let's use a `str` object as an example.


```python
mystring = "My Nice Long String"
mystring.upper()
```

Here we assigned the **object** `"My Nice Long String"` (of type `str`) the name `mystring`, and called the `upper()` method of that **object**. The type `str` has many such methods, and here I'll demonstrate a few.


```python
mystring.lower()
```


```python
mystring.startswith("My")
```


```python
mystring.endswith("My")
```


```python
mystring.replace("My", "Your")
```

If you're ever wondering what methods you're allowed to call on an object, you can check the online Python documentation for a list. You can also use the `Tab` autocomplete feature available in Jupyter (and most Python editors) by typing `mystring.` and then hitting the `Tab` key.

It's also worth going over a few **methods** of the `list` type, becase lists are commonly used in Python. The most useful is the `append()` **method**.


```python
mylist = []
mylist.append("my first element")
mylist.append("my second element")
mylist
```

Also useful is the `insert()` method (which will insert an item to the list before the specified index) and the `clear()` method (which empties the list).


```python
mylist.insert(1, "my first and a half-th element")
mylist
```


```python
mylist.clear()
mylist
```

You'll notice here that the `list` methods we've been using actually modify the object, which isn't something we can do with `str` objects or `tuple` objects (the methods we called on our `str` object returned a new `str` object instead of modifying the current object in place). Dictionaries (objects of type `dict`) also have methods that modify the original object (in addition to us modifying the object by assigning key/value pairs and removing items using the `del` keyword).


```python
mydictionary = {}
mydictionary['key1'] = 'value 1'
mydictionary['key2'] = 'value 2'
mydictionary['key3'] = 'value 3'
mydictionary
```


```python
mydictionary.clear()
mydictionary
```

There are also handy methods of the `dict` type that let us retreive, the keys and values of our `dict` object.


```python
mydictionary['key1'] = 'value 1'
mydictionary['key2'] = 'value 2'
mydictionary['key3'] = 'value 3'
mydictionary.keys()
```


```python
mydictionary.values()
```

You'll notice that the output looks a little funny because our keys and values are wrapped around what appears to be a `dict_values` function. This is telling us that the `values()` method isn't returning a list, it's returning something a lot like a list with a type `dict_values` (or `dict_keys`) type. This distinction is rarely a concern in Python, but if you ever need a list from something list-like, you can use the `list()` function to convert it to a `list`.


```python
mykeys = list(mydictionary.keys())
mykeys
```

## Notes on objects and variables

It's worth taking a moment here to discuss one of the finer points of objects and variables in Python. I've so far said that when we assign a variable using the equals sign (`mylist = [1, 3, 4]`), but what Python is actually doing is binding the name `mylist` to the object `[1, 3, 4]`. Consider the following:


```python
mylist = [1, 3, 4]
myotherlist = mylist
mylist.append(5)
mylist
```


```python
myotherlist
```

What happened here is that we bound the name `mylist` to the object `[1, 3, 4]` (of type `list`), then bound the name `myotherlist` to the object referenced by `mylist`, and then modified that object (appended `5` as an element of the list). When we examine the value of `myotherlist`, we find that it is the same as `mylist`, because **both names refer to the same object**. If you want to copy the data and create a new `list`, you have to explicity do so.


```python
mylist = [1, 3, 4]
myotherlist = list(mylist)
mylist.append(5)
mylist
```


```python
myotherlist
```

This starts to get incredibly complicated when you have lists inside of lists (because elements of lists are also just storing references to objects, so copying a list using `list()` will not copy the lists referred to by the first list). The module `deepcopy` is provided to help with this, but we're not quite at the point where we can import modules. First, we need to deal with some basic **flow structures**.
