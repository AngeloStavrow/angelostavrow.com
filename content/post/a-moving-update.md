+++
date = "2016-06-10T12:00:00-04:00"
draft = false
title = "A moving update"
aliases = [ "/blog/2016/6/10/a-moving-update" ]

+++

I'm in the process of (finally) finalizing the move of this site to [Hugo][1].

No, for real.

Most content is already in place. A few older posts need to be added, and some info pages are to be added. This is normal-priority.

I'm using a slightly-modified built-in template to render the site. There a few more modifications to make, but this is low-priority.

A commenting platform will be added. This is also normal-priority.

I want to explore adding SSL to the site, but this is low-priority.

Finally, a deploy strategy is to be set up&mdash;this is high-priority. The plan right now is to have the following workflow:

1. Merge in some changes (i.e. New post, updating old content, &cet.);
2. Merge commit triggers CI to build the site;
3. When CI succeeds, deploy the built site to the web host.

This will give me the opportunity to post from either my computer or my phone without issue. Then it's just a question of reconfiguring DNS to serve up the new site.

If all goes well, next week's post should be on the new platform!

[1]: https://gohugo.io
