---
title: "Writefreely for iOS v1.0"
date: 2020-10-21T13:15:00-04:00
draft: false
tags: ["writeas", "projects", "swift"]
---

I'm thrilled to announce the release of [WriteFreely for iOS] on the App Store. 🚀
<!--more-->
<img src="/images/2020-10-21/wf-ios-hero.png" style="display: block; margin: 0 auto; width: 600px" />

As I've discussed [here] and [elsewhere], I've been working with the [Write.as team] since this summer to build the official [WriteFreely] app for Mac and iOS. Yesterday, the iOS app launched on the [App Store].

It's been several years since I last published an app to the App Store. In the interim, I've largely been working in web development, where I found myself loving the near-instant feedback loop when making changes, the ease of creating adaptive layouts for all screen sizes with things like flexbox, and the incredibly simple release process (deploy to server &rarr; done).

Developing native iOS apps, in comparison, was a slog. Having to build and run every time you made a change is _slow_. Using AutoLayout is a right [pain in the ass]. Building an app that runs across all of Apple's platforms means not only testing in several Simulators, but working multiple targets with [wildly] [different] SDKs.

That's ([mostly]) all changed with the SwiftUI announcments made at WWDC this year. Now, multiplatform apps can be built for Mac and iOS with a shared codebase, and previews of the UI right in Xcode make it spectacularly fast to iterate on a design.

> This is building a native app with the speed of iteration that I've only ever encountered when building for the web.

As I posted in [this Hacker News submission], it hasn't been all roses. Some outright weird bugs and peculiarities with SwiftUI have bitten this project — beyond, y'know, working with a beta framework in a beta IDE on beta operating systems, or even having to change from an imperative to a declarative way of writing code. Things like `preferredColorScheme` worked mostly okay on iOS, but not on macOS; on the flip side, subscribing to a `didFinishLaunchingNotification` publisher works fine on macOS, but forget it on iOS. TextField controls have placeholder text, but multi-line TextEditor controls don't.

Ah, yes, the TextEditor control. Obviously, in a _blogging_ app, that's going to be important. On iOS, it works brilliantly — I've written a few posts in the app on my iPad with a keyboard attached, and it's performant even with `onChange()` handlers watching it. On macOS 11, though, it's been a wildly different experience. In the early betas of Big Sur, an accumulated input lag made it so that the app was a couple of words behind within two sentences.

This issue alone was the deciding factor in pushing to ship the iOS app first, and adopting a wait-and-see approach for the Mac app. On beta 10, it's much improved, but still present after a few paragraphs. That's great to see, but it's still not ready for primetime.

<img src="/images/2020-10-21/new-local-draft.png" style="display: block; margin: 0 auto; width: 600px" />

So, what's next? A 1.0.1 bugfix release for iOS is what I'm working on right now, and then it's time to get back to the Mac app. It's mostly at feature parity with the iOS app, but it still needs a lot of polish and love, and —if it doesn't get fixed in the next Big Sur beta— a workaround for the input lag issue in the post editor.

As it's an open-source project, you can see what we're working on in the [GitHub repo], or chat about what you'd like to see in [this forum topic]. 

More TK!

<!-- References -->
[WriteFreely for iOS]: https://writefreely.org/apps/ios
[here]: /post/wf-swiftui-mpa/
[elsewhere]: https://write.as/angelo/
[Write.as team]: https://write.as
[WriteFreely]: https://writefreely.org
[App Store]: https://apps.apple.com/us/app/writefreely/id1531530896
[pain in the ass]: https://twitter.com/jamesthomson/status/1318938009413234694
[wildly]: https://developer.apple.com/documentation/uikit
[different]: https://developer.apple.com/documentation/appkit
[mostly]: https://write.as/angelo/the-promise-vs-reality-of-swiftui-multiplatform
[this Hacker News submission]: https://news.ycombinator.com/item?id=24846559
[GitHub repo]: https://github.com/writeas/writefreely-swiftui-multiplatform
[this forum topic]: https://discuss.write.as/t/writefreely-swiftui-multiplatform-app-mac-iphone-ipad/1664