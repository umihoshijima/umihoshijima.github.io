---
layout: page
title:
feature_text: |
  # Quick start in R!
permalink: /r_intro_201907_pt1/
---
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


You've likely heard of the imprtance of  `reproducible science`. In a well-written paper, all of the technical steps are laid out so somebody else can replicate the experiment exactly with the information given. These `reproducible methods` are super important for scientific progress.

This same idea applies to data analysis!  In a perfect world, I want to see your original entered data sheet (the one you typed in the data onto), as well as every step along the way (until you get to your final graphs and statistics). This can be hard for me to see in excel. I would often drag data around to make it easier to plot. Then I might make a calculation column with an equation, then make another column with equations, etc... until I end up with a completely different spreadsheet from where I started! This means that it's hard to retrace your steps and know everything that you did.

If you do all of this data formatting, calculation, plotting, and stats in a `programming language`, it is automatically keeping a track of everything you are doing. Every line of code is a "step" between your raw data and your result!

More and more journals nowadays expect that your programming code and your raw data are made available online. This means that even Scientific programming isn't just for quantitative biologists anymore! Power to the people!

# Let's get started, then!

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
```
