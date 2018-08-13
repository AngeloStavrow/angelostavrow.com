+++
date = "2016-08-05T12:00:00-04:00"
draft = false
title = "How about that heat?"

+++

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">In other news, I’m totally ready for this sweltering summer to be over.</p>&mdash; Angelo Stavrow (@AngeloStavrow) <a href="https://twitter.com/AngeloStavrow/status/761251463213441025">August 4, 2016</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

I don't deal well with heat brought on by humidity. Which makes a Montreal summer pretty tough to deal with. Temperatures will routinely go over 30&deg;C, but when combined with high humidity, it'll feel even hotter.

Like, three-showers-a-day-ain't-enough hot. Grimy, unpleasant, swampy hot.

And most weather apps out there don't get it. Sure, they have a "feels like" temperature that compensates for some of it, but it never feels quite right.

To really understand how it works, you need a weather app made by Canadians, for Canadians. Because Canadians use a [humidex](https://en.wikipedia.org/wiki/Humidex).

![Feels like](/images/2016-08-05/feels-like.png)

Humidex distinguishes itself from the US [heat index](https://en.wikipedia.org/wiki/Heat_index) in that it's proportional to the dew point, not relative humidity. And generally, it's significantly higher.

I've seen days where the "actual" temperature (i.e., air temperature) is in the high twenties, but the "feels like" humidex is mid-thirties. That's a huge difference&mdash;what's comfortable to wear at one temperature may not be at the other. Or worse: if you're living with some medical conditions, the decision to leave the comfort of air conditioning may even be fatal.

A cursory look at weather apps (I've used many) shows that most use [Forecast.io](https://forecast.io)'s API. It's a great data source, especially for its hyper-local precipitation forecasts. But&mdash;and this of course makes sense, given market sizes&mdash;it doesn't use Canadian humidex calculations to derive the apparent temperature, so I never feel like I can trust these apps for forecast highs.

All I really want from a weather app is accurate alerts for incoming precipitation, and (humidex-corrected) forecast highs for the day. I've keep telling myself that I don't want to take on new projects, but having to switch between a couple of apps every day is kind of a pain.

🤔