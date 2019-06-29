---
layout: post
title:  "Timezones in R with Lubridate"
date:   2019-03-18 12:00:00 -0700
categories: R lubridate
---

*Note: This is by no means an exhaustive post, but one highlighting some issues I have had with R datetimes.*

In working with ocean sensors across the globe, I often have to deal with data collected in a variety of timezones. Often, subsets of the data will be collected in GMT/UTC, while others are taken in local time. Aligning these has always been, and will continue to be, a challenge for me.

One function that has made this more manageable is `lubridate`, which I can at this point not live without. Others have done far better write-ups of this package, so I will not bore anyone with my worse descriptions. [Here](https://lubridate.tidyverse.org/) is a great starting point.


Now I have had several issues recently with time, specifically timezones. To start with let us establish that I want to take a time, and have R recognize it as a specific timezone.

{% highlight r %}
library(lubridate)
str_time = '2018/01/01 00:00:00'
time_1 = ymd_hms(str_time, tz = 'US/Pacific')
{% endhighlight %}

`time_1` is now recognized as midnight on New Year's, 2018.

For reasons I do not necessarily want to get into, I wanted to manually set this timezone using "hours from GMT". This is a typical way in which timezones are displayed. In our case, we know that this location (Pacific coast of USA) is 8 hours behind UTC, and is usually displayed as `UTC -0800` during non-daylight savings dates. to first confirm that our site is not under daylight savings time, we can run the line:

{% highlight r %}
dst(time_1)
{% endhighlight %}

This should return "FALSE". To find a timezone aligning with our `UTC -0800` designation, we run the R function `OlsonNames()`. This returns all of the recognized timezones of our system. I find one labeled `GMT-8`, and happily try to use that:

{% highlight r %}
time_2 = ymd_hms(str_time, tz = 'Etc/GMT-8')
# Confirm:
time_1 == time_2
# FALSE
{% endhighlight %}

So it turns out these times are not entirely equivalent, but are 16 hours off! how could that be?

Well, it turns out that deep in the help file for `OlsonNames()` it states:

{% highlight r %}
?OlsonNames
{% endhighlight %}

*Most platforms support time zones of the form Etc/GMT+n and Etc/GMT-n (possibly also without prefix Etc/), which assume a fixed offset from UTC (hence no DST). Contrary to some expectations (but consistent with names such as PST8PDT), negative offsets are times ahead of (east of) UTC, positive offsets are times behind (west of) UTC.*

So a very frustrating outcome, but I suppose it's in the help file. I just hope that this eventually helps somebody equally frustrated that is googling for an answer. 
