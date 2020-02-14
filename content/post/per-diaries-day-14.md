---
title: "The Per Rewrite Diary: Day 14"
date: 2020-02-14T08:30:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I start work on laying out the product detail view.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per]. Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here].

## Getting to MVP

So, working with stack views is fun!

[Yesterday] I created a first stack view to layout the **Add** and **Cancel** buttons in the add-product view, with today's goal being to layout the rest of the view.

It was pretty straight forward, and now I have a functional-ish app! I can enter a (unitless) product and it'll get sorted according to price per unit:

!["Two screenshots of Per in action, displaying the add-product screen and the product list screen"](/images/2020-02-14/screenshots-progress-2020-02-14.png)

Of course, because there's _no_ validation going on, you can also crash the app by simply tapping the **Add** button when you don't enter any text. And you can't select anything other than "units" for the product you're adding. Still, technically, I could install the app on my phone now and start using it!

I've also now made a huge mess of the `ProductDetailContentViewController`, too. In it, there's all the setup code for the stack views (3), the text fields (3), the buttons (2), and a label. All of that should be broken up into at least two view controllers: one for the inputs, and one for the buttons, and validation can be added as necessary there.

So, breaking up this not-quite-massive-but-kinda-huge view controller is what I'll work on tomorrow.

[Per]: https://droppedbits.com/apps/per
[here]: /tags/per-rewrite-diary/
[Yesterday]: /post/per-diaries-day-13/