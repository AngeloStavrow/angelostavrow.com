+++
date = "2016-05-28T12:00:00-04:00"
draft = false
title = "TGHR: Opening the source"
tags = [ "tghr" ]
aliases = [ "/blog/2016/5/28/tghr-opening-the-source" ]

+++

<blockquote><strong>The Great HoneyJar Refactoring</strong> is a series of posts in which I take the first iOS app I ever wrote, <a href="http://droppedbits.com/honeyjar/">HoneyJar</a>, and refactor it out of its original burning-dumpster-fire state and into a modern app. And I'm doing it <a href="https://gitlab.com/AngeloStavrow/HoneyJar">in public</a>.</blockquote>

[Last week][1] I introduced an idea I had: to open-source my first-ever iOS app, embarrassing as it might be, and refactor it out in the open.

Over the last week, I handled the open-sourcing of the app. This took a bit more time than I expected, and normally this should be done _before_ releasing the project to the wild, but I figured it might be interesting to see just what the process was, what I learned, and some resources I found along the way.

## The starting point

HoneyJar, in its closed-source state, was very clearly an "I'm learning as I go and am super excited to ship" kind of project. It had a [README][2], kinda, which really just re-iterated some tagline-style description of the project, and that's it. Unit tests were few and far between, comments were… [not awesome][3], and there was no real structure in place to make it useful for other contributors.

So the first step was to do exactly that. I opened the [first issue][4] and set up a task list:

- Add a CONTRIBUTING document
- Add a LICENSE document
- Add a CHANGELOG document
- Update the README to provide more information on the project
- Set up GitLab continuous integration

Okay. I know. That _sounds_ like a pretty straightforward list. Quick and easy to add. Turns out, you should put a little effort into these things.

## Readez-moi

The first thing most people will see in a project is the README file. This should provide some information on how to install/use the project, where to find further info on licensing and contributing, &cet. But how much info? What exactly do I need to add?

I found [an excellent template][5] by [Billie Thompson][6] that fits the bill nicely. While I've commented out a bunch of stuff that isn't necessarily relevant (or maybe not relevant yet), I think it provides a pretty good overview of the what, who, why, and hows of the project. I'm relatively happy with the way the [README][7] turned out.

## With every change, log, log, log

Next up: everyone's favourite, the [CHANGELOG][8].

No, seriously. This either gets totally overlooked, or totally becomes a series of punchlines about episodes of Friends or whatever. In trying to figure out what needs to go into a change log, I discovered [Keep A CHANGLELOG][9]. Great resource.

As you go through merging in changes, part of the merge should including updating the change log with whatever you've, well, changed. As the project maintainers indicate, typical sections are:

> - `Added` for new features.
> - `Changed` for changes in existing functionality.
> - `Deprecated` for once-stable features removed in upcoming releases.
> - `Removed` for deprecated features removed in this release.
> - `Fixed` for any bug fixes.
> - `Security` to invite users to upgrade in case of vulnerabilities.

