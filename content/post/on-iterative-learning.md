+++
date = "2016-03-25T12:00:00-04:00"
draft = false
title = "On iterative learning"
aliases = [ "/blog/2016/3/25/on-iterative-learning" ]

+++

Earlier this week, I had a chance to talk out my approach to learning new things in the context of Cocoa development.

I've always found that I learn best when I get to [use it in anger](http://english.stackexchange.com/questions/30939/is-used-in-anger-a-britishism-for-something), but when it comes to something as big and sprawling as the Cocoa framework, there's a _lot_ of stuff to learn.

Given that I do the vast majority of my development solo, I've found that the best way to approach this is to break the framework down into manageable chunks, write a little project based around that chunk, and then ship it.

<hr>

I wrote my first app, [HoneyJar](http://droppedbits.com/honeyjar), as a first introduction to [Objective-C](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011210-CH1-SW1) and Xcode. IDEs like Xcode are generally pretty similar&mdash;at their core, they're text editors with some special features&mdash;but Objective-C looked like no other programming language I'd ever seen before.

```
- (void)doYouKnowTheMuffinMan:(TheMuffinMan)theMuffinMan;
```

_(I saw this somewhere on Twitter and I wish I could remember who to credit for it!)_

I also made the app iPhone-only, as this made for a much less complex platform as far as things like filesystems, window sizes, and interaction go.

I also took the opportunity to explore [CocoaPods](https://cocoapods.org), adding some libraries to handle some view and viewcontroller tasks&mdash;I had my hands full trying to figure out the language and the platform. I also integrated [HockeyApp](http://hockeyapp.net/features/) into the beta version I was working on, to get feedback from friends.

Once I was relatively happy with the app, I revved it to 1.0 and submitted it to the App Store&mdash;again, something I'd never done before. HoneyJar was never expected to be some kind of successful product, but a few people have downloaded it, and I know it's been useful to me on occasion.

<hr>

When Apple [announced Swift](https://swift.org/), its new programming language, I got pretty excited. Swift certainly _looked_ a lot more like the programming languages I was familiar with, and there was something really neat about "growing up" with a new language.

So, I started work on my next app, [Per](http://droppedbits.com/per). Per was all about digging into Swift (which has changed dramatically since the app was written)&mdash;there are no external libraries used anywhere in the app.

I also used it as an opportunity to play with some animations when presenting and dismissing view controllers:

![Per's unit-converter feature](http://static1.squarespace.com/static/53ebc004e4b0aa86f612f6fd/t/54ea85a1e4b08f2aaf399192/1424655859170/unit-conversion.gif?format=500w)

This meant that I got to look into [closures](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html) as well, again as a small chunk of their applicability.

<hr>

Last week I wrote about [writing for GitLab](/blog/2016/3/11/on-writing-for-gitlab) as a means of teaching myself more about their continuous integration features.

I'm now interested in working with [fastlane tools](https://fastlane.tools/), so I'm working on an app as a counterpart to [a new article I'll be writing for GitLab](https://gitlab.com/gitlab-com/blog-posts/issues/169). The app is relatively simple: it consumes GitLab's  API to present some kind of a dashboard for project milestones that the user is involved in.

Because I want to focus on learning fastlane, I don't want to worry _too_ much about handling API requests and responses, so I'm using a great library called [Alamofire](https://github.com/Alamofire/Alamofire), _and_ I'm making this a [tvOS app](https://developer.apple.com/tvos/).

Why tvOS?

Well, firstly, it's a new platform that I get to play with. That's pretty interesting in and of itself, because while it brings a standardized screen size, it also includes the Siri remote as the sole means of interaction.

But, additionally, an Apple TV assumes a persistent connection to the internet, which means one less thing to worry about: testing for a lack of connectivity (or worse, _spotty_ connectivity).

This way, I can learn about a new platform and a new toolchain without worrying about other pitfalls and perils.

<hr>

So that's how I like to learn: small, neatly defined chunks, where I can focus on the thing I want to find out about, and ship something that showcases what I've learned. I'd love to know what works best for you!
