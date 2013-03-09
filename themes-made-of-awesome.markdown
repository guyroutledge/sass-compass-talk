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

## What does it do?

### Organisation

### Variables

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


