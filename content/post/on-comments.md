+++
date = "2016-05-13T12:00:00-04:00"
draft = false
title = "On comments"
aliases = [ "/blog/2016/5/13/on-comments" ]

+++

I [mentioned][1] that keeping comments up-to-date with modifications in source code helps me with task-switching in a busy workplace.

Of course, that doesn't make a difference if you're not commenting your code _well_.

Therein lies the rub. I spent weeks on learning various sorting algorithms in my data structures and algorithms class, but I never really learned anything on the whys and wherefores of commenting.

I've been taught that 

```
// This is an inline comment.
```

and that

```
/* This is a block comment, which makes
 * longer comments a little bit more
 * readable.
 */
```
 
and also how to use comments to temporarily disable portions of your source code to try something out.

And that's fine.

But what should you put _in_ your comments?

Visual Studio has a neat feature where if you type three slashes in a row (`///`) before a method definition, it'll expand a comment snippet that includes, as appropriate, sections for a _summary_, what method argument _parameters_ are, and what the method _returns_. Whatever you enter there shows up in a tool tip when you hover over the method call later.

Similarly, three slashes (`///`) in Xcode will allow you to enter comments that show up when you hover over method calls.<sup id="fnref:1"><a href="#fn:1" rel="footnote">1</a></sup>

Handy, for sure.

But, really, when I'm heads down and trying to push code out before, say, I leave for the day, I've noticed that all I write in these comments is a re-iteration of the method signature&mdash;and given that I try to use very descriptive method and variable names in the first place, these extra lines of typing are pretty redundant. I'm just giving myself work for zero added benefit.

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">My point here is that if you have trouble expressing basic intent of code then there are bigger issues with it <a href="https://t.co/YcyuvQQxSH">https://t.co/YcyuvQQxSH</a></p>&mdash; Mx.Samantha (@queersorceress) <a href="https://twitter.com/queersorceress/status/730484670027186176">May 11, 2016</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Samantha's tweet inspired me to think a bit more about how I comment my code. Intent is key, I agree. If the source code is the documentation<sup id="fnref:2"><a href="#fn:2" rel="footnote">2</a></sup>, then the comments should probably play the role of both the chapter summaries, _and_ the sidebars that warn of the gotchas, the perils, and the pitfalls. The bigger-picture stuff. _Why_ is this structured this way? _What_ might I have to look out for when testing this particular method?

All the best practices we learn&mdash;design patterns, unit testing, &cet.&mdash;are meant to make our code more maintainable. Comments should be an integral part of that, not just as a repetition of what's obvious from the source, but as a reflection of the intent of the thing.

[1]: (http://www.makebeforebreak.com/blog/2016/4/29/on-interruption-driven-development)

<div class="footnotes">
  <ol>
  <li class="footnote" id="fn:1">
  <p>You definitely should pick up Erica Sadun's <a href="https://leanpub.com/swiftdocumentationmarkup">Swift Documentation Markup</a> to really make the most of this feature, by the way.<a href="#fnref:1" title="return to article"> ↩</a></p>
</li>
  	<li class="footnote" id="fn:2">
  	<p>I'm not 100% convinced of this particular mantra. Of course, code is the canonical source of ground truth, and that's the intent, but as far as it being documentation&mdash;well, that's a post for another day.<a href="#fnref:1" title="return to article"> ↩</a>
  	</p>
	</li>
  </ol>
</div>
