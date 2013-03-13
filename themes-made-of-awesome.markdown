# Themes Made of Awesome with SASS and Compass

Introduction
Who the hell am I?
What am I going to talk about?
Why should I be telling you about it?
Where can I get some code?

## What is SASS

SASS is CSS pre-processor.

It's like an extension of CSS in that it provides extra functionality and
features that makes designing websites faster, more maintainable and more fun.

The clue to what makes this possible is in the name *pre-processor*. 

Instead of writing CSS you write SASS or SCSS and then run a compiler which
turns it into plain old normal CSS for use on your website.

There are two different syntaxes: SASS and SCSS.

SASS looks like this:

	.module
		display:inline-block
		margin:1em
		padding:1em
		background:pink
		border:1px solid grey

SCSS looks like this:

	.module {
		display:inline-block;
		margin:1em;
		padding:1em;
		background:pink;
		border:1px solid grey;
	}

If you're used to writing CSS, you'll probably find SCSS easier to switch to as
any valid CSS syntax is valid SCSS. However, SCSS has a whole lot of awesome to
add to the game.

## How do I get it?

There are a whole host of GUI apps that can help you work with SASS. Some of
these include [CompassApp][1], [Scout][2], [LiveReload][3], [Codekit][4],
[Hammer for Mac][5], [Mixture.io][6] and probably others too.

However, the simplest way to get it and run it is from the command line. Let me
show you how not painful it is:

	sudo gem install sass

This brings us on to how to use SASS. And specifically how to use it with
WordPress...

## How do I use it with WordPress?

`style.css` is core to making WordPress work so we need to make sure we keep
a hold of it. There are a few ways we can do this.

In the most basic form you can just rename your `style.css` file to have the SCSS
file extension of `.scss` and then tell SASS to watch for changes.

	cd path/to/style.css
	mv style.css style.scss
	sass watch --style.scss:style.css

You can do this with a complete and working theme too - and if you would rather
use the whitespace dependent SASS syntax you can run a conversion on the command
line:

	sass convert style.css style.sass

You can then start writing SASS (or SCSS) beneath the comment block at the top.
To make sure SASS keeps this comment block, you can put a `!` after the first
asterisk:

	/*!
	Theme Name: Your Theme
	...
	*/

However, there are some limitations to just working with this simple directory
structure which will become clear when we start talking about some of the
features of SASS. 

You can tell SASS to watch a directory of multiple SASS `partials` and compile
them down into a single CSS file in another directory. The command to make that
happen is this:

	sass --watch scss:css

That will watch for changes in any `.scss` files in the `scss` directory and
compile them into a `.css` file (with the same name) in a directory called
`css`. Here's an image to show a sample directory structure:



Now to use this compiled stylesheet all you have to do is include it in your
theme:

	<?php wp_enqueue_style( 'global-styles', get_template_directory_uri() . '/css/global.css', null, '1.0', 'screen' ) ?>

But what about `style.css`?

`style.css` may be one of the two files *required* by WordPress but as long as
it's there and has the correct comment block, you're all set. There is no need
to add it to the `<head>` of your theme - in fact doing so would be a waste of
an HTTP request.
	

## What does it do?

So we've got SASS installed and the compiled stylesheet linked up in our theme,
what now? Well you could write plain old CSS and call it a day, but let's use
a few examples to run through some of the awesome features of SASS.

### Organisation

You are no doubt familiar with the idea of a single `style.css` file that
contains all the CSS for a whole project. You start out meaning well, even throw
in some comments to section things off a bit but as the project grows and
perhaps as time passes and things are changed, removed or added, things get
a bit bloated and hard to maintain. This can lead to all sorts of issues
- especially if there are multiple people working on the project. 

SASS has your back here. We have already seen how SASS compiles into a CSS file
but it doesn't have to be a 1-to-1 relationship. You can have as many SASS
`partials` as you like and `@import` them into a single stylesheet. Because SASS
is *pre*-processed, these `@imports` don't incur any extra requests. 

