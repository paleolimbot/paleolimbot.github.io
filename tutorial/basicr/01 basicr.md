---
title: Basic R Tutorial
layout: page
---

Hopefully you've gotten through the process of installing R and RStudio, but now you're presented with the ominous `>` of the R console. By the end of this tutorial, hopefully that `>` should fill you with a feeling of hope and opportunity, for magical things can happen when you type the right thing after the `>`.

## Expressions and Variables

Let's just do the basics: try typing in something like this at the prompt.

```R
> 1+1
[1] 2
> 2*5
[1] 10
> 5^2
[1] 25
> 2*(5+1)
[1] 12
```

As you can see, R works just like a calculator and evaluates all of these **expressions** just like you would expect. If we would like to save the result of one of these expressions we can assign that value to a **variable** like this:

```R
> x <- 1+1
```

Then, to view the value of `x` we can just type `x` at the console, and R will show us the value.

```R
> x
[1] 2
```

The `<-` means "assign the value on the right to the variable on the left". We can also use `x` in any expression and R will subsitute its value in like this:

```R
> x + 2
[1] 4
```

An **expression** is something that R can evaluate to produce a value, like `2+2` or `x + 2`. Any time you type this in the console without assigning it to a **variable**, R will print out the value. In fact, any time you type anything into the R console, R is evaluating that expression, which may or may not return a value. If there is no value returned, R won't print anything when you press enter.

So far we've just used numbers, but often we need to enter text into R. Whenever we do this, we surround the text in quotes, like this:

```R
mytext <- "I am text"
```

Text (called **strings**) are one of many data types available in R, but we'll learn about those after we talk about **functions**.

## Functions

A function is some kind of operation that takes one or more **arguments** (input values) and produces a **return value** (output value). The `sqrt` function is a good examle:

```R
> sqrt(4)
[1] 2
```

Here, `4` is an **argument**, and the function **returns** the square root of that, which is `2`. Functions can take more arguments, like the `max` function:

```R
> max(2, 6, 7, 2, 10)
[1] 10
```

Here we are giving the `max` function 5 arguments, of which it returns the maximum. One other way we specify arguments is by **keyword arguments**, like in the `paste` function.

```R
> paste("string1", "string2", sep="_")
[1] "string1_string2"
```

The `paste` function takes its arguments and combines them using the `sep` that we specify, in this case `"_"`. The function **returns** this string. Many functions in R have many many arguments and usually we only want to modify one or two of interest to us. The format is always `key=value`, where `key` is the name of the keyword and `value` is some **expression** we would like to use as the value for that argument.

R contains thousands of functions that do most of what you could possibly imagine with regards to data and statistics, but remembering which one you want and how to use it can be difficult. Luckily, RStudio makes it easy using tab autocompletion and easy access to help files. To autocomplete, start typing the name of the function and press the `[Tab]` key (or `Ctrl+Space`), and RStudio will helpfully provide you with suggestions.

To access the documentation for a particular function, you can type `?` in front of the function name and press return, and RStudio will open the helpfile for you if it exists.

```R
> ?paste
```

Will bring up the following:


>#### Description

>Concatenate vectors after converting to character.

>#### Usage
>
>```
paste (..., sep = " ", collapse = NULL)
paste0(..., collapse = NULL)
```
>
>#### Arguments
><table><tr valign="top"><td><code>...</code></td><td><p>one or more <span style="font-family: Courier New, Courier; color: #666666;"><b>R</b></span> objects, to be converted to character vectors.</p></td></tr><tr valign="top"><td><code>sep</code></td><td><p>a character string to separate the terms.  Not
<code><a href="NA.html">NA_character_</a></code>.</p></td></tr><tr valign="top"><td><code>collapse</code></td><td><p>an optional character string to separate the results.  Not
<code><a href="NA.html">NA_character_</a></code>.</p></td></tr></table>

Note that each of these **arguments** can be specified by **keyword**, and have default values that we can see in the **usage** section. The `...` means that we can pass any number of arguments to the function. There's too many functions in R to keep in your head at one time, so getting good at reading thes help files is very useful!

## Vectors

So far we've just been dealing with single values like `2+2` or `"mystring"`, but the real power of R is that it can operate easily on large lists of data, so it makes sense that it provides us with an easy way to work with this data. These lists of data are called **vectors**, and we create them using the `c` function (`c` stands for **concatenate**, or join together).

```R
> myvector <- c(10, 9, 8, 7, 2)
> myvector
[1] 10  9  8  7  2
```

Here the `c` function took our **arguments** of 10, 9, 8, 7, and 2, and **returned** a **vector**, which we assigned to the variable named `myvector`. When we evaluated `myvector`, it printed out the list of values it contained.

Vectors don't just have to contain numbers, they can contain strings as well.

```R
> mytextvector <- c("word1", "word2", "word3")
> mytextvector
[1] "word1" "word2" "word3"
```

 Here the `c` function took our **arguments** and **returned** a **vector** of strings, which we assigned to the variable `mytextvector`.

It is common to have to generate a vector of all the integer values between two numbers, so R provides a short form for this:

```R
> 1:10
[1]  1  2  3  4  5  6  7  8  9 10
> 20:12
[1] 20 19 18 17 16 15 14 13 12
> myothervector <- 13:16
> myothervector
[1] 13 14 15 16
```

You can also use an **expression** on either side of the `:`, like this:

```R
> (5^2):(3*10)
[1] 25 26 27 28 29 30
> start <- 25
> end <- start + 5
> start:end
[1] 25 26 27 28 29 30
```

## Indexing

Now we've created vectors, but to get at what's inside them we need to retreive values using an **index**. We do this using square brackets like this:

```R
> myvector <- c(10, 9, 8, 7, 2)
> myvector[1]
[1] 10
> myvector[5]
[1] 2
> myvector[3]
[1] 8
```

This code creates a vector, assigns it to the variable `myvector`, then retreives the 1st, 5th, and 3rd value stored in that vector. If we would like multiple values from the vector, we can pass multiple values as indicies, like this:

```R
> myvector[c(1, 5, 3)]
[1] 10  2  8
```

You'll notice that the index that we're using is actually a vector itself! I know, we're using a vector to index a vector and it's a little trippy, but it's incredibly useful. You'll remember that we can easily create vectors of sequential integers, which we can use to get a sequence of values from a vector by using it as an index.

```R
> myvector[1:3]
[1] 10  9  8
```

This would be equivalent to:

```R
> myvector[c(1, 2, 3)]
[1] 10  9  8
```

## Summary

In this lesson we covered **expressions**, **variables**, **functions**, **vectors**, and **indexing**, all of which you're going to need to know to move on. There are many R tutorials available online, so if you feel like you may not know what some of these words mean it's worth looking them up before moving on.

Ready? Let's move on to Part 2.