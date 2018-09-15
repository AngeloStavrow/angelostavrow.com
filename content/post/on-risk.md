---
title: "On risk"
date: 2018-09-15T11:00:00-04:00
tags: ["progress", "development"]
draft: false
---

Standing still is generally a safe bet. We don't have to expend any energy, and we don't have to concern ourselves much with the possible outcomes. We just stand there, and watch the world go by.

Making progress is a little different. Making progress is risky.

<!--more-->

I was inspired by an [interview with marathon swimmer Kim Chambers on the Hurry Slowly podcast][hurryslowly] that made me think about why we don't do things that seem risky, and why others don't seem to struggle with it.

## Fortune favours the prepared

Right now, I'm working on adding a feature to FogBugz/Manuscript. When you're working with a codebase this old, there's naturally going to be some measure of risk involved. There are a lot of moving parts involved, some of which may not be especially well documented, spread out over hundreds of thousands of lines of code in several projects.

So, as thoughtful developers, we work to minimize the risk involved by working with a process.

The first step in a project like this is to clearly state the goal. That's usually not especially hard: "add custom logging level" might be the goal.

Next, [we write a spec][spec]. Specs flesh out the project with more detail. If the goal is your destination, then the spec is route you map out to get you there. This isn't a step to be taken lightly; you need to understand the requirements, the current implementation, the perils and pitfalls, and the non-goals. The more work you put into plotting the route, the less uncertainty you face when you start your trip.

- What are we doing right now? How do we do it?
- What do we want to add? Why?
- What do we want to avoid adding? Why?
- Where do we make these these changes?
- How do we make sure we don't break things?
- Do we understand where the path gets tricky or dangerous?

Once you have a destination and a route, you can start moving ahead.

## Course corrections

Now, when we're taking a road trip in an old car, we _could_ just jump in and go, but that's ill advised. We probably want to check our tires and fuel, at a minimum. Did we bring enough music or podcasts to listen to? What about snacks? Okay, let's take stock of that before we go.

Okay, cool. Floor it. Full speed ahead, right?

_Nope._

We checked the weather forecast before we left, right? Not just at our destination, but at reasonable points along the way? Should we be concerned about that intermittent oil-pressure warning? That could be a faulty sensor, but maybe we should check the levels every so often.

Making forward progress isn't always about momentum. It's also about preparation, planning, and checking in to see where we're at.

So when we're working on this feature, we make a few little changes, one at a time, and examine what happens. Do our tests still pass? Are the results expected?

Okay, we're getting familiar with how this all works. Maybe we take a crack at a slightly bigger task. We write a big, messy function and a handful of new tests as an experiment — no big deal, because we're committing your changes and can roll back to a known-good state, right?

Build and test. The compiler yells at us. Okay, okay, let's step through this in the debugger to see what's happening. Okay, that property is undefined when we reference it. Why? Where does that happen? Got it. Let's fix that.

And so it continues. And we make progress, refactoring the messiness, getting changes reviewed by the team, and moving on to the next step. Maybe we need to update that spec. Maybe not.

And eventually, we reach that goal. The feature is code complete. It's in integration, tests are passing, and our hard work will be successfully deployed to production.

## Stacking the deck

Of course, this isn't just a story about how to safely add a feature to legacy code. This is true of any goal that you might be having trouble with. Risk —whether perceived or real— is an impedement to progress, but much of it can be overcome, and setting yourself up for success is about how you chip away at that risk.

Making forward progress towards a goal isn't guaranteed by rushing headlong at a problem with boundless enthusiam and a devil-may-care attitude. It's about the cycle of [planning, doing, checking, and adjusting][pdca]. It's about exploring the boundaries of what we know, and testing what we _think_ we know, slowly but surely casting the light of understanding into the dark corners of our ignorance.

And the more we do it, the better we get at understanding how artificial the limits we place on ourselves really are.

[hurryslowly]: https://hurryslowly.co/004-kim-chambers/
[spec]: https://www.joelonsoftware.com/2000/10/03/painless-functional-specifications-part-2-whats-a-spec/
[pdca]: https://en.wikipedia.org/wiki/PDCA