---
title: "WriteFreely Swift Package"
date: 2020-06-29T12:00:00-04:00
draft: false
summary: I've released a developer preview for a Swift package to work with WriteFreely.
tags: ["writeas", "projects", "swift"]
---

It's been a while since I posted an update here, and I wanted to announce a new project: a [WriteFreely Swift package] that you can drop into your Mac and iOS apps.

You can find it here: https://github.com/writeas/writefreely-swift

## What's WriteFreely?

[WriteFreely] is an open-source platform for writing on the web. It powers the Write.as service (find me [here]!), and lets you build writing communities on the web.

I was introduced to Write.as and WriteFreely while working with Glitch, where I got to chat about the service and [its principles] with Matt and CJ. Matt wrote a thoughtful [post on Glimmer] about the importance of privacy for creating on the web, and CJ built a tonne of cool sample apps that connected to the Write.as service for their [Glitch team].

## Cool, Tell Me More About The Project

This project represents a couple of firsts for me:

- This is the first time I've worked on a Swift package
- This is the first time I've wrapped a RESTful API in Swift
- This is the first time I've worked with [`URLSession`] and [`Result`] in Swift

Right now, it's an alpha/developer-preview release. There's a lot of room for improvement here, and I'm looking forward to working towards a 1.0 with the WF community.

As I mention in [this forum topic] and my [Write.as post], the design for the `WriteFreelyClient` is to leverage completion blocks that return a `Result` tuple with either a `User`, `Post`, or `Collection` (or an array of these types where that makes sense), or an `Error` on failure. That makes it pretty easy to build completion handlers:

```swift
func loginHandler(result: (Result<User, Error>)) {
    do {
        let user = try result.get()
        print("Hello, \(user.username)!")
    } catch {
        print(error)
    }
}

guard let url = URL(string: "https://your.writefreely.host/") else { fatalError() }
let client = WriteFreelyClient(for: url)
client.login(username: "username", password: "password", completion: loginHandler)
```

## What's Next For The Project?

Good question! There are definitely some major to-do items that are obvious to me:

- Add a test suite (there will be some refactoring required to facilitate this)
- Create generic-ish request templates to [DRY] out the `WriteFreelyClient` public methods
- Extend for use with the Write.as platform

Mostly, though, I'm excited for people to try it out and let me know how it works for them!

<!-- Links -->

[WriteFreely Swift package]: https://github.com/writeas/writefreely-swift
[WriteFreely]: https://writefreely.org/
[here]: https://write.as/angelo
[its principles]: https://write.as/principles
[post on Glimmer]: https://glitch.com/glimmer/post/write-as-privacy-centric-web-platforms
[Glitch team]: https://glitch.com/@writeas
[`URLSession`]: https://developer.apple.com/documentation/foundation/urlsession
[`Result`]: https://developer.apple.com/documentation/swift/result
[this forum topic]: https://discuss.write.as/t/writefreely-swift-package/1564
[Write.as post]: https://write.as/angelo/writefreely-swift-package-v0-1-0-released
[DRY]: https://en.wikipedia.org/wiki/Don%27t_repeat_yourself