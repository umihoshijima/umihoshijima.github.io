---
layout: post
title:  "weird quirks with r and datetimes - pt. 2"
date:   2019-03-29 12:00:00 -0700
categories: R lubridate
---

Last time we left our intrepid hero, they were [attempting to navigate time zones in R](http://umi.science/r/lubridate/2019/03/18/datetimesInR/). Weeks later, this is still something that I am having constant issues with.

In my field, we deploy quite a few sensors to monitor environmental factors. These sensors are often deployed in GMT for consistency, but can often also be deployed in local timezones. In collaborative projects with multiple lab groups, my code has to account for these differences - and this can be quite frustrating.

In order to streamline the process, I attempted to create a script that can assign a different timezone depending on a TRUE/FALSE designated earlier. Here is a simple test case:

{% highlight r %}
library(lubridate)
test_time ='2018/01/01 04:00:00'
{% endhighlight %}

We first start by loading my favorite R time-management tool, [lubridate](https://lubridate.tidyverse.org/). Being bad at time management in all definitions of the phrase, I am happy to find something that is relatively painless.

Then, I create a simple `if/else` statement to take the `test_time` variable, which exists and a string, and "cast" it as a datetime object using the appropriate time zone:


{% highlight r %}
bool = TRUE
if(bool)
{
  time_corr = ymd_hms(test_time, tz = 'GMT')
} else{
  time_corr = ymd_hms(test_time, tz = 'US/Pacific')
}
{% endhighlight %}

Changing the first line to `bool = FALSE` allows us to have R set the timezone automatically!

## Trying to make things too fancy  

In a classic case of overconfidence, I attempted to streamline this code. I knew that, in the past, I have used the function [ifelse()](https://www.rdocumentation.org/packages/base/versions/3.5.3/topics/ifelse) to make my code look prettier. This works particularly well for cases like the simple if/else case that I highlight above.

Directly adapting the above code chunk into an `ifelse()` looks something like this:

{% highlight r %}
time_corr = ifelse(bool,
                   ymd_hms(test_time, tz = 'GMT'),
                   ymd_hms(test_time, tz = 'US/Pacific'))
{% endhighlight %}

You put the logical as the first argument, then the if/else results in AFTER it in that order. This looked SO much better to me that I was rather dismayed when I ran the code and got the following result:

{% highlight r %}
 time_corr
[1] 1514779200
{% endhighlight %}

So it appears that this DOES take the right timezone, but then unfortunately converts that datetime to a `numeric`! This huge number is how R is handling time behind the scenes - every time is saved as "seconds since 1970/01/01, midnight, GMT". We can confirm this using the following line:

{% highlight r %}
ymd_hms('1970/01/01 00:00:00', tz = 'gmt') + time_corr
{% endhighlight %}

## Why does this happen?

The answer to this is deep in the documentation for `ifelse`. Buried deep-down in the comment of an example is the line:

`ifelse() strips attributes This is important when working with Dates and factors.`

I can't believe I let that chew up so much of my time.

## Final result:

I found a function that is relatively easy to tack on to the ifelse, making the result look something like this:

{% highlight r %}
time_corr = ifelse(bool,
                   ymd_hms(test_time, tz = 'GMT'),
                   ymd_hms(test_time, tz = 'US/Pacific')) %>%
  as_datetime()
{% endhighlight %}

In the end, I'm not sure if I saved any lines of code, but I'm still convinced it looks slightly prettier than when I started out. I guess I managed to learn something in the process? 
