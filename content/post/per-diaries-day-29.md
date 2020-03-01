---
title: "The Per Rewrite Diaries: Day 29"
date: 2020-02-29T09:19:17-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I continue work on the automatic unit conversion feature's UI.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## Bleh

I was really hoping to wrap up the conversion feature today, but I'm struggling to get a `UIPickerView` working the way I'd like.

Essentially, I want to be able to set the rows of the picker view based on the `UISegmentedControl`'s selected segment, but for whatever reason I just can't get it to work the way I'd like.

I'm also feeling very distracted today, so I've been having a hard time figuring out just why this isn't working. That's... okay. I mean, it's a little frustrating, but it's okay. I think part of this is coming from trying to force something so that I can write about it, instead of allowing myself the time to go deep and understand what's going on.

This is the last day of the month, but I'm going to continue journaling on a daily basis at least until this functional rewrite is done.