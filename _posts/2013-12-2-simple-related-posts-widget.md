---
layout: post
title:  "A Simple Related Posts Widget"
date:   2013-12-2
categories: wordpress
---

If you want to put some related posts in a widget you can use the smart <a href="http://wordpress.org/plugins/yet-another-related-posts-plugin/">YARPP</a> plugin. 

Or, if you merely want to list posts with matching tags, you can put a function in your Wordpress theme. In some ways this is better than the fuzzy logic of YARPP because it gives you more control: you just give posts the same tag, and you know they are related. Plus, its easy to edit such simple code to suit (if you know how <a href="http://codex.wordpress.org/The_Loop">the loop</a> works). 

Try this to start:
{% highlight html %}
// This function will list three posts in the sidebar with like tags.
// Activated by the shortcode &#91;related-posts]
function related_posts_widget() {
	ob_start();
	global $post;
	$tags = wp_get_post_tags( $post->ID );
	foreach ( $tags as $tag ) {
		$tagID[] = $tag->term_id;
	}
	if ($tagID) {
		$args = array(
			'tag__in'   => $tagID,
			'post__not_in' => array($post->ID),
			'showposts'  => 3
		);
		$related_query = new WP_Query($args);
		if( $related_query -> have_posts() ) { ?>
		&lt;ul>
		&lt;?php while ($related_query -> have_posts()) : $related_query -> the_post(); ?>
		&lt;li>
		&lt;a href="<?php the_permalink(); ?>" title="&lt;?php the_title_attribute(); ?>" rel="bookmark">&lt;?php the_title(); ?>&lt;/a>
		&lt;/li>
		&lt;?php endwhile; ?>
		&lt;/ul>
		&lt;?php }
		wp_reset_query();
	}
	$output = ob_get_contents();
	ob_end_clean();
	return $output;
}
add_shortcode('related-posts','related_posts_widget');</pre>
Also, for this to work you need to be able to put shortcodes in widgets, which does not work with the default WP installation. To enable shortcodes in widgets add this to your functions.php:
<pre> add_filter('widget_text', 'do_shortcode');
{% endhighlight %}