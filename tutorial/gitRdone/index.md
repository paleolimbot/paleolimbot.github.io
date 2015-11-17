---
title: git-R-done
layout: page
---

# git-R-done: Effective project management in RStudio

[RStudio](https://www.rstudio.com/) makes it easy to write and execute R code in all kinds of ways: we can run commands on the console, we can run lines from our `.R` files, we can "source" (or run the entirety of) our `.R` files, we can import packages, create packages, analyze data, create data, and I'm sure I've only scratched the surface. We usually write our R scripts for the present...to produce the chart or map or table we need for our thesis or paper or boss. There's plenty of theories and even some scholarly articles regarding [best practices for scientific computing](http://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.1001745). You probably don't have time to read all of them (I certainly didn't), but it's fairly intuitive that **writing code that's easy to understand and re-use in the future is a good idea**. There's more than a few ways to go about doing this, but for the purposes of this tutorial we're going to say the three things you should do to make things easy on yourself later are **avoid copy and paste**, **structure your files**, and **use git to backup and share your code**.

## Avoid copy & paste (or *modularizing* your code)
I undestand better than anybody that when you sit down to write code the first thing you have to do is *just make it work*. While you're doing that, copy and paste all you want! Until you have something that works, there's nothing worth using in the future anyway. As an example we'll take some secchi depth data from a two lakes near Halifax, Nova Sotia and plot it.

```R
sddata <- read.delim("secchi_depth_data.txt")

# get first lake data
firstlakedata <- sddata[sddata$lake=="First Lake",]

# get second lake data
secondlakedata <- sddata[sddata$lake=="Second Lake",]

# plot first lake data
plot(firstlakedata$date, firstlakedata$sd, pch=18, xlab="Date", 
     ylab="Secchi Depth")
title("First Lake Secchi Depth")

# plot second lake data
plot(secondlakedata$date, secondlakedata$sd, pch=18, xlab="Date", 
     ylab="Secchi Depth")
title("Second Lake Secchi Depth")
```

You'll notice that in plotting the data for both lakes, **the code to plot the first lake is the same as the code to plot the second** (only the name of the lake is different). This bad for a few reasons: every time I change the code to plot the data for one lake I have to make a nearly identical change in some other line of code. Not only is this time consuming (imagine if you had 30 lakes), it's a great way to introduce errors into your project. The reason you can tell immediately that this was a bad move: **I had to copy and paste my code, then change it slightly**. Any time you run into this problem, consider making a function.

```R
sddata <- read.delim("secchi_depth_data.txt")

# define function to plot data
plot_secchi_depths <- function(lakename) {
  lakedata <- sddata[sddata$lake==lakename,]
  plot(lakedata$date, lakedata$sd, pch=18, xlab="Date", 
       ylab="Secchi Depth")
  title(paste(lakename, "Secchi Depth"))
}

# call function with both lake names
plot_secchi_depths("First Lake")
plot_secchi_depths("Second Lake")
```

With two lakes it doesn't make much of a difference, but it saves a whole lot of work the more times you have to copy and paste something. What if you wanted to change the title of each plot? It's a simple example, but you can see how we're working towards code that can easily be reused as opposed to code specific to your project.

Taking it a step further, we can *parameterize* the function, so we can customize each call we make to the function.

```R
sddata <- read.delim("secchi_depth_data.txt")

# define function to plot data
plot_secchi_depths <- function(lakename, pch=18, col="black") {
  lakedata <- sddata[sddata$lake==lakename,]
  plot(lakedata$date, lakedata$sd, pch=pch, col=col, xlab="Date", 
       ylab="Secchi Depth")
  title(paste(lakename, "Secchi Depth"))
}

# call function with both lake names
plot_secchi_depths("First Lake", col="green")
plot_secchi_depths("Second Lake", col="red")
```

If we wanted to make the function completely self-contained, we could also pass our original `data.frame` as an argument, so that not only could we use the function in this script, but we could use the function in *any* script that had a `sd` column and a `date` column.

```
sddata <- read.delim("secchi_depth_data.txt")

# define function to plot data
plot_secchi_depths <- function(alldata, lakename, pch=18, col="black") {
  lakedata <- alldata[alldata$lake==lakename,]
  plot(lakedata$date, lakedata$sd, pch=pch, col=col, xlab="Date", 
       ylab="Secchi Depth")
  title(paste(lakename, "Secchi Depth"))
}

# call function with both lake names
plot_secchi_depths("First Lake", col="green")
plot_secchi_depths("Second Lake", col="red")
```

You can see now that **we've created code that we could literally drop straight into another project without changing a single line of code** (provided we've formatted the data in the same way). This is a very simple example, but I have many plotting functions that I use over and over again when working on similar projects. I almost never get away with leaving a function completely untouched (there's always *something* I didn't think of when I wrote the function the first time), but **because I took the time to split my code into re-usable parts, I saved tons of time**. You'll hear this also called *modularizing* your code.

## Structure your files

## Use git to backup and share