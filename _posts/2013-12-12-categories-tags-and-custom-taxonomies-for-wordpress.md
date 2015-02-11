---
layout: post
title:  "Categories, Tags and Custom Taxonomies for WordPress"
date:   2013-12-12
categories: wordpress
---

Wordpress comes with 2 taxonomies built in: Categories and Tags, but you can add <em>Custom Taxonomies</em> if you like.  I was building WP sites for three years before I needed to set up a custom taxonomy for a client. 

There is a lot you can do with Categories and Tags. I've always told my clients "use as few categories as you need to organize your site, and as many tags as you like". That sums it up.  Categories are to divide your content into sections, tags are open ended, and can be used to add keywords to your posts.

If you want to sort posts by a particular criteria or value, by far the simplest way to do it is with a new taxonomy. For example, if you have a store, each product needs a SKU, then you make a taxonomy for it. And another one for price, etc. 

_edit: What was I thinking? a SKU and a price should be entered in custom fields_

[Fashioncow.com](http://fashioncow.com/) was the first site I added custom taxonomies to. To see Fashioncow's taxonomies on the front page, scroll down and look on the sidebar: you will see lists of photographers and models. Clicking a name will get you all posts featuring that photographer or model. When Tanya, owner of Fashioncow, wanted a list of photographers on her sidebar, I told her I would add a <em>photographer taxonomy</em>, and she would have to go through all her posts, and enter the photographer (or photographers), in the new meta-box that would appear on her edit-post screen.

Now that posts can have photographer's names attached to them, how do we get a list of photographers on the sidebar? This is easy, if your install Justin Tadlock's plugin [Widgets Reloaded](http://themehybrid.com/plugins/widgets-reloaded). This lets you put a tag cloud on your sidebar, but instead of listing tags, it will list any taxonomy you want.  And you can set the minimum and maximum font size for list items to the the same, if you don't want the jumbled "tag-cloud" look. That's how we got Fashioncow's photographer list.

Then we added taxonomies for Models, Stylists, and a combined Hair/Makeup taxonomy also.  I think these features add significant usefulness to her site.

One more thing: the custom taxonomy function should be in a plugin file, not simply added to the theme's functions.php file. The reason is simple: if you put the taxonomy into the theme, and you ever have to change or de-activate the theme, your custom taxonomy will disappear. That content would not be deleted of course, its all still in the database, but the Wordpress engine will have no knowledge of its existence.  All you need to do is put the taxonomy functions into a separate php file, and save it as a plugin, and activate it from the WP admin.