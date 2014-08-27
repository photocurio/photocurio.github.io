---
layout: post
title:  "WordCamp Boston"
date:   2014-08-26
categories: WordPress JavaScript
---
I went to [WordCamp Boston](http://2014.boston.wordcamp.org/) last weekend. It was basically one-day of presentations, so it was a small WordCamp. My take-away was very different from what I expected: the future of WordPress (and of the web) is javascript. 

The second presentation I attended was on the history of APIs, and the last part of the talk devoted to a new [JSON API](http://wordpress.org/plugins/json-rest-api/) for WordPress.  JSON stands for JavaScript Object Notation btw. This is available now as a plugin, but is scheduled for intergration into the WP core in version 4.2. The whole thing was an eye-opener for me, since I had not considered what the implications could be of millions of WordPress sites around the globe suddenly being able to communicate with each other, and to other apps, much faster than at present. I was left baffled, musing "What can I do with this new thing?"

In the afternoon I attended a second presentation on the same plugin, this time by [Taylor Lovett](http://taylorlovett.com/), one of the developers of the API. So, now I'm getting the idea that this JSON API could be called the _buzz_ of WordCamp Boston.  I'm also thinking I need to up my javascript chops.  Actually, I'd been thinking that already, having had so much fun with jQuery on the last site ([LookBetterFeelBetter](http://www.lookbetterfeelbetter.com/)) I coded.  

It was not until the after party, taking about said API with some other devs, that I finally understood what I could do with it: _I don't need a theme any more_. If I make an app that loads posts and pages directly from the database through the API&mdash;I don't need all that php.

I don't yet know how I should build an app.  Maybe a javascript framework like backbone.js, or angularJS.  I have to find the simplest, clearest javascript framework.