Partials are prefixed with an underscore character and you might have several of
them for things like your reset, grid, typography, header, footer, sidebar,
etc. You can then include all these components that make up your global styles
into a `global.scss` file. 

	// In global.scss
	// Partials for global styles to be compiled to global.css

	@import "global-reset";
	@import "global-grid";
	@import "global-typography";
	@import "global-header";
	@import "global-footer";
	@import "global-sidebar";
	@import "global-forms";
	@import "global-buttons";

You could use a naming convention to keep things even more organised although
I tend to have a `global` directory, a `template` directory, a `pages` directory
and a `plugins` directory instead. You could do both and be super organised...

If you have a big website, you might want to create a separate compiled
stylesheet for a products section (for example) and enqueue it only on the
relevant pages. 

	// In product.scss
	// Partials for the shop to be compiled to product.css

	@import "product-range";
	@import "product-slideshow";
	@import "product-sidebar";
	@import "product-single";

This will mean your users will only download what they need,
when they need it. It also helps with caching - if the global styles are
unlikely to change much but you're forever adding new features to a single
stylesheet, the cache is broken and the user has to download the whole lot all
over again. This is *not* awesome.

### Variables

Variables are one of the things people rave about in SASS. They are very cool.

They can be used to set up you default color pallet, spacing, font-stacks, you
name it - if it can be set with CSS, it can be stored in a variable. Variables
can be all defined in a single partial or can be declared inline. I tend to
store global variables for the overall look and feel in a single
`_variables.scss` partial and then have "single use" variables inter-mingled
throughout my styles.

I also tend to use a prefix with variables so I know what they are supposed to
be for. Because you can store anything in a variable, you could do the
following:

	$padding: 2em;

	.module {
		width:50%; 
		padding:$padding;
		margin:$padding; // wtf?
	}

<!-- link to variables gist -->

### Nesting

Nesting allows you to write less. Imagine marking up a navigation you might
write some CSS like this:

	.main-nav               { /* styles */ }
	.main-nav ul            { /* styles */ }
	.main-nav ul li         { /* styles */ }
	.main-nav ul li a       { /* styles */ }
	.main-nav ul li a:hover { /* styles */ }
	.ie8 .main-nav          { /* styles */ }

To get the exact same structure in SASS you can just do this:

	.main-nav {
		// styles 

		ul {
			// styles 

			li {
				// styles 

				a {
					// styles 
					
					&:hover {
						// styles
					}
				}
			}
		}

		.ie8 & {
			// styles
		}
	}

When SASS compiles, it will actually generate the CSS in the first example.

It's advisable not to nest too deeply as this can cause you issues with CSS
specificity and unnecessary "code bloat" which is often one of the arguments
used against SASS. However, if you learn how SASS compiles into CSS you can be
aware of these issues and avoid them. 

<!-- link to gist for nesting -->

### Mixins

