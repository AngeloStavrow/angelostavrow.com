---
title: "Using Swift Packages in all Multiplatform SwiftUI App targets"
date: 2020-07-22T08:30:00-04:00
draft: false
summary: SwiftUI multiplatform apps have both iOS and macOS targets, but adding a Swift Package dependency to your target via Xcode requires you to choose one. So how do you give both targets access to the package?
tags: ["spm", "swiftui", "swift"]
---

A little bit of a UI/UX deficiency I've found when using Swift Package Manager in Xcode 12 &beta;2 is that adding a Swift package to a multiplatform (iOS/macOS) SwiftUI app requires you to choose a target for the package:

!["Add Package to App sheet in Xcode showing a target selection menu"](/images/2020-07-22/add-to-target.png)

That means that if you have a platform-agnostic package that you import, and try to use it for both the iOS and macOS targets, you'll invariably run across an error ("_No such module 'ModuleName'_") when you're writing code against the target you _didn't_ add it to in the above step, and your project won't build for that target.

{{< tweet 1285685565938176000 >}}

To fix this, [Stuart Breckenridge](https://stuartbreckenridge.com/) shared the following tip: go to the **Build Phases** tab for each of your targets, and make sure it's added under **Link Binary with Libraries**:

!["Target Build Phases settings in Xcode"](/images/2020-07-22/target-build-phases.png)

Build the app, and you'll be good to go.

I've filed FB8094575 to improve this user flow. 