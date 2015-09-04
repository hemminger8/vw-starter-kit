# VW Starter Kit

The VW Starter Kit quickly sets you up to create dynamically-resizing, responsive websites (using the vw, or viewport width, CSS unit).

## Getting Started

* Add the vw-starter SASS file to your project.
* In _vw-starter.scss, set your breakpoints and base font size in the variables section.

````

	$base-font-size: 16px;

	// layouts -- the pixel width of the Photoshop file for each layout
	$desktop-layout: 	1250px;
	$tablet-layout: 	640px;
	$mobile-layout: 	320px;

	// breakpoints -- the browser window size at which the layout should change
	$desktop-min: 		900px;
	$tablet-max: 		899px;
	$tablet-min: 		601px;
	$mobile-max: 		600px;

````

## Usage

There are a couple different ways you can use VWs in your project.

Option 1: Set a base font-size on the root <html> element. Then, in your CSS, set font-size, padding, margin, etc. for all other elements using the px2rem() function.

````

	html {
	    height: 100%;
	    font-size: $base-font-size;

		// font-size set to 16px at the desktop design layout width
	    @media screen and (max-width: $desktop-layout) {
	        @include vw-font-size($base-font-size, $desktop-layout);
	    }

		// font size reset to 16px at the tablet design layout width
	    @media screen and (max-width: $tablet-max) {
	        @include vw-font-size($base-font-size, $tablet-layout);
	    }

		// font size reset to 16px at the mobile design layout width
	    @media screen and (max-width: $mobile-layout) {
	        @include vw-font-size($base-font-size, $mobile-layout);
	    }
	}

````

Option 2: To affect individual components only, set a font-size on the component container element. Then, use ems and the px2em() function to resize all component elements.

````

	header {

		font-size: $base-font-size;

	    @media screen and (max-width: $desktop-layout) {
	        @include vw-font-size($base-font-size, $desktop-layout);
	    }

	    @media screen and (max-width: $tablet-max) {
	        @include vw-font-size($base-font-size, $tablet-layout);
	    }

	    @media screen and (max-width: $mobile-layout) {
	        @include vw-font-size($base-font-size, $mobile-layout);
	    }


	    h1 {
			font-size: px2em(48);
			margin: px2em(30, 48) 0;
		}

		h2 {
			font-size: px2em(36);
		}

    }

````

### Setting minimum and maximum font sizes

For accessibility to and to avoid text that resizes until infinity, you can set min and max font sizes using the vminBreakpoint() and vmaxBreakpoint() functions.

Example:

````

	h2 {
		font-size: px2em(16);

		// when the text becomes smaller than 12px, double its size!
		@media (max-width: vminBreakpoint(12px, $desktop-layout, 16px)) {
			font-size: px2em(16*2);
		}
	}

````

### VW Quirks & Fixes

#### Body Width Bug Fix

In many browsers, 100vw is not equivalent to 100% on the body element--the scroll bars are taken into account for % but not for vws. The relatively easy fix below forces the body to be 100vw wide. If 100vw is larger than 100% it applies a negative margin to center the body. The only caveat is that a little bit of your page will be trimmed on the sides.

````
body {
	width: 100vw;
	margin-left: calc((100% - 100vw) / 2);
	overflow-x: hidden;
}
````

#### Old Android Browsers Fallback

For old Android native browsers, use the following fallback:

````
html {
	@for $i from (320/2) through (strip-units($mobile-max)/2) {
		@media screen and (width: $i * 2px - 1px), (width: $i * 2px) {
			font-size: ($i * 2px) / strip-units($mobile-layout);
		}
	}
}
````

## Credits: 
[Jesse Hemminger](https://github.com/hemminger8/vw-starter-kit)
