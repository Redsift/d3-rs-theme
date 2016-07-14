# d3-rs-theme

`d3-rs-theme` color and theme support for charts. Currently, a light theme and dark theme are provided. The light theme is considered the default.

## Builds

[![Circle CI](https://circleci.com/gh/Redsift/d3-rs-theme.svg?style=svg)](https://circleci.com/gh/Redsift/d3-rs-theme)

## Script Import

`//static.redsift.io/reusable/d3-rs-theme/latest/d3-rs-theme.umd-es2015.min.js`

## Example

[View @redsift/d3-rs-theme on Codepen](http://codepen.io/rahulpowar/pen/ZOOLGW)

### Brand color palette

![Color palette for brand](https://raw.githubusercontent.com/Redsift/d3-rs-theme/master/readme/brand.png)

This package includes the Red Sift brand color scheme. These colors are typically only used for UI.

### Presentation color palette

![Color palette for charts](https://raw.githubusercontent.com/Redsift/d3-rs-theme/master/readme/palettes.png)

Primary use for this package is central access to the Red Sift presentation color scheme.

The colors above represent the `d3_rs_theme.presentation10` arrays which is a ordinal color scale. The entries should be used in sequence as they are arranged to provide distinctive contrasts. The primary theme and the one that should be used normally is the `standard` scale at `d3_rs_theme.presentation10.standard`. `d3_rs_theme.presentation10.darker` and `d3_rs_theme.presentation10.lighter` variations are also provided if options are required. A helper map is also provided at `d3_rs_theme.presentation10.names` to access the index values of colors by name.

    // Example browser usage
    let siftBlue = d3_rs_theme.presentation10.standard[d3_rs_theme.presentation10.names.blue];
	// siftBlue is the string #73c5eb

### Patterns

![Patterns for charts](https://raw.githubusercontent.com/Redsift/d3-rs-theme/master/readme/patterns.png)

In addition to colors, the theme module has build in generators for SVG patterns that can be combined with the colors. The patterns are packaged using the reusable chart convention and can be applied by injecting them into the parent SVG and referencing them when required.

	// typically use the first pattern, diagonal1
	let pattern = d3_rs_theme.diagonals('optional-name', d3_rs_theme.patterns.diagonal1);
	
	// set the foreground to the standard color
	pattern.foreground(d3_rs_theme.presentation10.standard[d3_rs_theme.presentation10.names.blue]);
	
	// set the background to the light variant
	pattern.background(d3_rs_theme.presentation10.lighter[d3_rs_theme.presentation10.names.blue]);
	
	// add the pattern to the svg (here svg is from d3-rs-svg)
	d3.select('body').call(svg).select(svg.self()).call(pattern);
	
	.....
	
	// get the reference for the fill
	..append('rect').attr('fill', pattern.url());


### Highlights

![Highlight on light and dark](https://raw.githubusercontent.com/Redsift/d3-rs-theme/master/readme/highlight.png)

A specific `highlight` pattern is provided to draw area markers. It can also be used to draw a line marker.

	let pattern = d3_rs_theme.diagonals('pattern-' + theme, d3_rs_theme.patterns.highlight);
    pattern.foreground(d3_rs_theme.display[theme].highlight);
    
    d3.select('body').call(svg).select(svg.self()).call(pattern);
    
    .....
	
	
	..append('rect')
		.attr('fill', pattern.url()), // get the reference for the fill
		.attr('width', pattern.interval()) // size the width by the central setting
		.attr('height', 4 * pattern.size()); // size the height by a multiple of the pattern size


### Lines

![Lines on light and dark](https://raw.githubusercontent.com/Redsift/d3-rs-theme/master/readme/lines.png)

A number of line styles are provided for data presentation. Colors are under `d3_rs_theme.display[theme].axis|grid` and the stroke thicknesses are `d3_rs_theme.dataWidth`, `d3_rs_theme.axisWidth` and `d3_rs_theme.gridWidth`. Additionally the grid line has a dash style that is applied to the style attribute `stroke-dasharray` under `d3_rs_theme.gridDash`.

Remember to use the CSS style `shape-rendering: crispEdges` for lines and paths.
	

### Fonts

![Fonts at 2 widths](https://raw.githubusercontent.com/Redsift/d3-rs-theme/master/readme/fonts.png)

Fixed width, variable width and brand fonts are provided with sane fallbacks. Using these fonts will first required a CSS import.
	
	let svg = d3_rs_svg.html()...;
    svg.style([
              d3_rs_theme.fontImportVariable,   
              `${svg.self()} text { font-family: ${d3_rs_theme.fontFamilyVariableWidth}; fill: ${d3_rs_theme.display[theme].text};  }`
            ].join('\n'));
            
Font sizes are calculated based on the available SVG width via the function `fontSizeForWidth()`.            
	
### Animations

![Curves for charts](https://raw.githubusercontent.com/Redsift/d3-rs-theme/master/readme/curves.gif)

A custom easing function is provided to animate transitions. This is primarily for motion as opposed to fades or other transitions.

	// set the x value on the rect using the custom duration and easing
	rect.transition().duration(d3_rs_theme.duration).ease(d3_rs_theme.easing()).attr('x', newValue);

### Other tools

You can create a theme shadow for SVG elements using `d3_rs_theme.shadow()` in a manner similar to patterns.

Check text color legibility using the `d3_rs_theme.contrasts.white()` function. It will return false if the color provided will not provide adequate contrast with white text.

	// fill with white or black text depending on the value of color
    let t = d3_rs_theme.contrasts.white(color) ? 
			d3_rs_theme.display.text.white : d3_rs_theme.display.text.black;        
    text.attr('fill', t);

## ES6 Usage

If you are using rollup or a suitable es6 module compatible system, you benefit from tree shaking.

	import { 
		random as random, 
		contrasts as contrasts, 
		presentation10 as presentation10
	} from '@redsift/d3-rs-theme';

	// make a random theme color function based on the standard presentation10 with grey filtered out. because reasons.
	let colors = random(presentation10.standard.filter((e, i) => i !== presentation10.names.grey));

	// get a random rgb string
	let rgb = colors();
	
	// get a random color as a function of the string 'hashme'
	let rgb = colors('hashme');

## Acknowledgements

This uses code from https://github.com/MoOx/color-convert under MIT

This uses thinking from http://hci.stanford.edu/~cagatay/projects/pk/perceptual-kernels.pdf
