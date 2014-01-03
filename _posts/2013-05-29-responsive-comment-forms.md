---
layout: post
title:  "Responsive Comment Forms"
date:   2013-05-29
categories: wordpress
---

After you make the front page of your theme responsive (see my [previous post](href="http://topheavypilesofbooks.com/2012/10/coding-a-responsive-site/) ), you need to check the other templates. And one area that might break your theme, and resists the simple CSS-media-queries method, is<strong> the comments form,</strong> typically loaded on the single post template. 

That's because forms tend to have fixed widths, and whats more, the width is defined with inline HTML, or at least its defined that way by default in the Wordpress comments template. The template outputs the following HTML for the Name field:
{% highlight html %}
<input id="author" type="text" aria-required="true" size="30" value="" name="author">
{% endhighlight %}
The `size="30"` is what controls the width. I don't know what units this is in.. thirty characters maybe? In any case this is impervious to CSS. There is a way to make this responsive however, and it goes back to the template hook that loads the comment forms: `comment_form();`.  It turns out that you can override the default options quite easily (see the Wordpress <a href="http://codex.wordpress.org/Function_Reference/comment_form">codex</a>). What I do is use an `if (is_mobile())` statement to load a smart phone size comment form, and an _else_ statement to load the non-mobile, default comment form. The code is rather long, but here it is:
{% highlight php %}
<?php
if (is_mobile()) {
	$args = array(
		'comment_field' => '<p class="comment-form-comment"><label for="comment">' . _x( 'Comment', 'noun' ) . '</label><textarea id="comment" name="comment" cols="28" rows="8" aria-required="true"></textarea></p>',
		'fields' => apply_filters( 'comment_form_default_fields', array(
			'author' => '<p class="comment-form-author">' . '<label for="author">' . __( 'Name', 'domainreference' ) . '</label> ' . ( $req ? '<span class="required">*</span>' : '' ) . '<input id="author" name="author" type="text" value="' . esc_attr( $commenter['comment_author'] ) . '" size="27"' . $aria_req . ' /></p>',
			'email' => '<p class="comment-form-email"><label for="email">' . __( 'Email', 'domainreference' ) . '</label> ' . ( $req ? '<span class="required">*</span>' : '' ) . '<input id="email" name="email" type="text" value="' . esc_attr( $commenter['comment_author_email'] ) . '" size="27"' . $aria_req . ' /></p>',
			'url' => '<p class="comment-form-url"><label for="url">' . __( 'Website', 'domainreference' ) . '</label>' . '<input id="url" name="url" type="text" value="' . esc_attr( $commenter['comment_author_url'] ) . '" size="27" /></p>' 
		) ) 
	);
} else {
	$args = '';
}
comment_form($args); 
?>
{% endhighlight %}

Just replace the `comment_form();` hook in your theme with the above code. And BTW, it won't work unless you have the __Mobile Client Detection Plugin__ installed. That's what lets you use the `is_mobile()` conditional.

What this code is doing is defining smaller textarea fields if the user agent is 'mobile'. See the `size="27"` parts? that's the key. And for the big textarea, I use cols="28". These numbers may be "magic numbers", i.e. they just happen to fit my theme with the font I use, but its no worse than the bad magic numbers that were in there before.