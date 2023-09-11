# CSS Variable Fonts

CSS variable fonts make it easy to create different design variations (weight and style) of a typeface/font from a single file using CSS.

With static fonts, each of the font variations (weight and style) are provided in a dedicated file, so the browser has to download files for each variation used in the project, which is a huge performance issue. Variable fonts come to the rescue!

With variable fonts, CSS is used to vary the appearance of variable fonts to produce the output we want. Variable fonts have variation axes that are use to design or change the appearance of the font. Variation axes represent specified aspects of typeface design, such as weight, width, slant, etc. There are 5 registered variation axes: weight, width, italic, slant, optical size. Additionally, typeface designers may provide custom variation axes to change any aspect of the font, such as height, etc.


## Adding and Using Variable Fonts

Adding variable fonts to a project is done in the same way as adding static fonts. The main difference when adding variable fonts is that there are fewer font weights to include.

Assuming that we have downloaded the variable font files, we can add them to project using the `@font-face` declaration just like we add other custom fonts. For example, in the code below, we have two `@font-face` declarations, one for the normal font style and one for the italic font style.

We begin with the `font-family` property, where we assign a name to the font to reference it later in the code. Following that, we specify other properties to set the range for different axes.

```css
@font-face {
    font-family: Amstelvar; 
    font-weight: 100 900;
    font-style: normal;
    font-stretch: 25% 140%;
    src: url(./fonts/Amstelvar-Roman-VF.ttf) format('truetype');
    font-display: swap;
}

@font-face {
    font-family: Amstelvar; 
    font-weight: 100 900;
    font-style: italic;
    font-stretch: 25% 140%;
    src: url(./fonts/Amstelvar-Italic-VF.ttf);
    font-display: swap;
}

body {
    font-family: Amstelvar;
}
```

## Using variable fonts

Some of the registered variation axes map to standard CSS properties, so we can use them to set different variation axes. This can be really useful if we want to serve a fallback font because the fallback font will understand this value and map it to whatever is the closest available value for the static font. For example:

- `weight` axis -----> `font-weight`
- `width` axis  -----> `font-stretch`

However, the better way to set values for variation axes is by using the `font-variation-settings` property because we are likely to run into inconsistent behavior across different browsers with standard CSS properties. Using the `font-variation-settings` property allows us to set values for all the available variation axes using one property in a single line.

This property takes comma-separated value pairs where each axis is referenced using a 4-letter tag in quotes, and the corresponding value is provided as an integer. Use lowercase tags for registered variation axes and uppercase for custom variation axes. For example:

```css
body {
    font-family: Amstelvar;
    font-variation-settings: 'wght' 360, 'wdth' 90, 'opsz' 14, 'YOPQ' 124;
}
```

To see what axis a variable font has and what the value ranges are for each axis, there are two options:
1. See the documentation of variable fonts.
2. The most suitable and practical way is to check the font editor in firefox developer tools which shows all standard and custom variation axis along with the range that can be used. It is really great tool to play with different variation axis.


## Providing font fallback support

Even though the support for variable fonts is pretty good in all modern browsers however, it is necessary to provide proper font fallback support for the older browsers. MDN documentation on variable fonts recommends using `@support` query to test browser support for variable fonts. Using this approach, we first declare styles for older browsers and then using @support query to test for the availability of variable fonts feature.

```css
h1 {
    font-family: some-static-font-family;
}

@supports (font-variation-settings: 'wdth' 115) {
    h1 {
        font-family: some-variable-font-family;
    }
}
```

If browser supports variable fonts, it will run the code inside `@support` query. This way we first write styles for all browsers and builds on top of it using `@support`.

However, this is quite lot of work where we are literally writing two sets of styles, one for older browsers, one for modern browsers. The second solution is bit sneaky where it takes the advantage of similarities in browser support for variable fonts and CSS custom properties / CSS variables. The browser support for both of them is pretty much same means the browser which supports CSS properties supports variable fonts as well and vice versa.

So, using this approach, we first create CSS variable and it's value is the name of variable font. Then inside the body tag, we declare `font-family` property twice. The first declaration here works as the fallback where we can have any font we want to provide as fallback and second declaration is where we specify variable font. The browser without support for custom properties will simply ignore the second declaration but if it supports, the second declaration will override first declaration and we get to use variable fonts. Without writing lot of extra code and any queries, this is lot cleaner way to do it but it requires thorough testing on different devices so make sure it it works. 

```css
:root {
    --font-family: Amstelvar;
}

body {
    font-family: Arial, Helvetica, sans-serif;
    font-family: var(--font-family);
}
```

This approach can also be used `font-variation-settings` as well. We can declare `font-weight`, `font-width` and `font-style` properties higher up in hierarchy and then provide `font-variation-settings` at the end of rule and that way the standard weight, width, style properties will be applied first and if browser supports `font-variation-settings` property, it will use that.

For more details and detailed documentation, please check [mdn](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_fonts/Variable_fonts_guide).


## Credits

This project uses content and concepts from the "CSS Variable Fonts" course on LinkedIn Learning by [Morten Rand-Hendriksen](https://www.linkedin.com/learning/instructors/morten-rand-hendriksen) ([course link](https://www.linkedin.com/learning/css-variable-fonts)).

---
