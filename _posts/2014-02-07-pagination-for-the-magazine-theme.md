---
layout: post
title:  "Pagination for the Magazine Theme"
date:   2014-02-07
categories: WordPress
---

Maybe you have a magazine-style WordPress theme with a slider at the top of the front page. The issue of pagination comes up when the user hits the Older Posts link.

Say you have a front page that displays 3 posts in the slider. The slider is a loop. Then a default loop that shows your most recent posts. And the number of posts is determined by the setting in your Settings -> Reading -> Blog pages show at most X posts. 

First of all, you probably don't want to load the same template for your back pages. You probably don't want the slider on the back pages. By default, clicking the pagination link will load the same template, but give the default loop and 'offset' equal to number of posts in the _Blog pages show at most_ setting. If you want second, and subsequent pages to skip the slider, and other front page stuff, and just show a list of posts, then you need a redirect to a different template.   

This is the fix: duplicate index.php, and name it backpages.php.  

Second, you need a redirect to backpages.php. It goes in the functions.php file. 

{% highlight php %}
<?php
function our_template_redirect() {
    if ( (is_home()) && ( is_paged('2') ) ):
        include (get_stylesheet_directory() . '/backpages.php');
        exit;
    endif;
}
add_action( 'template_redirect', 'our_template_redirect' );
?>
{% endhighlight %}

Next we need to fix the pagination on backpages.php. Use this code before the default loop on backpages.php, which re-calculates the offset value to account for the number of posts in the slider:

{% highlight php %}
<?php			
	// We need an offset to account for the posts in the slider
	$offset = 3;
	
	// Next, determine how many posts per page you want, using WordPress's settings
	$ppp = get_option('posts_per_page');
	
	// What page are we on?
	$paged = get_query_var( 'paged' );
	
	// Manually determine page query offset: (offset + (current page minus one x posts per page)
	$page_offset = $offset + ( ($paged -1 ) * $ppp );
	
	// Define the query
	query_posts( 'offset=' . $page_offset );

	// Start the default loop
	if (have_posts()) : while (have_posts()) : the_post(); 
?>
{% endhighlight %}
