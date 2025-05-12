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

**Note:** Some variable fonts _(almost all google fonts)_ come in two files: one for roman (regular) with all its variations, and one for italic variations. This is sometimes done to reduce the overall file size in cases where the italics aren't needed.


## Variation Axis

Variable fonts have **variation axes**, each of which is a numeric range describing a specific aspect of the typeface design. For example, the “weight” axis controls how light or how bold the letterforms can be, and the “width” axis controls how narrow or how wide they can be. An axis can be a range or a binary value. Weight might range from 1–1000, whereas italic might be 0 or 1 (off or on).

There are five registered variation axes in the OpenType specification which are frequent and common enough that authors of specification felt they are worth standardizing.

The five registered variation axis are:
1. Weight
2. Width
3. Italic
4. Slant
5. Optical Size

Additionally, typeface designers may provide custom variation axes to customise other aspects of the font, such as x-height/

**The variation axis are identifed by 4 letter words tag. The standard variation axis are identified by lowercase letters and custom are with uppercase letters.**

To see what axis a variable font has and what the value ranges are for each axis, there are two options:

1. See the documentation of variable font.
2. The most suitable and practical way is to check the font editor in firefox developer tools which shows all standard and custom variation axis along with the range that can be used. It is really great tool to play with different variation axis.


## CSS properties for Registered axes

The W3C has undertaken to map Registered variation axis to standard CSS properties, so we use them to set different variation axes. This can be really useful if we want to serve a fallback static font because the fallback font will understand the values and map to whatever is the closest available value for the static font.

- Weight axis -----> `font-weight`
- Width axis -----> `font-stretch`
- Italic axis -----> `font-style`
- Slant axis -----> `font-style`: oblique + angle
- Optical size axis -----> `font-optical-sizing`

There is also another lower-level syntax.

> The lower-level syntax (`font-variation-settings`) was the first mechanism implemented to test the early implementations of variable font support and is necessary to utilize new or custom axes beyond the five registered ones. However, the W3C's intent was for this syntax not to be used when other attributes are available. Therefore wherever possible, the appropriate property should be used, with the lower-level syntax of `font-variation-settings` only being used to set values or axes not available otherwise.  
> — _MDN Web Docs_

When using `font-variation-settings` it is important to note that axis names are case-sensitive (lowercase for registered uppercase for tags of custom variation axes). This property takes comma-separated value pairs where each axis is referenced using a 4-letter tag in quotes, and the corresponding value is provided as an integer with no units. With this property, we can set values for all the available variation axes in single line.

```css
body {
  font-family: Amstelvar;
  font-variation-settings: 'wght' 360, 'wdth' 90, 'opsz' 14, 'YOPQ' 124;
}
```


## Registered Variation Axis

- **Weight**: Describes how light or bold the letterforms can be. Its value range is 1–1000, but typeface designers typically restrict it to a smaller range of 100–900. It is represented by the `wght` tag.

We can use keywords like **normal** for (400) and **bold** (700) with variable fonts like we do with static fonts, but for custom weights like 375, we need to use numbers instead of keywords as we can't map these keywords to custom weights.

```css
font-weight: 375;
font-variation-settings: 'wght' 375;
```

- **Width**: Describes how narrow or wide or condensed or extended the letterforms can be. It takes any value from 0 and up and the normal/default value is 100(100%).

- **Italic**

- **Slant**

- **Optical size**







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

## Providing font fallback support

Even though the support for variable fonts is pretty good in all modern browsers however, it is necessary to provide a proper font fallback support for the older browsers. MDN documentation on variable fonts recommends using `@support` query to test browser support for variable fonts. Using this approach, we first declare styles for older browsers and then using @support query to test for the availability of variable fonts feature.

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

If browser supports variable fonts, it will run the code inside `@support` query. This way we first write styles for all older browsers and builds on top of it using `@support`.

However, this is quite lot of work where we are literally writing two sets of styles, one for older browsers, one for modern browsers. The second solution is bit sneaky where it takes the advantage of similarities in browser support for variable fonts and CSS custom properties / CSS variables. The browser support for both of them is pretty much same means the browser which supports CSS properties supports variable fonts as well and vice versa.

So, using this approach, we first create CSS variable and it's value is the name of variable font. Then inside the body tag, we declare `font-family` property twice. The first declaration here works as the fallback where we can have any font we want to provide as fallback and second declaration is where we specify variable font. The browser without support for custom properties will simply ignore the second declaration but if it supports, the second declaration will override first declaration and we get to use variable fonts. Without writing lot of extra code and any queries, this is lot cleaner way to do it but it requires thorough testing on different devices so make sure it works properly.

```css
:root {
  --font-family: Amstelvar;
}
body {
  font-family: Arial, Helvetica, sans-serif;
  font-family: var(--font-family);
}
```

This approach can also be used with `font-variation-settings` as well. We can declare `font-weight`, `font-width` and `font-style` properties higher up in hierarchy and then provide `font-variation-settings` at the end of rule and that way the standard weight, width, style properties will be applied first and if browser supports `font-variation-settings` property, it will overwrite them.

## File Size of Variable Fonts

As variable fonts include all the configuration for variations in one file usually. That file would be larger than a file of single font, but in most cases smaller or about the same size as the 4 fonts (regular, italic, bold, and bold italic) we might load for body copy. The advantage in choosing the variable fonts is that we have access to the entire range of weights, widths, and styles available, rather than being constrained to those we loaded.

For more details and detailed documentation, please check [mdn](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_fonts/Variable_fonts_guide).