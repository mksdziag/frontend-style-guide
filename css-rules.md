# CSS Structure and Rules

## Spacing

- Soft tabs, 4 spaces
- Put spaces before `{` in rule declarations.
- Put spaces after `:` in property declarations.
- End property declarations with `;`.
- Put line breaks between rulesets.
- Place closing braces of declaration blocks on a new line.
- A blank line should be placed between each selector block.

```
.foo {
	property: value;
	property: value;
}

.bar {
	property: value;
}
```

Large blocks of single declarations can use a one-line format.

```
.icon_arrow::before { content: "\e602"; }
.icon_calendar::before { content: "\e603"; }
.icon_clock::before { content: "\e604"; }
```

Long, comma-separated property values, such as collections of gradients or shadows, can be arranged across multiple lines. It improves readability and produces more useful diffs.

```
.foo {
    box-shadow:
        1px 1px 1px #000,
        1px 1px 1px 1px #333 inset;
}
```

## Formatting

- Only use lowercase (except for comments)
- Use shorthand hex values, when possible. Use `#fff` instead of `#ffffff`
- Use double quotes `"..."` around values

## Units

- `line-height` should be unit-less, thus making it a multiplier based on the font-size
- Avoid specifying units for zero values. Use `margin: 0;` instead of `margin: 0px;`

## Property Ordering

Properties are grouped by categories. Each category is separated with a blank line.

- Content
 - `content`
- Typography, color and background
 - `background` `color` `filter` `font` `letter` `line-height` `text` `word`
- Display
 - `border` `box-sizing` `display` `height` `margin` `max-height` `min-height` `outline` `overflow` `padding` `vertical-align` `visibility` `width`
- Flex (`display: flex` must be first of block)
 - `align` `flex` `justify-content`
- Positioning
 - `bottom` `float` `left` `position` `right` `top` `z-index`
- Visual and animation
 - `animation` `box-shadow` `opacity` `transform`
- Others
 - `appearance` `list-style` `table-style` `cursor` ...

```
.element {
	color: #000;
	font-family: Arial, sans-serif;
    font-size: 1.5rem;

	display: block;
	padding: 12px 20px 9px;
	margin-left: 0;
	border-bottom: 1px solid #333;

	position: absolute;
	top: 10em;
	left: 10em;

	@mixin transition(#333, .2s);
}
```

## Selectors

- Reduce location depedency
- Increase portability. Use `.widget__link` instead of `.widget > a`
- Avoid over-qualified selectors. Use `.widget__link` instead of `a.widget__link`
- JS hooks prefixed with `js-` : `.js-menu` 
- State prefixed with `is-` : `.is-open`
- Try to avoid key selectors (the right-most part of a selector) that are too large, like `.foo div`. When it's impossible to avoid, try to use a child selector (e.g. `.foo > div`). This will limit the browser to look for only one level higher in the DOM

### Naming Convention

Class names should be human readable, communicating useful information. Use structural and purposeful names over presentational ones (e.g. `.important` instead of `.red`). Be short but as specific as possible. Go for `.site-logo` instead of `.logo`.

#### BEM

We use a version of the BEM technique to add intent and reletionship to classnames :

- `.block`
- `.block__element`
- `.block--modifier`

```
<div class="post">
	<h2 class="post__title">My post!</h2>
	<img class="post__image post__image--large" />
	<img class="post__image" />
	<img class="post__image" />
</div>
```

The name of a block, element or modifier consisting of multiple words will use the `-` delimiter. A block named `primary nav` would become `.primary-nav`.

### Specificity

Keep CSS specificity as low as possible.

## Single Responsibility Principle

Each class must serve a single responsibility, decomposing big blocks into small pieces that do only one thing, but do it well. Those pieces can then be reused into other elements. 

Every time you see a repeatable pattern between multiple classes, it should be splitted into a new class that will handle this pattern only.

*Bad example:*
```
<a class="card">Simple card #1</a>
<a class="card">Simple card #2</a>
<a class="ad-spot">This is an ad spot!</a>

<style>
	.card {
		background-color: #fff;

		display: block;
		width: 300px;
		padding: 20px;

		border-radius: 5px;
	}

	.ad-spot {
		background-color: #ccc;

		display: block;
		width: 300px;
		padding: 20px;
	}
</style>
```

This is bad, because `.card` and `.adSpot` are sharing common styling. If we need to change the `width` of a card, we will also need to change it inside the ad spot CSS.

*Good example:*
```
<a class="box card">Simple card #1</a>
<a class="box card">Simple card #2</a>
<a class="box ad-spot">This is an ad spot!</a>

<style>
	.box {
		display: block;
		width: 300px;
		padding: 20px;
	}

	.card {
		background-color: #fff;

		border-radius: 5px;
	}

	.ad-spot {
		background-color: #ccc;
	}
</style>
```

We now see that the `.box` class is reusable for any items. As this is only doing one thing (setting the display), we can combine it with other classes to build something more complex.

## Variables

### Types

- *Color*, starting with `c-`
- *Font*, starting with `f-`
- *Size*, starting with `s-`

### Naming Convention

- Lowercase
- Use the `-` delimiter
- You can use the modifier `--` like in BEM
- The first letter is about the type of the variable

### Examples

- `c-primary` : Primary color
- `c-secondary--light` : Secondary color, lighter
- `f-primary--light` : Primary font with bold weight
- `s-lineheight` : Default line-height value

## Comments

Each CSS file should begin with it's name and a short description of its main purpose.

```
/**
 * STYLE.CSS
 * Default application style
 */
```

### Table of contents

A table of content, just after the initial file declaration, should be describing the main sections of the file. With larger files, adding sub-sections can also be useful.

```
/**
 * STYLE.CSS
 * Default application style
 *
 * - REQUIRE - DEF              Require and images def
 * - INIT                       Document initialization
 * - LAYOUT                     Container, Wrapper, Stripe
 * - SITE SECTIONS              Site sections: sidebar, nav, header, main
 * - ELEMENTS                   Default style for elements like <p> and <a>
 * - FORMS                      Form styling
 * - MESSAGES                   Messages
 * - STATES                     States
 * - CLASSES                    Common classes
 */
```

or

```
/**
 * LINECHART.CSS
 * Linechart for DC elements
 *
 * - REQUIRE - DEF              Require and images def
 * - STYLE
 *   + Legend
 *   + Axis 
 *   + Grid
 */
```

### Sections

Every sections (defined in the table of content) should start with a title. This title is precided by 2 new lines and have one new line after.

```
/* -- Name of the section ---------------------------------------------- */
```

### Sub-sections

Useful sub-sections should also be delimited by a simpler title.

```
/* -> My sub-section -- */
```

## Media-queries

Place media queries as close to their relevant rule sets whenever possible. Don't bundle them all in a separate stylesheet or at the end of the document. Doing so only makes it easier for folks to miss them in the future. Here's a typical setup.

```
.element { ... }
.element--selected { ... }
.element__avatar { ... }

@media (min-width: 480px) {
	.element { ...}
	.element--selected { ... }
	.element__avatar { ... }
}
```

## Others

- W3C valid
- Add new line at end of file
- Classes, variables and comments should use english spelling
