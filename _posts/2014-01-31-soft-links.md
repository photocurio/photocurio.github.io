---
layout: post
title:  "Soft link effects"
date:   2014-01-31
categories: CSS3
---

This is a simple way to make a site more like a smooth web-app: give a half-second transition property to all links.

{% highlight php %}
a, a strong, a em { 
	color: #B00; 
	text-decoration: none; 
	-webkit-transition: all 0.5s ease; 
	-moz-transition: all 0.5s ease; 
	-o-transition: all 0.5s ease; 
	transition: all 0.5s ease; 
}
{% endhighlight %}
