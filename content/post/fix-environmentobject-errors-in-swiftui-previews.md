---
title: "SwiftUI Previews: How to fix 'Cannot convert value of type SomeType to expected argument type EnvironmentObject<SomeType>' errors"
date: 2020-07-31T14:30:00-04:00
draft: false
summary: "Using Environment Objects in a SwiftUI preview is surprisingly straightforward."
tags: ["swift", "swiftui"]
---

Using `@EnvironmentObject` is a great way to [share data between your views]. Previously, I'd been using `@ObservedObject`s all over my views, and it felt clumsy.

Hot tip: by setting an `environmentObject` for a `NavigationView`, any children of this `NavigationView` can then add a property like `@EnvironmentObject var someType: SomeType`. SwiftUI then gives them access to the observed object, without you having to pass the object down the navigation tree and through views that don't need access to it:

```swift
/* ContentView.swift */

import SwiftUI

struct ContentView: View {
    @ObservedObject var someType: SomeType

    var body: some View {
        NavigationView {
            MyView()
        }
        .environmentObject(someType)
    }
}

/* MyView.swift */

import SwiftUI

struct MyView: View {
    var body: some View {
        SomeTypeList()
    }
}


/* SomeTypeList.swift */

import SwiftUI

struct SomeTypeList: View {
    @EnvironmentObject var someType: SomeType
    
    var body: some View {
        // Do something with someType
    }
}
```

But! If you were passing it in to a child view as an `ObservedObject` and had set up some test object for use in your SwiftUI preview? If you try to pass in your test object, you'll get an error:

_Cannot convert value of type '`SomeType`' to expected argument type '`EnvironmentObject<SomeType>`'_

To fix this, use the `environmentObject` modifier on your child view's preview provider:

```swift
struct SomeTypeList_Previews: PreviewProvider {
    static var previews: some View {
        SomeTypeList()
            .environmentObject(testSomeType)
    }
}
```

Yay, your preview works again!

[share data between your views]: https://www.hackingwithswift.com/quick-start/swiftui/how-to-use-environmentobject-to-share-data-between-views