---
permalink: /r_ucsb_2019/pt1/
title: "R Workshop - Part 1"
header:
  overlay_image: /assets/images/ant_diving_green.jpg

---

[Back to main workshop page]({{ site.url }}/r_ucsb_2019/)

> # Objectives:
> * Learn why we are using R
> * Become familiar with the R environment
> * Learn how to do math and assign values to variables/objects
> * Use comments to inform the script
> * Import a spreadsheet of data
> * Exploring a data frame
> * Explore plotting the data
> * Pointing you towards future resources

# Why R?


>I already know how to use excel, and have done some statistics in other programs (e.g. JMP). Why do I need to learn ANOTHER thing?


You've may have heard of the importance of  `reproducible science`. In a well-written paper, all of the technical steps are laid out so somebody else can replicate the experiment exactly with the information given. These `reproducible methods` are super important for scientific progress.

This same idea applies to data analysis!  In a perfect world, I want to see your original entered data sheet (the one you typed in the data onto), as well as every step along the way (until you get to your final graphs and statistics). This can be hard for me to see in excel, as it is hard to retrace your steps and know everything that you did.

If you do all of this data formatting, calculation, plotting, and stats in a `programming language`, it is automatically keeping a track of everything you are doing. Every line of code is a "step" between your raw data and your result!

More and more journals nowadays expect that your programming code and your raw data are made available online. This means that even Scientific programming isn't just for quantitative biologists anymore! Power to the people!

## Starting with variables

You can get output from R simply by typing math in the console:

```
3 + 5
12 / 7
```

However, we want more than a fancy calculator right? You were promised so much more! Things become very interesting when you can **assign values to objects**. To create an object, we need to give it a name, then the assignment operator `<-`, then the value we want to give it:

```
weight_kg <- 55
```

We have now put the value of `55` "into" `weight_kg`.

```
weight_kg
```

Object names can't start with a number (e.g. `2ndTreatment` is no good but `treatment_2` works). In general you don't want to give something a name that's already taken. If we make another thing called `weight_kg`, we will "overwrite" the first one:

```
weight_kg <- 60
# Type name of object to print
weight_kg
```

Now that r "remembers" `weight_kg`, we can do some math with it! Let's find out what 60kg is in pounds:

```
# pounds is 2.2x kg.
2.2 * weight_kg
# did that change weight_kg?
weight_kg
```

We can also change an object’s value by assigning it a new one:

```
weight_kg <- 57.5
2.2 * weight_kg
```

let's save the result of this calculation to a new variable, ``weight_lb`:

```
weight_lb <- 2.2 * weight_kg
weight_lb
# change weight_kg:
weight_kg <- 100
# what is the result here?
weight_kg
# what about now?
weight_kg <- weight_kg / 2
```

## Using comments to inform the script

In the above examples, I am using a pound sign / hashtag to "comment" the script. This lets me write regular english throughout my code in order to understand it better. this will be super helpful going ahead in the future!

Sometimes, I even start a script by writing in what I want to do in a few comments. Then I fill in the space between the comments with the code.

## Using functions in R

You're likely going to very quickly want to do something a little more than addition, subtraction, multiplication, and division in R. For this we will use our own *functions* that allow us to do more complex operations. A function executes a more complex chunk of code. Functions look a bit like variables, but have parentheses after them. the "input" of the function goes into these parentheses.

![Diagram of a function](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3b/Function_machine2.svg/200px-Function_machine2.svg.png)

Functions can be anything! functions allow us to plot, calculate, transform, and operate on data in any way. they can be as simple as taking the square root of a number:

```
# square root of the weight from above
b <- sqrt(weight_kg)
b
```

or rounding a number:

```
round(3.14159)
# Let's see if round has any more tricks:
?round
round(3.14159, digits = 2)
```

## Data types in R: vectors and data frames.

So far, every variable has represented a single number. However, variables in R can represent much more. they can represent words as well...
```
animal <- "sea urchin"
```

...or even groups of numbers and words.

A "list" of things is called a vector in R. We make lists using the command `c()`:

```
weight_g <- c(10, 30, 40, 15)
weight_g
# this tells us the "class":
class(weight_g)
```

We can pull out individual values from the vector using brackets:

```
# extract first "element" of the vector:
weight_g[1]
weight_g[2]
# we can also pull out multiple into a smaller
weight_g[c(1,2)]
weight_g[c(1,1,1,1,1,2)]
```

## Logical Indexing

## Importing Data

Make a folder on your Desktop called "Workshop", then put this spreadsheet inside of it:

*[Download link]({{ site.url }}/assets/mammals.csv)*

This file is a `csv`, or "comma-separated value". It is the best way to import data into R. You can easily save your excel spreadsheets into a csv file ("save as" in excel), but you will have to save each "sheet" as a separate `csv`. There are ways to read excel `.xls` files directly into R, but we won't be covering that [link to that code here, though](https://readxl.tidyverse.org/)

### Finding out where we are on the computer

R will let you navigate through the computer. A new project will probably start you out in your "Documents" folder, or in your "Home" folder (where you Document folder is).

```
# Where are we?
getwd()
# What files are there?
list.files()
```

### Moving around on the computer

Let's move into the folder where our data is.


If you start in your Home folder, you just need to go onto your desktop, then into your workshop folder, then get mammals.  

```
setwd('Desktop/workshop')
getwd()
```

If you start in your documents folder, you need to go 1 folder "up", or go into the enclosing folder. That require a `..`:

```
setwd('../Desktop/workshop')
getwd()

# .. always gets you 1 folder "up":
setwd('..')
getwd()
setwd('workshop')
setwd('../workshop') # go out of workshop ,and right back in.
```

If you ever get "lost", you can always go back to your "starting point" with a tilde `~` (above the tab key).

```
setwd(`~`) # ET PHONE Home
# If you don't know where you're starting, you'll ALWAYS get to the workshop folder this way:
setwd('~/Desktop/workshop')
```


Now let's import the data! To do that we use another function called `read.csv`. `read.csv` takes a file name as an input, and returns the contents of that file.

```
read.csv('mammals.csv')
```

*Note: You could've put in the whole name of the file from the start ('Desktop/workshop/mammals.csv'). So `read.csv` can understand that as well! *

Whoops, just like in the beginnign when we asked R `4+2`, the "output" of `read.csv` isn't assigned to a variable name. we can do that here:

```
mammals = read.csv('mammals.csv')
```
