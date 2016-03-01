---
title: Python 3
layout: page
---


We've gone over most of the **functions**, **objects** and **methods** that are common in Python, but to combine them in powerful ways we're going to need to be able to dynamically manipulate our data. In all programming languages there are **if** statements, **for loops** and **while loops**, all of which you will use as you start to program in Python. The simplest of these is the **if** statement.

## If statements

The basic idea of an **if** statement is to test a **condition**, then perform code if that condition is `True`. Consider the following:


```python
a = 5
if a > 3:
    print("A is greater than 3")
```

    A is greater than 3


Here we have code that we want to execute if `a` is greater than `3`. The generic syntax of this is:

```Python
if condition:
    # do something here
    ...
else:
    # do something else
    ...
```

Note the colon (`:`) followed by the next line indented. This is how Python knows the code you want to execute inside the **if** statement is, and it's important that you pay attention to your indentation. Python prefers 4 spaces per indent, and good editors will happily do this for you. The **condition** here is something that Python can evaluate to be `True` or `False`, which is a fairly liberal definition in Python (e.g. empty lists or zero-length strings will evaluate to `False` in this case.

We'll go over how to construct these conditions in a second but first let's add this `else` to our original if statement:


```python
if a < 3:
    print("A is greater than 3")
else:
    print("A is not greater than 3")
```

    A is not greater than 3


We can also add a second **condition** using the `elif` keyword:


```python
a = "two"
if a == "three":
    print("A equals three")
elif a == "two":
    print("A equals two")
else:
    print("A does not equal three or two")
```

    A equals two


You may have noticed we've beeing using some new operators to construct these **conditions** evaluated in the **if** statements, here's a summary:

| Operator       | Meaning                                            | Example (result)                                               |
|----------------|----------------------------------------------------|----------------------------------------------------------------|
| ==             | Equals                                             | 4 == 4 (`True`)                                                |
| !=             | Not Equals                                         | 4 != 5 (`True`)                                                |
| >, >=          | Greater than, greater than or equal to             | 4 > 3 (`True`); 4 >= 4 (`True`)                                |
| <, <=          | Less than, less than or equal to                   | 4 < 5 (`True`); 4 <= 4 (`True`)                                |
| `in`, `not in` | Membership (usually for `list` and `dict` objects) | `5 in [4, 5, 6]`; `'five' in ['four', 'five', 'six']` (`True`) |
| `and`, `or`    | Combine boolean operations                         | `4 != 5 and 3 < 6` (`True`); `4 == 3 or 5 > 2` (`True`)        |
| `not`          | The opposite                                       | `not False` (`True`); `not (5 in [4, 5, 6])` (`False`)           |

These operators work for all types, although the meaning is slightly different for each type. For the most part, they do what you would expect, like in this example comparing string operators:


```python
a = "three"
if a == "three":
    print("A equals three")
else:
    print("A does not equal three")
```

    A equals three


## Loops

The real power of programming is that it repeat things a lot of times, and to do that we need a structure called a **loop**. Consider the following:


```python
mylist = ["item1", "item2", "item3", "item4"]
for item in mylist:
    print(item)
```

    item1
    item2
    item3
    item4


Here we define `mylist`, and use a **for loop** to **iterate** through each item of the list. The syntax here is as follows:

```Python
for item in iteratable:
    # do some operation on 'item'
    ...
```

We have an **iteratable** here, which can be a `list`, `tuple`, `dictionary`, `file` or a number of other data types. Here each value returned by the **iteratable** is bound to the name `item`, and within the loop we can use the value of `item` any way we choose (in the last example I just printed it, but there are many options for how to do this). Note the use of the colon (`:`) for the indented block again, so Python knows which code we want looped over. We can similarly loop through `dict` objects:


```python
mydict = {'key1': 'item1', 'key2': 'item2', 'key3': 'item3'}
for key, value in mydict.items():
    print(key, value)
```

    key3 item3
    key2 item2
    key1 item1


Note that here the **iteratable** returned two values. If we just passed `mydict` to the for loop we would have to do this:


```python
for key in mydict:
    print(key, mydict[key])
```

    key3 item3
    key2 item2
    key1 item1


If we want to loop through just the value of the dictionary and don't care about the keys, we can do this (note that the values aren't returned in any particular order, and certainly not the order you added them in):


```python
for value in mydict.values():
    print(value)
```

    item3
    item2
    item1


Another common operation is to iterate through a list of numbers, which we can do using the `range()` function. The `range()` function will produce a list of numbers (more technically it is an iterator, in case anybody asks) based on our input, which is usually just the end of the sequence:


```python
for i in range(4):
    print(i)
```

    0
    1
    2
    3


Note that the start is 0 and the end is the number we pass in minus 1. This is because `range()` is usually used to produce valid indicies of a `list`, so we can do the following:


```python
for i in range(len(mylist)):
    print(i, mylist[i])
```

    0 item1
    1 item2
    2 item3
    3 item4