Mixins enable you to re-use a snippet of code in more than one place. It's
important to think about what this means though - if you are reusing a lot of
code by using mixins, there's probably a more efficient way of writing your CSS.
However, one of the big features of mixins is that they can accept arguments. 

	@mixin button($background, $foreground, $font-size:1em){
		display:inline-block;
		padding:0.5em 1em;

		color:$foreground;
		font-size:$font-size;
		font-family:$font-brand;

		background:$background;
	}
	.button {
		@include button(blue, white);
	}
	.button-error {
		@include button(#f90, white, 2em);
		box-shadow:0px 0px 5px rgba(red, 0.3);
	}

Another handy feature of mixins is the `@content` feature. This allows you to
re-use styles with different selectors.

	@mixin placeholder() {
		::-webkit-input-placeholder {
			@content;
		}
		::-moz-placeholder {
			@content;
		}
		:-moz-placeholder {
			@content;
		}
		:-ms-input-placeholder {
			@content;
		}
	}

It's likely that you want the same placeholder attribute styling for all
browsers but you can't comma separate these for some reason - using a mixin with
`@content` you can call it with as follows:

	@include placeholder{
		color:#ccc;
		font-style:italic;
	}

### Extends and Silent Placeholders

As well as being a big fan of SASS, I'm a big fan of lean, modular CSS.
There are frameworks for this like [OOCSS][7] but I tend to prefer the more
theoretical and framework-free [SMACSS][8]. 

These methodologies are similar in that they both encourage breaking up your
styles into reusable components that are applied as classes in your HTML - and
not just a single class on an element but perhaps many of them; a base module
followed by modifiers. For example:

	<a class="button button-large button-error" href="/help">Fail!</a>

This is all well and good but it might be neater if there weren't three
presentational class names on our link. Enter `@extend` and `silent placeholder
classes`.

You will be familiar with the following CSS selectors:

	a { }             // element selector
	.button { }       // class selector
	#button-large { } // ID selector

In SASS we have silent classes that look like this:

	%button-large { font-size:3em; }

When SASS compiles, this silent placeholder is no-where to be seen until we use
it to `@extend` an existing selector.

	%button {
		display:inline-block;
		margin:1em 0;
		padding:1em 2em;

		color:#fff;
		font-size:1em;
		background:#eee;
	}
	%button-large { font-size:3em; }

	.feature-button {
		@extend %button;
		@extend %button-large;
	}

This will apply the styles from `%button` and `%button-large` to the
`.comment-submit-button` class. This class is named by it's purpose but is made
up of a series of presentational components. This can make our markup more human
readable and reduce the number of presentational class names. As these silent
placeholders don't compile into anything, they also make our output CSS nice and
lean. That's two helpings of awesome in one go!

### Color Functions

Colour makes the web a more beautiful place. We're already using variables to
keep use of our colour consistent (eg. $color-brand) but what if we want to use
variations on that color and don't remember how `#f29c7b` relates to `#e9561e`
without having to open up Photoshop.

	a { 
		color:$color-brand; // #f29c7b
	
		&:visited { 
			color:darken($color-brand, 20%); // #e9561e
		}
	}

There are a whole load of color functions - some I use very regularly and others
never at all. Here's a few of my favourites:

	rgba($color, $alpha);
	adjust-hue($color, $degrees);
	mix($color1, $color2, [$weight]);
	lighten($color, $amount);
	darken($color, $amount);
	saturate($color, $amount);
	desaturate($color, $amount);
	transparentize($color, $amount);

### Flow Control, Functions and Math

Now we're getting to some powerful stuff. SASS turns CSS into something closer
to a scripting language (like JavaScript) more than a purely declarative
language and it does this by adding conditionals, loops, functions and
mathematic operations. The math is fairly self explanatory so we'll just look at
that with some wider examples. Let's first take a look at conditionals...

#### Conditionals

Conditionals are used in programming to change what happens based on a
condition which evaluates to true or false (or truthy or falsey to be more
precise). SASS uses the following conditionals:

	@if ( $condition ) {
		// do stuff if $condition is true
	} @else if ( $other-condition ) {
		// do this instead if $condition is false
		// but $other-condition is true
	} @else {
		// do this if neither of the above are true
	}

Now that's all very well, but how is this useful in CSS? How about an
accessibility example?

Let's say you have a error message that has a light red background. What color
text should it have for the most contrast, black or white? We can use SASS to
find out:

	$color-background:#f29c7b; // light orange/red
	$color-foreground:null;

	@if ( lightness($color-background) > (lightness(#fff) + lightness(#000))/2 ) {
		color-forground:#000;
	} @else {
		$color-foreground:#fff;
	}

In this case the lightness (or luminosity) component of `#f29c7b` is 71.5%
according to SASS. This is greater than the midpoint between black and white
(50%) so we should use black text for higher contrast. You can have a play
around with other colors in the `_conditionals.scss` partial included with the
example files.

#### Loops

Loops are another common programming concept - they take a block of instructions
and repeat them as long as a specified condition is true. It's simplest to look
at an example.

	@for $i from 1 through 4 {
		column-#{$i}-of-4 {
			width: $i * 25%;
		}
	}
	// outputs:
	// .column-1-of-4 { width:25%; }
	// .column-2-of-4 { width:50%; }
	// .column-3-of-4 { width:75%; }
	// .column-4-of-4 { width:100%; }

The for loop runs the block of code between it's braces setting the value of $i
from 1 *through* (up to and including) 4. If the loop had the condition 
`$i from 1 *to* 4` it would only run 3 times; it runs from 1 up to (but not including) 4.

The same thing could be achieved with a `@while` loop which gives more
fine-grained control of how the counter variable ($i) changes:

	@while $i <= 4 {
		column-#{$i}-of-4 { 
			width: $i * 25%;
		}
		$i: $i + 1;
	}

The difference here is that we have to specify how we want $i to be modified so
that the loop continues. We could have done `$i: $i + 2` and get the following
output:

	.column-1-of-4 { width:25%; }
	.column-3-of-4 { width:75%; }

On the third iteration, `$i` would have been increased from 3 to 5 and the
condition "while `$i` is less than or equal to 4 would not be true and the loop
would stop. I have used `@for loops` numerous times when writing SASS but have
not yet found myself reaching for `@while`...

The third and possibly most useful loop is the `@each loop` this is similar to
`foreach` in PHP and `$.each` in jQuery. It's a loop that operates on a set of
data. In SASS that set of data takes the form of a list (although in other
languages it's usually an array). Let's look at a common example of social media
icons:

	$social-icons: twitter, facebook, google-plus, pinterest, dribble, youtube;
	@each $social in $social-icons {
		.share-#{$social} {
			background:url('../images/#{$social}-icon.png');
		}
	}
	// outputs
	// .share-twitter     { background:url('../images/twitter-icon.png'); }
	// .share-facebook    { background:url('../images/facebook-icon.png'); }
	// .share-google-plus { background:url('../images/google-plus-icon.png'); }
	// .share-pinterest   { background:url('../images/pinterest-icon.png'); }
	// .share-dribble     { background:url('../images/dribble-icon.png'); }
	// .share-youtube     { background:url('../images/youtube-icon.png'); }

If you're more social than me, this could save you a *lot* of time. 


## What is Compass?

So this is all great stuff but this is just SASS. We've not even looked at the
most awesome thing that you get when you work with SASS: Compass. 

Compass is a framework that sits on top of SASS. Since SASS is a framework that
sits on top of CSS, you might be wondering what the hell a framework on top of a
framework looks like. 

Compass provides a set of helper functions, tools and pre-defined mixins that
can save you even more time and make your stylesheets more maintainable. That
sounds very similar to my definition of SASS so the best thing to do is
demo exactly *what* this enhanced version of SASS can do...


### How do I get it?

First off, we better install it. Remember how easy it was to install SASS? Well,
Compass is pretty much the same deal:

	sudo gem install compass

When that has installed (along with it's documentation) you're all set to create
a Compass project.

	cd /path/to/theme
	compass create --sass-dir=scss --css-dir=css
	compass watch

You now have all the power of Compass at your fingertips.

Compass will create your directory structure as defined by the `--sass-dir` and
`--css-dir` options. You can leave these blank and Compass will use it's
defaults - I just prefer this naming over `sass` and `stylesheets` which are the
defaults.

To tell Compass to watch for changes, just type `compass watch`. You can also
issue the command `compass compile` which will compile everything once but not
watch for further changes.

#### Compass Config

Compass creates a configuration file called `config.rb` in your project
root. This is where you can change the paths to your `scss` and `css`
directories as well as tell compass where your HTTP root, images and javascript
folders are. 

Here is also where you can control the output of the compiled CSS. The options
are `:expanded`, `:nested`, `:compact` or `:compressed`.

During development, I'd recommend to use the `:expanded` setting - this will
leave plenty of whitespace and helpful comments in the compiled stylesheet that
can be useful when debugging. 

When putting a site live, I'd recommend the `:compressed` setting which removes
all inline comments and whitespace, minifying and compressing the file to it's
smallest possible size - this is great for performance.

All of the [material on github][9] is contained within a Compass project so have
a play around with the different settings to see which ones you prefer.

Now let's have a look at some of the awesome stuff Compass gives you access
to...


### Compass Mixins

When we talked about mixins, I showed an example of writing your own mixins to
cut down on keystrokes and automate some of the repetition for vendor prefixes.
But when writing mixins, you still have to *write* them. Compass gives you them
for free. Lots of them.

	// This is boring to type out
	-webkit-transform: rotate(5deg);
	   -moz-transform: rotate(5deg);
	     -o-transform: rotate(5deg);
	    -ms-transform: rotate(5deg);
	        transform: rotate(5deg);

	// This is super fast
	@include rotate(5deg);

Compass has you covered in almost every case - they also keep them up to date so
when a browser drops support for a prefix, it will be dropped from Compass.
There are a couple of caveats in that you need a Compass plugin for animations
and last time I tried to use a radial gradient the syntax hadn't been updated -
but for the most part you're all set. 

However, vendor prefixes are a little bit of a boring topic these days. Let's
quickly look at a couple of examples that are really sweet:

	@include alternating-rows-and-columns($even-row-color, $odd-row-color, $dark-intersection) 

This one-liner produces CSS for a table with alternating coloured rows and
columns with borders that are a blend of the background and `$dark-intersection`
variable. You can optionally add a fourth and fifth argument for `$header-color`
and `$footer-color`. Take a look at the sample code for a glimpse at the awesome
behind this 1-line include. It's incredible.

There's loads of helper utilities for things like setting all your headings and
links colors in one fell swoop or adding a clearfix without having to Google for it
first. There's mixins for horizontal lists, baseline grids, text-replacement and
more. I *highly* recommend reading through the [Compass docs][10] to see what
else is available.

As well as all these helpful bits of off-the-shelf goodness, Compass allows you
to combine it's features with the features of SASS. As such, you can build quite
complex bits of functionality with little hassle.

	@mixin generate-grid($_cols, $_width, $_gutters, $_grid_wrapper:grids, $_guides:false, $_color:red) {
		$_total: $_cols * ($_width + $_gutters);
		$_margin: ($_gutters / $_total) * 100;

		[class^="grid-"] {
			float:left;
			margin: 0 (1% * $_margin) 0 0;
		}

		.#{$_grid_wrapper} {
			clear:both;
			width:auto;
			margin:0 0 0 (-1%*$_margin);
			overflow:hidden; /* clear the floats */

			[class^=grid-] {
				float:left;
				margin:0 0 0 (1% * $_margin);
			}

			@for $i from 1 through $_cols {
			.grid-#{$i} {
				width: ((($_width * $i) + ($_gutters * ($i - 1)))/$_total)*100%;
			}
			}
		}
	}

The above mixin will build a fluid grid for responsive design with any
combination of columns, column widths and gutter widths you can think of. It's
similar to 960.gs and based on an old grids generator from CSS-Wizardry. There's
code up for grabs on [my github profile][11].

To use it just call 

	@include generate-grid(12, 60, 20);

And you'll get a pretty familiar 12 column fluid grid. Awesome.

### Helper Functions

Mixins etc. are great and powerful but sometimes you just need a little bit of a
helping hand and Compass has a couple of really useful helper functions.
Actually, it has a shit-load of helper functions for everything from `pi()` to
`sin(), cos(), tan()` to `inline-image()` that embeds a data-URI of an image
asset.

But there are a couple of useful ones that are worth knowing about:
`image-url()` and `image-dimensions()`.

	$image: image-url(image.png);
	$padding:20px;

	.element-with-bg-image {
		width: image-width($image) - ($padding*2);
		heigth: image-height($image) - ($padding*2);
		padding: $padding;
		background:url($image);
	}

By setting all these properties with variables saves you having to do the math
and saves you re-doing all the math when you inevitably end up saving out a
tweaked version of an image with slightly different dimensions than before. It's
an easy win, every time.

### Sprites
