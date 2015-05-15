# vw-starter-kit
Get ready to have your mind blown by the magic of vw CSS units and take your CSS acrobatics to the next level.

## How it works
If you haven't heard of vw's before, it is a new CSS unit of measure and basically 1vw = 1% of the width of the viewport.
Our layout is designed at a certain width, in this example the 
layout width is stored in the sass variable $mobile-layout.
I want to set the font-size to a vw value such that when 
the browser-width = $mobile-layout, 1rem = 1px so that when 
I measure something in photoshop to be 32px I can just set it 
to 32rem in the css (or sass) and it will match the photoshop file and scale.

	1% of browser-width = 1vw

or writen another way

	browser-width * 0.01 = 1vw

Because we said above that we want to find the vw value for 1px
when browser-width = $mobile-layout, we can substitute 
browser-width = $mobile-layout

	$mobile-layout * 0.01 = 1vw

separate the px units from the value

	strip-units($mobile-layout) * 1px * 0.01 = 1vw

divide both sides by 0.01

	strip-units($mobile-layout) * 1px = 100vw

divide both sides by strip-units($mobile-layout)

	1px = 100vw/strip-units($mobile-layout)

So simple we don't even need a mixin!
```css
font-size: (100vw/strip-units($mobile-layout));
```

Now do the same for each layout.
```scss
@media screen and (min-width: $tablet-min) {
	font-size: (100vw/strip-units($tablet-layout));
}
```

Setting a max scale amount for your layout is just as easy.

Once the layout has increased to 120 percent (i.e. $tablet-layout * 1.2), 
stop the scaling by setting a fixed font-size.
In this case 1px * 120%, or 1px * 1.2 = 1.2px.
Again, so simple we don't need a sass mixin.
```scss
@media screen and (min-width: ($tablet-layout * 1.2)) {
	font-size: 1.2px;
}
```
The max scale trick is particularly useful when you have a max width for your layout, i.e. at some point the layout stops scaling and you just center it and fill in the extra space on the sides with some color or tecture.

Now don't forget that you have to set the font-size for each breakpoint.
```scss
@media screen and (min-width: $desktop-min) {
	font-size: (100vw/strip-units($desktop-layout));
}
```

It is also easy to set a minimum scale amount for your layout. But to be honest I don't ever do this because it just reintroduces the problem that we are trying to solve with vw's, namely issues with wrapping as the width changes. Instead I just have a tablet layout that kicks in before the text becomes too small to be readable.

Once the layout has decreased to 80 percent (i.e. $desktop-layout * 0.8), 
stop the scaling by setting a fixed font-size.
Again, 1px * 80%, or 1px * 0.8 = 0.8px
```scss
@media screen and (min-width: $desktop-min) and (max-width: ($desktop-layout * 0.8)) {
	font-size: 0.8px;
}
```

It's that simple. Now simply measure things in pixels in your Photoshop layout and set them in rem's in your css.

## Example
http://hemminger8.github.io/vw-starter-kit/
