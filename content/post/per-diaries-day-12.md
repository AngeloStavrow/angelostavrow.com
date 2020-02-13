---
title: "The Per Rewrite Diary: Day 12"
date: 2020-02-12T07:45:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I start planning my work on the product detail views.
tags: ["per", "per rewrite diary", "ios"]
aliases: ["/post/per-diary-day-12/"]

---

> This post is part of a series about rewriting my iOS app, [Per]. Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here].

## In The Details

On [Monday] I worked on a plan for the week. I tackled a simple clear-the-list feature yesterday, and today I'm going to start —but probably not finish— work on the product detail view(s) that let users enter, uh, product details.

Again, some planning is worth the effort before jumping into writing code. The goal of this part of the rewrite is to get something functional up and running before doing any custom design, and to learn, but making it easy to change a codebase is more invoved than you'd think.

<img title="A screenshot of Per's product-entry screen" src="/images/2020-02-12/per-product-entry-screenshot.jpg" style="width:240px;margin-left: 1em;float: right;lock;0px;"/>

In its current form, a product is entered into Per as a set of three text fields, as shown. And it's... not wonderful.

The design sacrifices a lot for the sake of compactness — in my initial sketches, I wanted Per to handle as much input as possible in a single view, to make it very quick and easy to get in and out of the app while, say, doing your groceries.

For v2, users aren't limited to comparing two products, so we necessarily need a separate view for product entry. This gives me a lot more room to breath, as it were, so starting off with three sets of label-plus-textfield input areas is a good start! 

This is where [stack views] are super helpful. I feel that [flexbox] is a good analogue to stack views in the web development world; you create either a row or a column of views and align them along the main and cross axes according to some rules, and voilà! You have a basic layout. And you can nest stack views, for something like this:

```
+------------------+   ^
|                  |   |
+------------------+   |
|                  |   |
+------------------+  Vertical stack view
|                  |   |
+==================+   |
||       ||       || <-+-- Horizontal stack view
+==================+   V
```

Tomorrow, I'll start implementing something like this by putting the existing **Add** and **Cancel** buttons in a horizontal stack view.

[Per]: https://droppedbits.com/apps/per
[here]: /tags/per-rewrite-diary/
[Monday]: /post/per-diaries-day-10/
[stack views]: https://developer.apple.com/documentation/uikit/uistackview
[flexbox]: https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Flexbox