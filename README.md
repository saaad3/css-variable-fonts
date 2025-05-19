# CSS Variable Fonts

## Quick Guide to Confusing Typography Terms

- **Typography**: The art and technique of styling text to make it readable and visually appealing. It is about choosing fonts, setting sizes, adjusting spacing etc.

- **Typeface**: Set of characters that share the same design, like Roboto.

- **Font Family**: A group of fonts with the same design (typeface) but different styles or weights (e.g., Roboto Regular, Medium, Bold).

- **Font**: A specific style and weight of a typeface (e.g., Roboto Bold).

- **OpenType**: A modern font format that stores fonts with advanced features, like different styles or weights in one file. Variable fonts are mostly available in OpenType format with .otf extension.

## Variable Fonts: Introduction

CSS variable fonts make it easy to create different design variations (weight and style) of a typeface from a single file using CSS.

With static fonts, each font/variation (weight, style, etc.) is provided in a separate file. This means the browser has to download a file for each of the font used, resulting in multiple HTTP requests, which is a major performance issue. This is where variable fonts offer a significant advantage.

**With variable fonts, different variations of a typeface are incorporated into a single file rather than having a separate font file for every width, weight, or style and use CSS to vary the appearance of a typeface to achieve the desired output.**

**Note:** Some variable fonts _(almost all google fonts)_ come in two files: one for roman (upright / regular) with all its variations, and one for italic variations. This is sometimes done to reduce the overall file size in cases where the italics aren't needed.

## Variation Axis

Variable fonts have **variation axes**, each of which is a numeric range describing a specific aspect of the typeface design. For example, the “weight” axis controls how light or how bold the letterforms can be, and the “width” axis controls how narrow or how wide they can be. A Variable font may have single variation axis or any number of, it entirely depends on typeface designer. An axis can be a range or a binary value. Weight might range from 1–1000, whereas italic might be 0 or 1 (off or on).

There are five registered variation axes in the OpenType specification which are frequent and common enough that authors of specification felt they are worth standardizing.

The five registered variation axis are:

1. Weight
2. Width
3. Italic
4. Slant
5. Optical Size

Additionally, typeface designers may provide custom variation axes to customise other aspects of the font.

**The variation axis are identifed by 4 letter words tag. The standard variation axis are identified by lowercase letters and custom are with uppercase letters.**

To see what axis a variable font has and what the value ranges are for each axis, there are two options:

1. See the documentation of variable font.
2. The most suitable and practical way is to check the font editor in firefox developer tools which shows all standard and custom variation axis along with the range that can be used. It is really great tool to play with different variation axis.

## CSS properties for Registered axes

The W3C has undertaken to map Registered variation axis to standard CSS properties, so we use them to set different variation axes.

- Weight axis -----> `font-weight`
- Width axis -----> `font-stretch`
- Italic axis -----> `font-style`
- Slant axis -----> `font-style`: oblique + angle
- Optical size axis -----> `font-optical-sizing`

There is also another lower-level syntax.

> The lower-level syntax (`font-variation-settings`) was the first mechanism implemented to test the early implementations of variable font support and is necessary to utilize new or custom axes beyond the five registered ones. However, the W3C's intent was for this syntax not to be used when other attributes are available. Therefore wherever possible, the appropriate property should be used, with the lower-level syntax of `font-variation-settings` only being used to set values or axes not available otherwise.  
> — _MDN Web Docs_

When using `font-variation-settings` it is important to note that axis names are case-sensitive (lowercase for registered and uppercase for tags of custom variation axes). This property takes comma-separated value pairs where each axis is referenced using a 4-letter tag in quotes (single or double), and the corresponding value is provided as an integer with no units. With this property, we can set values for all the available variation axes in single line.

```css
body {
  font-family: Amstelvar;
  font-variation-settings: 'wght' 360, 'wdth' 90, 'opsz' 14, 'YOPQ' 124;
}
```

## Registered Variation Axis

### Weight

Describes how light or bold the letterforms can be. Its value range is 1–1000, but typeface designers typically restrict it to a smaller range of 100–900.

It is represented by the **wght** tag.

Though, we can use keywords like normal for 400 and bold for 700 with variable fonts like we do with static fonts, but for custom weights like 375, we need to use numbers instead of keywords as we can't map these keywords to custom weights.

```css
font-weight: 375;
/* Or */
font-variation-settings: 'wght' 375;
```

### Width

Describes how narrow or wide or condensed or extended the letterforms can be. It takes any value from 0 to up and the normal/default value is 100(100%).

It is represented by the **wdth** tag.

This variation axis doesn't merely stretches the typeface in case of variable fonts _(for static fonts, it just stretches the font)_, it actually changes the typeface to take up more or less space along the width axis. It varies the display by either condensing or extending it.

If a value is outside the range encoded in the font, the browser should render the font at the closest value allowed.

```css
font-stretch: 115%;
/* Or */
font-variation-settings: 'wdth' 115;
```

### Slant

Changes the angle or obliqueness of the letterform according to the value provided. The similar effect we get when we set `font-style` to obqliue. It's not italicizing the font but slanting it to create different visual presentation.

It is represented by the **slnt** tag.

The normal or upright slant value is typically 0 (0 degress) with allowed values ranging from -90 degress to +90 degress.

```css
font-style: oblique 6deg;
/* Or */
font-variation-settings: 'slnt' -10;
```

### Italic

