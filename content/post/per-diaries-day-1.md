---
title: "The Per Rewrite Diary: Day 1"
date: 2020-02-01T15:30:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, it's view controllers all the way down.
tags: ["per", "per rewrite diary", "ios"]
---

> This post is part of a series about rewriting my iOS app, [Per]. Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here].

## Tabula Rasa

Rewriting Per from the ground up is going to be a learning experience, and I'm going to take this opportunity to write about the process as I go. I'm aiming to make one (relatively) small change per day, with the goal of having a functional MVP by the end of February 2020 that's not using any custom design work — i.e., using built-in animations, colour palettes, fonts, and icons.

Working this way —without custom design work— means that I can focus on design patterns, unit testing, and so on, moving quickly without getting blocked on UI decisions. It also de-couples the development work (which I'm doing) from the design work (which someone else is doing), so that we can move through this iteratively, but without deep dependency on each other's progress.

I should pause here to define the functional MVP. By the end of the month, I want to replicate the functionality of Per in its current iteration, plus one more feature, which means it should:

- Provide a quick, easy way to compare price per unit (existing feature)
- Handle unit conversion automatically (existing feature)
- Allow simple arithmetic when entering price or quantity (existing feature)
- Expand the number of compared products from two to "unlimited" (new feature)

So, I started with a single-view iOS app, added the bundle identifier, and then started ripping out **Main.storyboard** from everywhere. Thankfully, my podcast co-host Frank Courville posted a [great article] on the topic last month to follow.

## View controllers all the way down

I also started implementing a "layered view controller" concept presented in [another article] of Frank's — this uses **coordinator** view controllers, that manage **container** view controllers or wrap **context** view controllers that are composed of **content** view controllers. Read Frank's article, and be sure to download the sample app. You'll need to sign up for his newsletter, but I've been subscribed for a couple of years now and it's all been high quality articles on iOS development.

Per isn't an especially complex app, so this may seem like overkill. I'm going to be building this iteratively, though, and while this does mean I'll be creating several view controllers, this method will keep things small and loosely coupled.

Today was spent setting up this hierarchy.

Specifically, I created a simple top-level Product List coordinator view controller that sets a `UINavigationController` as its root view controller, which in turn embeds a Product List context view controller.

That context view controller wrap a Product List _content_ view controller that implements `UITableViewController`. Why a table view controller? That feels like the fastest way to add a list of several products for comparison. I didn't yet set up any delegates or data sources for that table view, nore did I create a Product detail view controller or any way to navigate to it.

I'll kick off work on that tomorrow by creating some very basic models.

[Per]: https://droppedbits.com/apps/per
[here]: /tags/per-rewrite-diary/
[great article]: https://ioscoachfrank.com/remove-main-storyboard.html
[another article]: https://ioscoachfrank.com/layered-view-controllers.html