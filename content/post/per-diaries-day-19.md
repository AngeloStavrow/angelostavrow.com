---
title: "The Per Rewrite Diary: Day 19"
date: 2020-02-19T18:15:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I tackle a memory leak.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per]. Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here].

## Whoopsie

So the plan today was to pause on writing code, and do what I think of as "catch-up" work. There are some inconsistencies among, for example, content view controllers — some have all kinds of view layout and initialization code in `viewDidLoad()`, others dump that stuff into a child `UIView` with `setupView()` and `setupConstraints()` methods. Pushing through on a daily basis with spike solutions and experiments makes for a lot of forward momentum, but it's important to take a step back and make sure you're tidying as you go — hence the list of 'papercuts,' or little issues that aren't a big deal on their own, but a real problem if they're left to accumulate.

_Aside: Remember that this is a fairly small and simple app being built by a single person, so it_ really _doesn't need a fancy and overcomplicated methodology. Experimenting and 'trying silly ideas' are not only allowed, they're encouraged._

So, that was the plan. But overnight I realized that something felt… _unsettled_ in my brain. Every time I create a new form view, am I making sure the memory is being deallocated when it's dismissed?

I _thought_ so — but you know that feeling. The one telling you that you've probably missed something.

So today I sat down and fired up Instruments, watching allocations as I navigate in and out of the add-product form. Sure enough, every time I present it, we get a `ProductDetailContentViewController` being allocated, but not being de-allocated.

Well, crap.

Who's got two thumbs and didn't give a delegate a `weak` reference? This guy. This, as you may know, creates a retain cycle, where the view controller can't be destroyed because it's got a strong reference to its delegate object. So, okay, add a `weak` keyword and we're done, right?

## 🚫🚫🚫🚫🚫

Nope. Xcode refuses to compile the code and gives me the following error if I try that:

```
'weak' must not be applied to non-class-bound
 'ProductDetailContentViewControllerDelegate'; 
 consider adding a protocol conformance that has a class bound.
```
Oh. Okay… so what does that mean? I asked [Frank] about it, and here's what he explained:

> "Make your protocol inherit from AnyObject. Essentially, the compiler is making sure your weak variable is a reference type and not a value type, because a weak value type doesn’t make sense."

And yes, making the `ProductDetailContentViewControllerDelegate` protocol conform to `AnyObject` fixed the issue. This was a case of me looking for the issue somewhere that was just far enough removed from the actual problem, that I couldn't see the fairly obvious solution.

A thing I'm noticing: if you feel like you're fighting the language/compiler, asking questions like "why doesn't `removeFromParent()`, y'know, _remove from parent_?" — step back. Re-evaluate and make sure you're asking the right question.

Oh yeah, and one final bit: _why_ doesn't a weak value type make sense? Well, _weak_ or _strong_ is in relation to the _reference_ one object has to another. You can't have a reference to a value type — you don't point to them, you copy them. You can only have a reference to the aptly-named reference type.

So, okay. Tomorrow, we're back to the papercuts.

[Per]: https://droppedbits.com/apps/per
[here]: /tags/per-rewrite-diary/
[Frank]: https://ioscoachfrank.com