If we pass in two numbers to `range()`, it will generate a list between the two numbers (again, with the final number less one).


```python
for i in range(3, 7):
    print(i)
```

    3
    4
    5
    6


If we pass in three numbers, the third number represents the step size.


```python
for i in range(3, 12, 2):
    print(i)
```

    3
    5
    7
    9
    11


And to produce any of these backwards (this works for any iteratable), we can wrap it in `reversed()`.


```python
for i in reversed(range(4)):
    print(i)
```

    3
    2
    1
    0


## Other Loops

It's worth also mentioning there's another type of loop called a **while loop**, which operates in a slightly different way. If we were to use it a little like a **for** loop, it would look like this:


```python
count = 0
while count < 5:
    print(count)
    count += 1
```

    0
    1
    2
    3
    4


The generic structure for a **while** loop is as follows:

```Python
while condition:
    # do something a bunch of times
    ...
```

Here **condition** is the same kind of condition we used in an **if** statement (i.e. something that evaluates to `True` or `False`). The loop will happily repeat the code in the middle until **condition** evaluates to `False`, meaning if you accidentally write the loop with a condition that will always be `True` you're going to have a program that never ends! To avoid this, you should probably stick to using **for** loops unless you really need to use a **while** loop. Just as an example, here is a case where **while** loops are useful:


```python
mystring = "I   find  extra   spaces      quite obnoxious"
while "  " in mystring:
    mystring = mystring.replace("  ", " ")
mystring
```




    'I find extra spaces quite obnoxious'



Here I'm removing multiple spaces from the `mystring` object until there are no longer any double spaces in the string (i.e. `"  " in mystring` evaluates to `False`). It's impossible to do this with a **for** loop, since I'm not really iterating through anything, I'm just performing an operation until I'm satisfied with the results.

## Comprehensions

As another quick note, it's worth mentioning the **list comprehnsion** since it's incredibly useful. Often it's desire to create a new list using the objects from other list as input. Consider the following:


```python
mylist = ["item1", "item2", "item3", "item4"]
mynewlist = []
for item in mylist:
    mynewlist.append(item + "...but in a new list")
mynewlist
```




    ['item1...but in a new list',
     'item2...but in a new list',
     'item3...but in a new list',
     'item4...but in a new list']



Here we are creating a new list from the items in `mylist` just adding some text on the end of each item. To do this in a **comprehension** the syntax would be like this:


```python
mynewlist = [item + "...but in a new list" for item in mylist]
mynewlist
```




    ['item1...but in a new list',
     'item2...but in a new list',
     'item3...but in a new list',
     'item4...but in a new list']



You can think of a comprehension like a shorthand for loop, and while it seems a little difficult at first, simplifying these expressions into a single line of code produces cleaner code and usually runs slightly faster than the non-comprehension equivalent. It's also possible to append a condition to the end of a comprehension like this:


```python
mynewlist = [item + "...but in a new list" for item in mylist if item != "item2"]
mynewlist
```




    ['item1...but in a new list',
     'item3...but in a new list',
     'item4...but in a new list']



## Try/Catch blocks

This bit is a little advanced but it's wroth knowing what these are since sometimes you'll see it in others code and occasionally you will want to evaluate code that may fail without rigorously checking the input. If you've typed something wrong so far in the tutorial you may have gotten an error like this:


```python
value = float('not a number')
value
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-30-c0350b3a1566> in <module>()
    ----> 1 float('not a number')
    

    ValueError: could not convert string to float: 'not a number'


Here it's pretty obvious we tried to do something that Python is not going to let us do, but imagine you were reading in 10,000 numbers from a file...it might be that you just want to ignore these values instead of stopping the entire operation for one failure. The syntax for this is as follows:


```python
try:
    value = float('not a number')
except ValueError:
    value = 0.0
    print("Encountered an error parsing the value!")
value
```

    Encountered an error parsing the value!





    0.0




```python
try:
    value = float('5.4')
except ValueError:
    value = 0.0
    print("Encountered an error parsing the value!")
value
```




    5.4



The idea here is that instead of checking if `'not a number'` could possibly be not a number, we just tried the operation anyway and setup a block that will be evaluated if this fails. In general when writing code, it's good practice to specify the type of error after `except`, in this case `ValueError`. You'll also see `IndexError` and `AttributeError` among many others. In a more real-world example, here's how we might use a a try/catch statement inside a loop to read in a bunch of values.


```python
myinput = ['4.6', '4', '6.2', 'a']
myparsedinput = []
for item in myinput:
    try:
        myparsedinput.append(float(item))
    except ValueError:
        myparsedinput.append(None)
myparsedinput
```




    [4.6, 4.0, 6.2, None]



You can imagine that when reading in a large number of values, it's worth doing this to catch the bad ones. Note that if we put the try/catch block outside of the for loop, the for loop would exit when it tried to parse the non-string.

Now that we've learned the flow controls in Python, it's time to move on to reading files.
