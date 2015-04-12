---
layout: post
title:  "Assigning templates to Categories"
date:   2015-4-4
categories: WordPress
---

I discovered the plugin [Advanced Custom Fields](https://wordpress.org/plugins/advanced-custom-fields/) last summer.  This is such a powerful plugin. I immediately bought the [Repeater Field](http://www.advancedcustomfields.com/add-ons/repeater-field/) addon. With repeater fields you can make rows of the same type of field.  _Loops_ of fields.

An example of a nice way to use this plugin combo would be a page of customer testimonials on a website. In the past I would have made a Custom Post Type called Testimonial, and made a page to list all the testimonials, with an intro and a `WP_query` loop. But using Repeater Fields is much nicer; its simpler to code and has easier content management.

To start, make a page in your WordPress site called Testimonials, and write an intro.

Now you need to have the two plugins mentioned above installed.  Go to Custom Fields in your Admin area, and make a new Field Group. Call it Testimonial Fields, and under field type choose _repeater_.

You can add any number of sub-fields to a repeater field. It depends on your design.  I used 4 sub-fields in a recent site: pullquote, testimonial text, attribution, and video link. Note that you can label the fields helpfully, and drag them to re-sequence them!  These are the touches that make this plugin so awesome.

There is metabox called Location where you can specify exactly where you want these fields to show up. So set your Testimonial Fields to display on the Testimonials page.

Then you need to make a testimonials template in your theme.  If you duplicate your page.php file and rename it page-testimonials.php that will work.

The code that loops through the Testimonials is very simple. Paste this into your new template file, somewhere below `the_content()`:

{% highlight php %}
<?php if( have_rows('testimonial_fields') ): ?>
	<ul class="testimonial-list">
		<?php while( have_rows('testimonial_fields') ): the_row(); ?>
			<li>
				<div>
					<h2><?php the_sub_field('pullquote'); ?></h2>
					<div><?php echo wp_oembed_get(get_sub_field('video_link')); ?></div>
					<p ><?php the_sub_field('testimonial_text'); ?></p>
					<p><?php the_sub_field('attribution'); ?></p>
				</div>
			</li>
		<?php endwhile; ?>
	</ul>
<?php endif; ?>
{% endhighlight %}


This setup is so much better than using a custom post type! Unless every testimonial needs its own permalink that is. Testimonials can all be edited on one page, and re-sequenced by drag and drop.

##Now add a little jQuery

Another lovely use for Repeater Fields is making client-editable accordions.  I generally make custom post types for staff pages on business sites&mdash;because each staff member need their own URL. And often these staff members have 2 thousand word resumes.  When posting very long content online, an accordion is helpful to show and hide sections one at a time. See this demo [at a law firm](http://www.benachragland.com/team/jennifer-d-cook/).  

This is how I do it: make a Repeater Field called Resume.  It has two sub-fields: Resume Heading (`resume_heading`) and Resume Content (`resume_content`).  

Use this code to display the list of headings. The first list item is a link to the Bio, which is not in a custom field; its simply the regular content section of the post. The resume section will hide it when they display.

We can make the anchor links from the `resume_heading` field by passing the string through the WordPress sanitizing function `sanitize_title`.
{% highlight php %}
<?php if( have_rows('resume') ): ?>
	<ul id="accordion"> 
	    <li><a href="#biography" class="active">biography</a></li>
	<?php while(have_rows('resume')) : the_row(); ?>
	    <li>
	    <a href="#<?php echo sanitize_title(get_sub_field('resume_heading')); ?>" class="ready"><?php the_sub_field('resume_heading'); ?></a></li> 
	<?php endwhile; ?>
	</ul>
<?php endif; ?>
{% endhighlight %}

This code displays the content. 

{% highlight php %}
<div class="entry-content">			
	<div id="biography" class="resume-content showing">
		<?php the_content(); ?>
	</div>
	<?php while( have_rows('resume') ): the_row(); ?>
    	<div id="<?php echo sanitize_title(get_sub_field('resume_content')); ?>" class="resume-content" >
        	<h2><?php the_sub_field('resume_heading'); ?></h2>
        	<?php the_sub_field('resume_content'); ?>
        </div> 
	<?php endwhile; ?>	
</div>
{% endhighlight %}

It needs CSS to initially hide the resume:

{% highlight css %}
.resume-content { display:none; }
.resume-content.showing { display:block; }
{% endhighlight %}

And some sweet jQuery to make it work:

{% highlight js %}
jQuery(document).ready(function($) {
	$('#accordion li a').click(function(e) {
		var accordionid = $(this).attr("href");
		$('.resume-content').not(accordionid).slideUp();
		$(accordionid).slideDown();
		e.preventDefault();
	});
});
{% endhighlight %}

