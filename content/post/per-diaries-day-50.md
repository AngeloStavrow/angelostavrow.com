---
title: "The Per Rewrite Diaries: Day 50"
date: 2020-03-21T17:30:00-04:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I wrap up and recap this public daily development diary.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per](https://droppedbits.com/apps/per). Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here](/tags/per-rewrite-diary/).

## A Recap

Today is the fiftieth —and last— entry into this daily development diary. I'm not going to be discussing further work on Per v2 here, because at this point the work I'm doing is new features, and because I think it's probably getting tiresome for some readers.

(There are no analytics gathered on this site, so I don't know if readership has increased or decreased with this series.)

The goal was to get Per from a blank Xcode project to feature parity with the currently-shipping version in an hour a day, and to record my progress publicly as I went. I hoped to complete that work in February (so, 29 days). Non-goals included any kind of custom design work. As much as possible, I'm only using standard iOS 13 UI elements. That's not to say that custom design work won't be coming, but that wasn't part of this project.

Also, this rewrite is being done entirely in code — no Interface Builder work, no XIBs, and no storyboards (aside from the one required for the app's launch screen).

Without further ado, here's my takeaway on fifty days of public development journaling:

- **Public journaling means public accountability.** While I technically took a day here or there where I didn't really post anything beyond saying, "I'm taking a break today," I felt like I had to do _something_ every day just because I'd set that expectation. I made more progress in these past seven weeks than I did in the last… ugh, has it really been _four and a half years_ since the last update?

- **Limiting this to one hour (-ish) per day maintains forward momentum.** Because I only dedicate an hour or so per day to this task, I have to make sure that I plan my work accordingly. I've tried to end every entry with a simple description of what I'll tackle the next day, a trick I use in my daily work. Knowing that I only have an hour or so to work on it means that I scope the work down to fit.

- **It's a record of my work.** Writing it down makes it something I remember, but it's also something I can refer back to if I need to revisit insights. This is way more useful than comments in the source code. I only wish I'd thought to do something like a daily `git commit` with a link to the day's journal entry.

- **It can be stressful.** Beyond the COVID-19 pandemic, there's a lot going on in my life right now, and this extra commitment has sometimes felt overwhelming. Those tend to be the days when I say, "yeah, I'm taking the day off." Never trade your health in the name of "not breaking the streak." But if you feel _some_ capacity to do a little work, it's not a bad idea to do some catch-up and planning on days like that!

So, this has been fun, and very helpful. I'm going to continue working on Per, and I'll probably share something here now and again — but if you're interested in finding out more as the app gets closer to release, check out the Dropped Bits [blog] and [Twitter] account.

[blog]: https://droppedbits.com/news/
[Twitter]: https://twitter.com/DroppedBitsHQ/