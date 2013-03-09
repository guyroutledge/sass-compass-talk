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

There are two different sytaxes: SASS and SCSS.

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

You can then start writing SASS beneath the comment block at the top. To make
sure SASS keeps this comment block, you can put a `!` after the first asterisk:

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

SASS has your back here. We have aleady seen how SASS compiles into a CSS file
but it doesn't have to be a 1-to-1 relationship. You can have as many SASS
`partials` as you like and `@import` them into a single stylesheet. Because SASS
is *pre*-processed, these `@imports` don't incure any extra requests. 

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

<!-- link to variables gist -->

### Nesting

### Mixins

### Extends

### Color Functions

### Math

### Flow Control

### Functions



## What is Compass?


## What does it Do?

### Vendor Prefixes

### Helper Functions
#### Image Dimensions
#### Elements-of-Type
#### Inline-Image
#### URL Helpers

### Sprites


