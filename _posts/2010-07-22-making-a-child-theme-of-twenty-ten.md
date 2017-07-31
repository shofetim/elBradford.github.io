---
id: 1134
title: Making A Child Theme of Twenty Ten
author: bradford
layout: post
guid: http://theblawblog.com/?p=1134
permalink: /2010/making-a-child-theme-of-twenty-ten
categories:
  - Technology
  - Web Design
---
I realized after a recent upgrade to WordPress 3.0 and after [changing my blog&#8217;s address][1] that I was tired of my old blog&#8217;s look. I was really impressed with the new default theme for 3.0:<a href="http://2010dev.wordpress.com/" target="_blank"> Twenty Ten</a>. It&#8217;s simple, aesthetically pleasing, and supports all the new features of 3.0.

Since my new job requires me to learn a lot of PHP and HTML I&#8217;ve used some of my new skills in developing a child theme for Twenty Ten. Child themes are great! They allow you to basically extend and customize an existing theme in a way that won&#8217;t break if the theme gets an update.

<!--more-->

The notable changes I made to Twenty Ten include adding the Twenty Ten Header Rotator plugin, using custom images for the header, adding some Twitter/RSS buttons at the top, adding a snazzy search bar and putting my contemplative face in the header.

To use my own images for the Twenty Ten header I borrowed some code provided by <a href="http://www.akademy.co.uk/" target="_blank">Matthew</a> on the <a href="http://hungrycoder.xenexbd.com/scripts/header-image-rotator-for-twenty-ten-theme-of-wordpress-3-0.html" target="_blank">Twenty Ten Header Rotator plugin site</a>. I replaced the following code in functions.php

<pre class="brush:php">register_default_headers( array(
	'berries' =&gt; array(
		'url' =&gt; '%s/images/headers/berries.jpg',
		'thumbnail_url' =&gt; '%s/images/headers/berries-thumbnail.jpg',
		/* translators: header image description */
		'description' =&gt; __( 'Berries', 'twentyten' )
	),
	'cherryblossom' =&gt; array(
		'url' =&gt; '%s/images/headers/cherryblossoms.jpg',
		'thumbnail_url' =&gt; '%s/images/headers/cherryblossoms-thumbnail.jpg',
		/* translators: header image description */
		'description' =&gt; __( 'Cherry Blossoms', 'twentyten' )
	),
	'concave' =&gt; array(
		'url' =&gt; '%s/images/headers/concave.jpg',
		'thumbnail_url' =&gt; '%s/images/headers/concave-thumbnail.jpg',
		/* translators: header image description */
		'description' =&gt; __( 'Concave', 'twentyten' )
	),
	'fern' =&gt; array(
		'url' =&gt; '%s/images/headers/fern.jpg',
		'thumbnail_url' =&gt; '%s/images/headers/fern-thumbnail.jpg',
		/* translators: header image description */
		'description' =&gt; __( 'Fern', 'twentyten' )
	),
	'forestfloor' =&gt; array(
		'url' =&gt; '%s/images/headers/forestfloor.jpg',
		'thumbnail_url' =&gt; '%s/images/headers/forestfloor-thumbnail.jpg',
		/* translators: header image description */
		'description' =&gt; __( 'Forest Floor', 'twentyten' )
	),
	'inkwell' =&gt; array(
		'url' =&gt; '%s/images/headers/inkwell.jpg',
		'thumbnail_url' =&gt; '%s/images/headers/inkwell-thumbnail.jpg',
		/* translators: header image description */
		'description' =&gt; __( 'Inkwell', 'twentyten' )
	),
	'path' =&gt; array(
		'url' =&gt; '%s/images/headers/path.jpg',
		'thumbnail_url' =&gt; '%s/images/headers/path-thumbnail.jpg',
		/* translators: header image description */
		'description' =&gt; __( 'Path', 'twentyten' )
	),
	'sunset' =&gt; array(
		'url' =&gt; '%s/images/headers/sunset.jpg',
		'thumbnail_url' =&gt; '%s/images/headers/sunset-thumbnail.jpg',
		/* translators: header image description */
		'description' =&gt; __( 'Sunset', 'twentyten' )
	)
));</pre>

which is a static array, with this while loop:

<pre class="brush:php">if ($handle = opendir( STYLESHEETPATH. '/images/headers/') )
{
	$headers = array();

	while (false !== ($file = readdir($handle)))
	{
		$pos = strrpos( $file, '.' );

		if( $pos !== false && $pos &gt; 0 )
		{
			$file_name = substr( $file, 0, $pos );

				if( strpos( $file_name, "-thumbnail" ) === false )
				{
					$file_ext = substr( $file, $pos+1 );
					$file_ext_low = strtolower( $file_ext );

					if( $file_ext_low == "jpg" || $file_ext_low == "jpeg" )
					{
						$headers[$file_name] = array ('url' =&gt; '%s_enhanced/images/headers/' . $file,
													'thumbnail_url' =&gt; '%s_enhanced/images/headers/' . $file_name . "-thumbnail." . $file_ext,
													'description' =&gt; __( str_replace( "_", " ", $file_name ), 'twentyten' ));
					}
				}
		}
	}
	closedir($handle);
	register_default_headers( $headers );
}</pre>

Notice on line 16 and 17 I had to append &#8220;\_enhanced&#8221; to the $s variable in order to get it to work since I am storing the images in my child theme&#8217;s images folder instead of the parent directory. $s appears to refer to the theme directory url and in my case adding &#8220;\_enhanced&#8221; works since my theme is in the twentyten_enhanced folder. Additionally I used STYLESHEETPATH instead of TEMPLATEPATH. STYLESHEETPATH refers to the child theme&#8217;s directory while TEMPLATEPATH refers to the parent theme directory.

In order to add the Twitter/RSS buttons in the header I added the following div right below the div with id &#8220;site-description&#8221; in the header.php file:

<pre class="brush:php">&lt;div id="twitter-rss"&gt;
	&lt;a href="rss"&gt;&lt;img alt="Subscribe to RSS" src="http://theblawblog.com/wp-content/icons/rss.png" /&gt;&lt;/a&gt;
	&lt;a href="http://twitter.com/bradford"&gt;&lt;img alt="Follow me on twitter!" src="http://theblawblog.com/wp-content/icons/twitter.png" /&gt;&lt;/a&gt;
&lt;/div&gt;</pre>

That won&#8217;t look pretty without some css to go along with it so I added these css rules to style.css:

<pre class="brush:css">#twitter-rss {
	float: left;
}
#branding img.twitter-rss {
	border-top: 0px;
	border-bottom: 0px;
	display: inline;
}</pre>

