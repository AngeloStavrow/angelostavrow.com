+++
date = "2016-06-03T12:00:00-04:00"
draft = false
title = "TGHR: Testing the units"
tags = [ "tghr" ]
aliases = [ "/blog/2016/6/3/thgr-testing-the-units" ]

+++

<blockquote><strong>The Great HoneyJar Refactoring</strong> is a series of posts in which I take the first iOS app I ever wrote, <a href="http://droppedbits.com/honeyjar/">HoneyJar</a>, and refactor it out of its original burning-dumpster-fire state and into a modern app. And I'm doing it <a href="https://gitlab.com/AngeloStavrow/HoneyJar">in public</a>.</blockquote>

Earlier this week, [I tweeted][1] about my adventures in trying to add a test suite to HoneyJar.

The idea is this: I want to be sure that I'm not breaking anything in the app, as it exists right now, when I start refactoring. Without an existing test suite, I have no way of knowing if I'm creating any regressions.

The TDD approach is to

1. Write a test that checks a particular public method against a particular condition;
2. Write the code that makes the test pass; and
3. Repeat steps 1 and 2 until you're done writing the method / class / app.

The tests come first, and the code "fills in the blanks".

When you've got a legacy codebase<sup id="fnref:1"><a href="#fn:1" rel="footnote">1</a></sup>, however, does it still make sense to write failing tests first? Probably not.

Furthermore, what if it's just not _possible_ to test certain methods or classes, because of the way the code is structured? If you haven't written the code with testing in mind, then it might be too coupled or complex to test after the fact.

> Stop writing legacy code. Do not write new features without unit testing. You are just making the code base worse and getting further away from introducing tests into your system. Get your testing infrastructure set up and a few tests running successfully and it will be much easier to think about introducing tests for the rest of the code.
> 
> &mdash; Dan Lee, [_TDD when up to your neck in legacy code_][2]

Right now, what's making me a little bit crazy is that I'll open a class to start adding some unit tests, see all kinds of ways to improve the code before adding the tests, and have to remind myself that I've made a promise not to touch the code until the test suite is in place.

SO. FRUSTRATING.

It's _especially_ frustrating when you realize that you'd written something that you can't really test, but you're not allowing yourself to fix it.

But we have a great way of getting around that. [Comments][3].

So, here's what I'm doing:

1. Open the class to which I want to add tests;
2. Add a new test case class for said tests;
3. Scan through the class under test and grind my teeth at obvious problems;
4. Take three deep breaths;
5. Start adding whatever tests that I can add without changing the code; and
6. Add thorough `//TODO` comments on "next steps" for refactoring this class&mdash;obvious problems, how to improve testability, &cet.

I'm going to go through doing this for the entire codebase first. I _could_ add some tests for a class, then refactor it, then move on to the next one, but then I have no way of knowing if I'm making a change that will somehow propagate through the app and trigger a failure elsewhere. So, while I'm not especially happy with the coverage I'm getting right now, it's better this way.

Once [the issue is closed][4], I'll start tackling the actual refactoring&mdash;and I'll feel more like the project is ready for others to work on, too. For me, at least, I t's really hard to make changes to a codebase I'm unfamiliar with if I don't have the safety net of a test suite.

As always, more tk.


[1]: https://twitter.com/AngeloStavrow/status/738053482490527744
[2]: https://danlimerick.wordpress.com/2012/04/25/tdd-when-up-to-your-neck-in-legacy-code/
[3]: /blog/2016/5/13/on-comments
[4]: https://gitlab.com/AngeloStavrow/HoneyJar/issues/2

<div class="footnotes">
  <ol>
  	<li class="footnote" id="fn:1">
  <p>By "legacy codebase", here, we mean untested code, i.e., a codebase with no pre-existing test suite.<a href="#fnref:1" title="return to article"> ↩</a></p>
</li>
  </ol>
</div>
