# VW Starter Kit

The VW Starter Kit quickly sets you up to create dynamically-resizing, responsive websites (using the vw, or viewport width, CSS unit). For a brief explaination of how this works watch my YouTube video: https://www.youtube.com/watch?v=mGCOsB_gF0M

## Getting Started

* Add the _vw-starter.scss SASS file to your project.
* In _vw-starter.scss, set your breakpoints, layout widths, and base font size in the variables section.

````

	// base font size must be in pixels!!!
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

Option 1 (recommended): Apply the vw-base mixin to the root <html> element. Then, in your CSS, set your style measurements (font-size, padding, margin, width, height, position, etc.) for all other elements using the px2rem() function. Simply measure your dimmensions and values in pixels in Photoshop, and enter the pixel value into the px2rem() function. Your value will be converted to REMs, which are similar to EMs except that they are relative to the root element and will not be affected by the parent element's font-size.

````

	html {
		@include vw-base;
	}

	h1 {
		font-size: px2rem(48);
		margin: px2rem(30) 0;
	}

````

Option 2: To affect individual components only, apply the vw-base mixin to the component container element. Then, use ems and the px2em() function to resize all component elements. This is a little more complicated because EMs are relative to the font-size of the context in which they are used. Therefore, if the context is different from $base-font-size you will have to pass the context into the px2em() function as the second paramater, as in the margin style example below.

````

	header {
		@include vw-base;

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

To avoid text that gets too small or expands too large, you can set min and max font sizes using the font-size-breakpoint() function.

Example:

````

	h2 {
		font-size: px2em(16);

		// when the text scales down to 12px, set a fixed font-size of 12px on desktop
		@media screen and (max-width: font-size-breakpoint(12px, $desktop-layout, 16px)) and (min-width: $desktop-min) {
			font-size: 12px;
		}

		// when the text scales up to 20px, set a fixed font-size of 20px on tablet
		@media screen and (max-width: $tablet-max) and (min-width: font-size-breakpoint(20px, $tablet-layout, 16px)) {
			font-size: 20px;
		}
	}

````

### VW Quirks & Fixes

#### Body Width Bug Fix

In many browsers, 100vw is not equivalent to 100% on the <html> element--the scroll bars are taken into account for % but not for vws. The relatively easy fix below forces <html> to be 100vw wide and is included, by default, in the vw-base mixin. If 100vw is larger than 100% it applies a negative margin to center the <html> element. The only caveat is that a little bit of your page will be trimmed on the sides.

````

	html {
		@media screen {
			width: 100vw;
			margin-left: calc((100% - 100vw) / 2);
			overflow-x: hidden;
		}
	}

````

To disable the fix in the vw-base mixin (for e.g., if you're applying vw-base to components, rather than to the <html> element) pass a value of false to the mixin:

````

	html {
		@include vw-base(false);
	}

````

####Print Stylesheets

VWs are interpreted as 0px when you print which renders elements invisible. To avoid undesirable styling, keep all vw styles within @media screen blocks.

Style for desktop first, then use @media **screen** to override your styles for tablet and mobile.

### Browser Compatibility
IE9+, Firefox, Chrome, Safari, iOS, Android 4.4+