I also added the following style rules to the site description and site title to position it around the image of my thoughtful visage and include a hidden link in the site description:

<pre class="brush:css">#site-title {
	margin: 10px 0 18px 110px;
	width: 462px;
}
#site-description {
	float: left;
	margin: 25px 10px 18px 0;
	text-align:right;
}
#branding a.no_u {
	text-decoration: none;
}
#branding a.no_u:link {
	color: #666;
}
#branding a.no_u:visited {
	color: #666;
}</pre>

The last 3 groups of code refer to a class called &#8220;no\_u&#8221; so when I made the link in my site description I had to make sure to reference the class &#8220;no\_u&#8221; like this:

<pre class="brush:xml">&lt;div id="site-description"&gt;
	&lt;a class="no_u" href="http://en.wikipedia.org/wiki/Prayer_of_Saint_Francis#Prayer"&gt;&lt;?php bloginfo( 'description' ); ?&gt;&lt;/a&gt;
&lt;/div&gt;</pre>

Next I included a snazzy hidden search box as well. This was a little more involved but still quite simple if you understand a few basics of PHP. First, I added the search form element in the header.php file right below the php line that gets the menu array, I&#8217;ll just include it as a point of reference:

<pre class="brush:php">&lt;?php wp_nav_menu( array( 'container_class' =&gt; 'menu-header', 'theme_location' =&gt; 'primary' ) ); ?&gt;
&lt;?php $search_text = ($_REQUEST["s"]=='')?"...search":'Search for: '.$_REQUEST["s"];?&gt;
&lt;div class="search-form"&gt;
	&lt;form method="get" id="searchform" action="&lt;?php bloginfo('home'); ?&gt;/"&gt;
		&lt;input type="text" value="&lt;?php echo $search_text; ?&gt;" name="s" id="s" onblur="if (this.value == '')
			{this.value = '&lt;?php echo $search_text; ?&gt;';}" onfocus="if (this.value == '&lt;?php echo $search_text; ?&gt;')
			{this.value = '';}" /&gt;&lt;input type="hidden" id="searchsubmit" /&gt;
	&lt;/form&gt;
&lt;/div&gt;</pre>

Line 2 above is PHP code that sets a variable $search_text to &#8220;&#8230;search&#8221; IF we aren&#8217;t in the middle of a search, and &#8220;Search for: whateveryousearchedfor&#8221; if we have just performed a search. That makes the content of the form below it dynamic. Below on lines 3-9 is the search form. If just this code is added to your header you&#8217;ll have a search box floating somewhere in your header. To style it I used CSS rules:

<pre class="brush:css">.search-form input {
	width: 210px;
	color: #AAA;
	background: #000000;
	border: none;
	display: block;
	margin: 8px 8px 0 0;
	text-align: right;
	float: right;
}
.search-form input:focus {
	width: 150px;
	background: #FFFFFF;
	color: #666;
	text-align: left
}</pre>

These two rule blocks define two states of the search-form class. The first state is the default state. It&#8217;s colored black with no border, its text is white, and its margin and float rules define where it is in the header &#8211; aligned to the right.

The second block defines how it changes when it&#8217;s in focus. Its width changes, its background becomes white and its text color becomes black. It&#8217;s quite simple and it makes for an attractive and subtle search box for the Twenty Ten theme.

I also added an image of myself at the top. This was really simple by just adding the following css rules:

<pre class="brush:css">#header {
	background-image:url('http://theblawblog.com/wp-content/themes/twentyten_enhanced/images/watermark_cropped.png');
	background-repeat:no-repeat;
}</pre>

I&#8217;m still in the process of customizing my child theme, I&#8217;m not sure what I&#8217;ll be doing next but this is a great start.

Resources:

  * <a href="http://hungrycoder.xenexbd.com/scripts/header-image-rotator-for-twenty-ten-theme-of-wordpress-3-0.html" target="_blank">Header Image Rotator for Twenty Ten Theme Plugin</a>
  * <a href="http://zeaks.org/index.php/2010/05/25/twenty-ten-child-theme-part-2/comment-page-1#comment-1608" target="_blank">Twenty Ten Child Theme part 2</a>
  * <a href="http://old.nabble.com/TEMPLATEPATH-in-child-themes-td23527204.html" target="_blank">TEMPLATEPATH in child themes</a>
  * <a href="http://op111.net/53" target="_blank">How to make a child theme for WordPress: A pictorial introduction for beginners</a>
  * <a href="http://codex.wordpress.org/Child_Themes" target="_blank">Child Themes (WordPress Codex)</a>

 [1]: http://theblawblog.com/changing-my-wordpress-blog-url-and-using-lighttpd-to-redirect/