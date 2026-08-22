---
layout: page
title: Fonts
parent: rcss
next: text
---

RCSS implements a simpler version of the [CSS2 font model](http://www.w3.org/TR/REC-CSS2/fonts.html) when dealing with text rendering. This is for two reasons:

* The document renderer is fully under the control of the author, so (for example) a specific font can be assumed to exist.
* Improved performance.

Fonts are specified in a similar fashion to CSS. However, before a font can be used, it must be loaded into the font engine. This can be done from RCSS using the [`@font-face`](#font-face) at-rule, or from C++ using [`Rml::LoadFontFace()`](../cpp_manual/fonts.html).


### Font face declarations: The '@font-face' at-rule
{:#font-face}

The `@font-face`{:.prop} at-rule loads one or more font files and registers them with the font engine under a given family name, so that they can be used with the `font-family`{:.prop} property. It is the RCSS equivalent of calling [`Rml::LoadFontFace()`](../cpp_manual/fonts.html) from C++.

```css
@font-face {
	font-family: "Roboto Mono";
	src: "assets/RobotoMono-Regular.ttf", "assets/RobotoMono-Bold.ttf";
}

@font-face {
	font-family: "Roboto Mono";
	src: "assets/RobotoMono-Italic.ttf", "assets/RobotoMono-BoldItalic.ttf";
	font-style: italic;
}

body {
	font-family: "Roboto Mono";
}
```

Both `font-family`{:.prop} and `src`{:.prop} are required, all other descriptors are optional. A block missing either of them is ignored, and a warning is emitted to the log.

The at-rule is processed as soon as the style sheet is parsed, and the resulting font faces are registered globally in the font engine. This implies they are not scoped to the style sheet or document that declared them. Declaring the same face several times is harmless, any repeat is detected as a duplicate and skipped.

#### Descriptors

`font-family`{:.prop#font-face-font-family}

Value: | \<string\>
Initial: | undefined
Applies to: | `@font-face`{:.prop} blocks
Inherited: | N/A
Percentages: | N/A

The family name to register the loaded faces under. Required.


`src`{:.prop#font-face-src}

Value: | \<string\> \[, \<string\>\]\*
Initial: | undefined
Applies to: | `@font-face`{:.prop} blocks
Inherited: | N/A
Percentages: | N/A

A comma-separated list of font files to load. File names are resolved relative to the path of the current style sheet. Required.


`font-style`{:.prop#font-face-font-style}

Value: | normal \| italic
Initial: | normal
Applies to: | `@font-face`{:.prop} blocks
Inherited: | N/A
Percentages: | N/A
Required: | No

The style to register the loaded faces as. This always overrides the style declared inside the font file, thus an italic font file must be declared with `font-style: italic`{:.prop} for it to be selected by the `font-style`{:.prop} property.


`font-weight`{:.prop#font-face-font-weight}

Value: | all \| normal \| bold \| \<number \[1,1000\]\>
Initial: | all
Applies to: | `@font-face`{:.prop} blocks
Inherited: | N/A
Percentages: | N/A

Values have the following meanings:

`all`{:.value}
: The weight is retrieved from the font file. If the file contains several weight variations, all of them are loaded.

`normal`{:.value}
`bold`{:.value}
`<number>`{:.value}
: The weight to register the font as. When the font file contains several weight variations, this selects which variation to load.


`-rmlui-fallback-face`{:.prop#font-face-fallback-face}

Value: | false \| true
Initial: | false
Applies to: | `@font-face`{:.prop} blocks
Inherited: | N/A
Percentages: | N/A

When `true`{:.value}, the loaded faces are used for any characters that cannot be found in the fonts specified by the document. Several fallback faces can be declared, they are prioritized in the order they were loaded. See [loading fonts](../cpp_manual/fonts.html) for details.


`-rmlui-face-index`{:.prop#font-face-face-index}

Value: | \<number\>
Initial: | 0
Applies to: | `@font-face`{:.prop} blocks
Inherited: | N/A
Percentages: | N/A

The index of the face to load within a font collection, such as a `.ttc`{:.path} file.


### Font specification properties

#### Font family: the 'font-family' property
{:#font-family}

`font-family`{:.prop}

Value: | \<string\>
Initial: | undefined
Applies to: | all elements
Inherited: | yes
Percentages: | N/A

This property specifies the name of a family of fonts to be used to render sections of text descending from the element. Note that, unlike CSS, only a single font family can be specified with this property, not a comma-delimited font set.

#### Font styling: the 'font-style' and 'font-weight' properties
{:#font-style}

`font-style`{:.prop}

Value: | normal \| italic
Initial: | normal
Applies to: | all elements
Inherited: | yes
Percentages: | N/A

This property can be used to request normal or italicised versions of a font from within a font-family. Note that RCSS does not yet support oblique font styles.

`font-weight`{:.prop}
{:#font-weight}

Value: | normal \| bold \| \<number \[1,1000\]\>
Initial: | normal
Applies to: | all elements
Inherited: | yes
Percentages: | N/A

This property can be used to request normal or bolded versions of a font from within a font-family. A numeric value can be specified for more granularity on supported fonts. The range is based on the commonly used OpenType specification: 100 (Thin), 200 (Extra Light), 300 (Light), 400 (Normal), 500 (Medium), 600 (Semi Bold), 700 (Bold), 800 (Extra Bold), 900 (Black).

#### Font size: the 'font-size' property
{:#font-size}

`font-size`{:.prop}

Value: | \<length\> \| \<percentage\>
Initial: | 12px
Applies to: | all elements
Inherited: | yes
Percentages: | Font size of parent element

Values have the following meanings:

`<length>`{:.value}
: The font size is generated at the point size requested. For font-relative units (such as `em`{:.value} ), the font size is relative to the parent element's font size.

`<percentage>`{:.value}
: The font size is generated at the point size of the element's parent's font, scaled by the percentage.


#### Font shorthand
{:#font}

`font`{:.prop}

Value: | `font-style`{:.prop} `font-weight`{:.prop} `font-size`{:.prop} `font-family`{:.prop}
Initial: | See individual properties
Applies to: | all elements
Inherited: | yes
Percentages: | N/A

A shorthand property for setting all the font properties at once.


#### Font kerning: the 'font-kerning' property
{:#font-kerning}

`font-kerning`{:.prop}

Value: | auto \| normal \| none
Initial: | auto
Applies to: | all elements
Inherited: | yes
Percentages: | N/A

Values have the following meanings:

`auto`{:.value}
: Font kerning is enabled if available by default, but is disabled for small font sizes to improve the readability of text.

`normal`{:.value}
: Font kerning is always enabled if available.

`none`{:.value}
: Font kerning is disabled.

Font kerning affects how characters are spaced next to each other. Most fonts have kerning information that improves readability by making the optical spacing between characters more uniform.