In contrast with Slant which sets the angle of typeface, Italic design typically provides a different design for letterforms like A, G, F etc. so we may see substitutions of some characters when transitioning from upright to italic

It can be set in the range of 0-1, where 0 specifies not-italic, 0.5 specifies halfway-italic and 1 specifies fully-italic.

It is represented by the **ital** tag.

> In CSS, both italic and oblique are applied to text using the font-style property. Also note the introduction of font-synthesis: none; — which will prevent browsers from accidentally applying the variation axis and a synthesized italic. This can be used to prevent faux-bolding as well.
> — _MDN Web Docs_

```css
font-style: italic;
/* Or */
font-variation-settings: 'ital' 0;
font-synthesis: none;
```

### Optical Size

Refers to how the font looks at different sizes to make it easier to read. It is done by varying the overall stroke thickness of letterforms based on physical size. If the font size is very small (like 10 or 12px), the characters would have an overall thicker stroke, and perhaps other small modifications to ensure that it would reproduce and be readable at a physically smaller size. Conversely, when a much larger size was being used (like 48 or 60px), there might be much greater variation in thick and thin stroke weights, showing the typeface design more in line with the original intent.

Optical size ensures that text is clear and readable, whether it's tiny or very large.

It is represented by the **opsz** tag.

Normally optical sizing is handled automatically by the browser but with some variable fonts, we can override automatic behaviour in two ways:

1. We can toggle this automatic behaviour on and off by using the new `font-optical-sizing` and setting it to either **auto** which is a default behaviour or **none** which turns the default behaviour off which will make all font sizes use the font’s default design.




2. We can provide custom value for optical size using `font-variation-settings` property and setting the optical sizing, 'opsz' to whatever number want. The lower the number, the smaller the font is perceived to be so thicker the font strokes get.

```css
font-optical-sizing: auto;
/* Or */
font-optical-sizing: none;
/* Or */
font-variation-settings: "opsz" 36;
```

> However when using font-variation-settings: 'opsz' <num>, you can supply a numeric value. In most cases you would want to match the font-size (the physical size the type is being rendered) with the opsz value (which is how optical sizing is intended to be applied when using auto). The option to provide a specific value is provided so that should it be necessary to override the default — for legibility, aesthetic, or some other reason — a specific value can be applied. _MDN Web Docs_


## Custom Variation Axis

### Grade

> The term 'grade' refers to the relative weight or density of the typeface design, but differs from traditional 'weight' in that the physical space the text occupies does not change, so changing the text grade doesn't change the overall layout of the text or elements around it. This makes grade a useful axis of variation as it can be varied or animated without causing a reflow of the text itself. — _MDN Web Docs_

It is represented by the **GRAD** tag.

```css
font-variation-settings: 'GRAD' 50;
```

## Adding and Using Variable Fonts

Adding variable fonts to a project is done in the same way as adding static fonts. The main difference when adding variable fonts is that there are fewer font weights to include.

Assuming that we have downloaded the variable font files, we can add them to project using the `@font-face` at-rule just like we add other web fonts. For example, in the code below, we have two `@font-face` at-rules, one for the normal font style and one for the italic font style.

Inside the block, we begin with the `font-family` property, where we assign a name to the font to reference it later in the code. Following that, we set the allowable ranges for different variation axes using CSS properties.

```css
@font-face {
  font-family: Amstelvar;
  src: url(./fonts/Amstelvar-Roman-VF.ttf) format('truetype');
  font-weight: 100 900;
  font-style: normal;
  font-stretch: 25% 140%;
  font-display: swap;
}
@font-face {
  font-family: Amstelvar;
  src: url(./fonts/Amstelvar-Italic-VF.ttf) format('truetype');
  font-weight: 100 900;
  font-style: italic;
  font-stretch: 25% 140%;
  font-display: swap;
}
body {
  font-family: Amstelvar;
  font-style: normal;
  font-stretch: 100%;
}
h1,
h2,
h3,
h4,
h5,
h6 {
  font-weight: 600;
}
strong {
  font-weight: 500;
}
i,
em {
  font-weight: 300;
  font-style: italic;
  font-stretch: 125%;
}
```

If a variable font includes both Roman and italic variations in a single file, the value of `font-style` inside the @font-face block should be:

```css
font-style: normal italic;
```

Otherwise, the browser won’t use the real italic design from the font file (even it's present). It will just tilt the regular style instead.

## Providing font fallback support

Even though the support for variable fonts is pretty good in all modern browsers however, it is necessary to provide a proper font fallback support for the older browsers. MDN documentation on variable fonts recommends using `@support` query _(feature query block)_ to test browser support for variable fonts. Using this approach, we first declare styles for older browsers and then using @support for modern browsers.

```css
h1 {
  font-family: some-static-font;
}
@supports (font-variation-settings: 'wdth' 115) {
  h1 {
    font-family: some-variable-font;
  }
}
```

We first write styles for all older browsers and builds on top of it using `@support` as if browser supports variable fonts, it will run the code inside `@support` query. 


## File Size of Variable Fonts

As variable fonts include all the configuration for variations in one file usually. That file would be larger than a file of single font, but in most cases smaller or about the same size as the 4 fonts (regular, italic, bold, and bold italic) we might load for body copy. The advantage in choosing the variable fonts is that we have access to the entire range of weights, widths, and styles available, rather than being constrained to those we loaded.

For more details and detailed documentation, please check [mdn](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_fonts/Variable_fonts_guide).