Additionally&mdash;I love this&mdash; every subtitle of the document should be a link to a `git compare` of the previous and current tags (or `HEAD`, if you're working on an unreleased version).

Excellent idea.

## License to thrive

[Jeff Atwood said it in 5 words][10]. Pick a license. This is important, or you're pretty much putting your work out there without letting anyone know if they can use it. Picking a license helps you both cut the overhead of having to reply to emails ("can I use this code?") while putting down some ground rules on how your work can be used.

I went with a simple [3-clause BSD license][11]. Essentially, this is a use-as-is-without-warranty license, requiring attribution by others who redistribute your work, and written permission if they want to use my name (or my organization's name) in any endorsement or promotion of their derived products.

## Just add contributors

The hardest to set up was the [CONTRIBUTING][12] document. Here, there are two main things I wanted to address:

1. Expectations of all involved (i.e., a code of conduct), and
2. Just _how_ to contribute.

The how-to-contribute part is pretty straightforward. Everything should start with an issue, and once labels and owners are assigned, work can begin. There's some basic info on how to report a bug vs. how to add a feature request, and I added some requirements for merge requests too. Such requirements can get pretty complex and involved, but it's important to step back and examine the scope of the project too. We're not building CocoaPods here; a tiny not-super-important project like HoneyJar probably won't attract much in the way of contributions, so there's no need for overly complex requirements for making changes.

That said, I believe that _any_ project, regardless of size, has the potential for community. And a community is only as good as the expectations for interaction. In my work, as in my life, I really try to come from a position of trust, mindfulness, and open-ness, so putting together a code of conduct that embraces these values is important to me.

Luckily, I'm not the only one that feels this way. The [Contributor Covenant][13] is a good place to start, and other open-source communities ([1][14], [2][15], [3][16], as examples) have done a great job of adapting it. I've followed in their footsteps, and I think the code of conduct section nicely explains the let's-be-wonderful-to-each-other feels that I want in my community.

## All the great fails

The last thing I absolutely wanted up and running for this first issue was to get continuous integration up and running.

"But Angelo, you don't have a test suite set up for your project."

Yeah, I know. But the [next big step][17] before I start working on the actual refactoring is _exactly that_: add a test suite for existing code. It would be downright silly to try refactoring something without making sure that you're not breaking anything as you go through.

So, I followed [the steps in the article I wrote][18] and created a new runner and a `.gitlab-ci.yml` file, along with some sample test, to make sure everything's working.

And builds kept getting stuck with the error:

```
Failed to authorize rights (0x1) with status: -60007.
```

Turns out, [I needed to update the runner on my build machine][19] to address some changes in Xcode 7.3&mdash;once that was done, things started building as expected. One of the great things about GitLab being an open-source project is that if you're running into trouble, there's likely an issue available to help you figure out what's happening and how to fix it (and if there isn't, open one!).

I've left the [build history][20] as-is, along with the [commit history][21], as breadcrumbs to how I managed to get things working, along with the stupid mistakes like overlooked typos in your runner tags. I don't think there's any value in trying to hide these goofs, because they could be invaluable the next time you're yelling at your system for not _just working come on you stupid thing everything was fine a minute ago what do you mean this tag doesn't exist_.

## Onwards and upwards

So, the project is now, IMO, officially open-sourced. The next big step is to add the test suite, and _then_ I can move on to the fun stuff: refactoring.

More tk.

[1]: /blog/2016/5/20/the-great-honeyjar-refactoring
[2]: https://gitlab.com/AngeloStavrow/HoneyJar/blob/4b5fa1653ac040ebe6f0896bd17b4ba463fc80aa/README.md
[3]: /blog/2016/5/13/on-comments
[4]: https://gitlab.com/AngeloStavrow/HoneyJar/issues/1
[5]: https://gist.github.com/PurpleBooth/109311bb0361f32d87a2
[6]: https://github.com/PurpleBooth
[7]: https://gitlab.com/AngeloStavrow/HoneyJar/blob/master/README.md
[8]: https://gitlab.com/AngeloStavrow/HoneyJar/blob/master/CHANGELOG.md
[9]: http://keepachangelog.com/
[10]: https://blog.codinghorror.com/pick-a-license-any-license/
[11]: https://gitlab.com/AngeloStavrow/HoneyJar/blob/master/LICENSE
[12]: https://gitlab.com/AngeloStavrow/HoneyJar/blob/master/CONTRIBUTING.md
[13]: http://contributor-covenant.org/version/1/4/code_of_conduct.md
[14]: https://thoughtbot.com/open-source-code-of-conduct
[15]: https://github.com/CocoaPods/CocoaPods/blob/master/CODE_OF_CONDUCT.md
[16]: https://swift.org/community/
[17]: https://gitlab.com/AngeloStavrow/HoneyJar/issues/2
[18]: https://about.gitlab.com/2016/03/10/setting-up-gitlab-ci-for-ios-projects/
[19]: https://gitlab.com/gitlab-org/gitlab-ci-multi-runner/issues/1143
[20]: https://gitlab.com/AngeloStavrow/HoneyJar/builds
[21]: https://gitlab.com/AngeloStavrow/HoneyJar/commits/9a3e8202b2dab26bf2ed45251918cfa5c844b0fc