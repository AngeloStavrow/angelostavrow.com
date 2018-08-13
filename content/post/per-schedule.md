+++
date = "2016-09-26T12:00:00-04:00"
draft = false
title = "Thoughts on Per"

+++

I haven't posted here in a little while, thus [breaking the streak](/post/on-streaks/) that I was intending to maintain. I don't feel too bad about this, because I've been working on other stuff in the meanwhile, but it's nice to be writing again.

Specifically, in light of the coming [App Store purge](https://developer.apple.com/support/app-store-improvements/), I've been working on a big update to [Per](http://droppedbits.com/per). While I'm not especially concerned that Per, in its current form, is at risk of being culled, it _has_ been a long time since it was updated.

So.

For one, I'm dropping support for anything prior to iOS 10. Per has a pretty tiny user base&mdash;maybe a couple hundred downloads&mdash;and I don't see much in the way of daily active users<sup id="fnref:1"><a href="#fn:1" rel="footnote">1</a></sup>, but for such a tiny user base, I can't justify putting effort into supporting iOS 8 and 9.

Second on the list is a pretty hefty redesign. I fully admit that Per is… kinda homely-looking. There are some interactions that I'm planning on (and have started experimenting with) for 2.0, and while it's slow going, I like what I see so far.

I'm also thinking about taking advantage of some new input methods, but I don't know yet how well that will work. I don't want to tip my hand just yet because those may not be technically feasible, but it's got me pretty interested in seeing what can be done<sup id="fnref:2"><a href="#fn:2" rel="footnote">2</a></sup>.

Third on the list is the business model for Per.

Per brings in pretty much zero revenue. Most of the downloads came from a period of time when I made my apps free for a week, whereas the normally-paid (at USD$1.99) version has seen only a handful of downloads at most.

Note that I haven't marketed Per at all, save for a couple of tweets and blog posts, so this isn't unexpected. But I think it's time to change that.

Given the way the App Store economy works, it's pretty clear that charging up-front is a barrier to getting your app downloaded. This is no secret, so some business model that includes free downloads makes sense.

One option is to make Per free, with an in-app purchase (IAP) to unlock advanced features, like the unit conversion and  in-field math operations it already features, and the new input methods mentioned. This means zero revenue unless people want these features. How desirable are they in a relatively simple utility app like Per?

Another option is to make Per free, but ad-supported. Thing is, I personally don't like the aesthetic that ads introduce in an app's interface and experience, and I know I'm not the only one. So there would definitely have to be an IAP to remove ads. I have a couple of ideas on where and when I'd show an ad, so they wouldn't be _too_ annoying.

Both cases mean that current paying users would experience either reduced functionality, or an ad-filled experience. That sucks, so yet _another_ option would be to create a new SKU. Folks that have the original version of Per get a fully-featured and ad-free upgrade, and new users could opt to pay for this version outright, whereas the other version of the app would implement one of the above options, but would list for free.

That's a fair bit of extra administrative work for me, and confusing for both old and new users, so I don't think I'll do that.

I could also leave Per as-is and create a new SKU for Per 2. No further updates for the old app, but then that also probably means it'll eventually be culled from the App Store anyhow.

Given that something like 3 users have actually _paid_ for Per (thanks!), I think that I'll skip the two-SKU options altogether, because they're administrative headaches. This means that Per 2 will be a free upgrade, but will either be removing some features or including ads (sorry!).

Maybe there's a way to check whether a user paid for an app or not, I'm not sure. [Let me know](mailto:contact@angelostavrow.com)!

As I mentioned, I know that Per is really just a simple utility app, but it's something that my wife and I use pretty much every time we shop for groceries. It's certainly _useful_, but I also want it to be _great_. That not only means redesigns and refactoring, but it also means getting it in the hands of as many people as possible.

More tk.

<hr>

<div class="footnotes">
  <ol>
  	<li class="footnote" id="fn:1">
  		<p>I know that this could be because most people disable sharing this data when they set up their phone.<a href="#fnref:1" title="return to article"> ↩</a></p>
	</li>
	<li class="footnote" id="fn:2">
  		<p>Seems to me that, for some developers, adding a new feature to an older app has less to do with a revenue bump than it does the excitement of tackling a new problem. That's how I feel, anyhow.<a href="#fnref:2" title="return to article"> ↩</a></p>
	</li>
  </ol>
</div>
