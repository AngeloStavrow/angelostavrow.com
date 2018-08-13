+++
date = "2016-04-29T12:00:00-04:00"
draft = false
title = "On interruption-driven development"
aliases = [ "/blog/2016/4/29/on-interruption-driven-development" ]

+++

As the sole developer at work, I have to balance our “ask me anything, anytime” culture with a need for unbroken concentration on whatever code I'm writing.

I'm generally fine with a door-always-open policy like this, because it makes communication fast and easy&mdash;especially in a small team, where everyone wears several hats&mdash;but as projects get closer to a deadline, it does tend to put a bit of strain on my ability to get things done. When that project involves a relatively complex application, however, it can be very hard to keep things on schedule, because [task switching has such a high cost](http://www.joelonsoftware.com/articles/fog0000000022.html):

>That's because programming is the kind of task where you have to keep a lot of things in your head at once. The more things you remember at once, the more productive you are at programming.

I've been struggling with this a little bit lately. It's no one's fault, of course&mdash;colleagues are just interacting with each other the way they always have.

So far, I've been trying some of the more common workarounds to this kind of workflow:

- coming in to work two hours earlier than anyone else to focus on trickier problems
- working from home<sup id="fnref:1"><a href="#fn:1" rel="footnote">1</a></sup>
- declining meeting requests that I don't absolutely need to join
- delegating other tasks to other colleagues
- using noise-attenuating headphones when I need peak focus

It's helping somewhat, but what I've noticed as the project approaches release-candidate is a little more interesting.

**The more loosely-coupled the code is, the easier it is to come back to it after an interruption.** Instead of having to keep all kinds of interaction in my head, I love small, simple classes that have no knowledge of their context. They do one or two related things, and that's _it_. Jumping in and out of these is relatively easy.

**A good debugger and a test suite are indispensible.** Interrupted tasks are [twice as likely to contain errors](http://blog.ninlabs.com/2013/01/programmer-interrupted/), so a thorough test suite&mdash;and really writing tests before code, as TDD requires&mdash;helps catch these. Some parts of my project include firmware that's written without the benefits of test frameworks or breakpoints, so I have to resort to [caveman debugging](http://www.ecnmag.com/blog/2015/06/debugging-caveman). This is something I'm going to avoid in future projects.

**Refactor your comments with your code.** You should never trust your comments to accurately reflect what your code does unless you consider them to be inseparable from the code, and change them while you make changes to your code. When you're interrupted, ask for a minute to take a note on what you're doing and add `TODO:`, `FIXME:`, or `HAX:` comments that you can come back to.

These things do way more for my ability to mitigate how badly an interruption affects my productivity than headphones, working from home, or refusing meetings, _and_ they make my code better to boot.

What works best for you?

<div class="footnotes">
  <ol>
  	<li class="footnote" id="fn:1">
  		<p>This is difficult, because the software I’m working on has to control some permanently-installed hardware in our lab.<a href="#fnref:1" title="return to article"> ↩</a></p>
	</li>
  </ol>
</div>