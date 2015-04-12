---
layout: post
title:  "Assigning templates to Categories"
date:   2015-4-4
categories: WordPress
---

WordPress has an odd way of handling category templates. You are supposed to create a file called **category-wildlife.php** for the 'wildlife' category.

While that is intuitive and simple, suppose you have 50 categories on your site, and you need 3 templates?  Suppose you three different templates and each one corresponds to a different layout.  You want to be able to edit a category, and assign it a template from your list of three choices. That's how page templates work.  You certainly don't want 50 template files for your 50 categories.

Here is the way to do it:

1.  Use [Advanced Custom Fields](https://wordpress.org/plugins/advanced-custom-fields/) to put a custom field named 'template' on your categories. I made radio button fields, with one button corresponding to each category template that I'm using: *standard*, *sorted* and *tabbed*. I set standard as a default field. 

1.  Create three category templates: category-standard.php, category-sorted.php, category-tabbed.php.

1.  Make a _template redirect_ to load the correct template depending on the value of the template field.  Paste it into your functions file.

{% highlight php %}
<?php function category_template_redirect() {
	if(is_category()) {
		$this_cat = get_the_category();
		$template_field = get_field('template', $this_cat[0]);
		if (have_posts()) {
			include(TEMPLATEPATH . '/category-' . $template_field . '.php');
			die();
		} else {
			$wp_query->is_404 = true;
		} 
	} 
}
add_action( 'template_redirect', 'category_template_redirect' );
?>
{% endhighlight %}
