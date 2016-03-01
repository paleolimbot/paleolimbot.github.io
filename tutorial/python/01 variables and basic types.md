---
title: Python 1
layout: page
---

## Use Python like a calculator

The easiest way to use the Python `>>>` prompt is like a calculator:


```python
4+1*3+2
```




    9




```python
6.2 / 4.0
```




    1.55




```python
2**6
```




    64




```python
3 * (2 + 1)
```




    9



You'll notice that Python respects the usual order of operations. Note the `**` operator for denoting exponents.

## Variables

We can also assign the value of one of these expressions to a variable so we can refer to it later. The syntax for this is as follows:


```python
mynumber = 5
```

We can also type any expression to the right of the equals sign, and the result will be assigned to the variable named on the left.


```python
mynumber = 2 + 3
```

Now that we've defined the variable `mynumber`, we can use it in any other expression.


```python
myothernumber = mynumber * 2
myothernumber
```




    10



Variables don't have to contain just numbers, in fact, they can refer to an object of any type. So far we've just used the `int` type, but we can just as easily assign a `str` (string), `float` (number with a decimal point) or any other type (we'll learn about some other types in the next section).


```python
mystring = "this is my first string" # a string, type 'str'
myint = 5 # an integer, type 'int'
myfloat = 3.9 # a floating point number, type 'float'
```

To delete a variable, we use the special `del` keyword. This means we can no longer refer to the variable in our code.


```python
mystring
```




    'this is my first string'




```python
del mystring
mystring
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-10-9ec8d5b540fd> in <module>()
          1 del mystring
    ----> 2 mystring
    

    NameError: name 'mystring' is not defined


## Lists

Storing integers, floats, and strings to variables is cool, but chances are you had more lofty ambitions. The real power of programming languages is the ability to operate on large amounts of things, and to keep track of all of those things we're going to need the Python `list` type. We'll create `list`s using square brackets like this:


```python
mylist = []
```

This list will be empty, but we can also create a list with elements already in it:


```python
mylist = ["first item", "second item", "third item"]
```

In this case I happened to make all of my items be strings, but we could just as easily make them `int`s, `float`s, or a mix of types.


```python
myotherlist = [1, 3.1415926535, "a string"]
```

To retreive elements from a list, use the square brackets after the variable name, using indicies starting at 0.


```python
mylist = ["first item", "second item", "third item"]
mylist[0]
```


```python
mylist[1]
```


```python
mylist[2]
```

If we try to access an element in the list that we haven't assigned, Python will give us a nasty sounding error:


```python
mylist[4]
```

We can also use negative numbers as indicies to access the end of a list:


```python
mylist[-1]
```


```python
mylist[-2]
```

And we can use the `:` between two indicies to get a subset of the list, which Python calls a **slice**.


```python
mylist[1:3]
```


```python
mylist[0:-1]
```

Because the square brackets are used for both indexing and creating `list`s, if we want to create a `list` of only one element, we have to insert a comma before the end bracket to make sure Python knows what we mean.


```python
singleelementlist = ["first element",]
```

We can use each element in a `list` just as we would a variable.


```python
mylist = [2, 8, 9, 13, 15]
mylist[1] * 2
```


```python
mylist[-1] / 5
```

Similarly, we can assign and delete list elements just like a variable.


```python
mylist[0] = 565
mylist[-1] = 1001
mylist
```


```python
del mylist[0]
mylist
```

But if you try to assign an index that doesn't exist, you will get an error:


```python
mylist[5] = 1002
```

To do that, you'll have to call `mylist.append(1001)`, but hold on to that thought for a second. First, we have to cover a few more **types**, and then we'll get to **functions**.

## Dictionaries

Another useful type in Python is the `dict` type, which lets us store key/value pairs. Instead of a number as an index we can use any kind of key we want, which we usually make a string since that's human readable. We create a dict using curly brackets like so:


```python
mydictionary = {}
```

And just like `list` objects we can assign elements, but instead of an integer as an index, we're going to call it a **key** and assign it a **value**.


```python
mydictionary["mykey"] = "my value"
mydictionary["key2"] = "my other value"
mydictionary["anumber"] = 4
```

Then we retreive those values in the same way we would from a list, using the square brackets:


```python
mydictionary["mykey"]
```


```python
mydictionary["key2"]
```


```python
mydictionary["anumber"]
```

If we just evaluate the dictionary at the prompt, we'll get a string in curly brackets with our key/value pairs. It's possible to create a dictionary in this way, much like how we created a `list` object with elements already in it.


```python
mydictionary
```

Just like variables and lists, we can delete elements using the `del` keyword:


```python
del mydictionary["key2"]
mydictionary
```

## Types

So far we've hinted at the fact that each object in Python has a `type`. Each `type` has a slightly different behaviour in Python when we use common operators such as `+`, `-`, `*`, `/`, `**`, and the indexing behaviour we just talked about. We'll also throw in the modulus (`%`) operator because it's often useful when dealing with strings.

### Numbers

Numbers behave exactly as you'd expect in Python, with the `+` meaning add, the `-` meaning subtract, the `*` meaning multiply and the `**` meaning "raise to the power of". The `/` operator means division for `float` types (numbers with a decimal), but when dividing two `int` objects in Python 2 you will get something called 'floor division'. In Python 3, you have to use the `//` operator to get floor division, the difference being as follows:


```python
10 / 3
```


```python
10 // 3
```

Why, you ask? It's often quite useful to do this when programming, especially in combination with the modulus operator, which gives the remainder.


```python
10 % 3
```

If you ever want to make sure that you're getting true division, the easiest way is to make sure Python knows both numbers are `float`s by putting a decimal point in them.


```python
10.0 / 3.0
```

### Strings

We breifly touched on strings earlier (type `str`), but it's worth going over the finer points of using them. To create them, you can enclose your string in single (`'`) or double (`"`) quotes, but if you use the same type of quote inside your string, you have to *escape* it using a backslash (`\`).


```python
mystring = "my string with 'quotes' inside" # works
mystring = "my string with \"quotes\" inside" # also works
```

We can also use the indexing behaviour that we used with lists to access letters within the string:


```python
mystring[0]
```


```python
mystring[-1]
```


```python
mystring[4:10]
```

We can use the `+` operator to concatenate strings together:


```python
"first string" + "second string"
```

The `*` operator will repeat the string the number of times you specify:


```python
"me" * 5
```

And just like number operators you can string expressions together like this:


```python
"this little piggy went " + "wee" * 5 + " all the way home"
```

The modulus (`%`) operator can also be used in strings to format objects inside of a string, which is useful when typing out error messages. The exact syntax can get complicated, but the general idea is that you put a `%s` somewhere in your string, and use the modulus operator to provide an object to format.


```python
"there were %s little piggys" % 5
```

### Lists Revisited

We already know that `list`s can be indexed, but the `+` and `*` operators that we used for strings also work with lists in a similar way: the `+` operator concatenates (joins) the two `list`s together, and the `*` operator repeats them.


```python
mylist = ["one", "two", "three"]
mylist + ["four", "five"]
```

Note that if you try to append a string (type `str`) it won't work because Python doesn't know what you mean by adding a string to the end of a list. Instead, you have to add a single-element list (with a comma before the closing bracket, like we learned earlier).


```python
mylist + ["four",]
```

The `*` operator repeats lists, like this:


```python
mylist * 2
```

### Tuples

You'll also sometimes see lists of things enclosed in round brackets (`()`). This type is caled a `tuple`, and it's exactly like a list except you can't change it once you create it. There's good reasons to do this, often two values should be treated as one object (like x and y coordinates, for example). We create a `tuple` just like a `list`, but with round brackets. Indexing works just like a `list`.


```python
mytuple = ("first element", "second element", "third element")
```


```python
mytuple[0]
```


```python
mytuple[0:2]
```


```python
mytuple[-1]
```

The `+` operator and `*` operator also work identically, except they return a `tuple` instead of a `list`.


```python
mytuple = ("one", "two", "three")
mytuple + ("four", "five")
```


```python
mytuple + ("four",)
```


```python
mytuple * 2
```

You'll often have to use a `tuple` to pass multiple values around in Python, like if you want to format a string using the `%` operator and you have more than one missing value.


```python
"%s turtle doves, 3 %s hens" % (2, "french")
```

## Booleans

Booleans (type `bool`) are either `True` or `False`, and we'll look a little more closely at them when we get to *if* and *else* statements in the next section.

## The None Type

There is also a special type called the `None` type, which is used to indicate that there is no value for a particular expression. This is more important when we get to functions later on, but for now take my word for it that it can be useful to assign `None` to a variable whose value isn't yet defined.
