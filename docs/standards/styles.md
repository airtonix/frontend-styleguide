Scss Code Styleguide
===================

Here is our comprehensive styleguide for scss.

## Table of Contents

1. [Terminology](#terminology)
    - [Rule Declaration](#rule-declaration)
    - [Selectors](#selectors)
    - [Properties](#properties)
    - 🚧 Specificity
1. [CSS](#css)
    - [Formatting](#formatting)
    - [Comments](#comments)
    - [OOCSS and BEM](#oocss-and-bem)
    - [ID Selectors](#id-selectors)
    - [JavaScript hooks](#javascript-hooks)
    - [Border](#border)
1. [Sass](#sass)
    - [Syntax](#syntax)
    - [Ordering](#ordering-of-property-declarations)
    - [Variables](#variables)
    - [Mixins](#mixins)
    - [Extend directive](#extend-directive)
    - [Nested selectors](#nested-selectors)
    - [Settings](#settings)
1. [File Structure](#file-structre)
1. [Translation](#translation)
1. [License](#license)


## Terminology

### Rule declaration

A “rule declaration” is the name given to a selector (or a group of selectors) with an accompanying group of properties. Here's an example:

```css
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

### Selectors

In a rule declaration, “selectors” are the bits that determine which elements in the DOM tree will be styled by the defined properties. Selectors can match HTML elements, as well as an element's class, ID, or any of its attributes. Here are some examples of selectors:

```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### Properties

Finally, properties are what give the selected elements of a rule declaration their style. Properties are key-value pairs, and a rule declaration can contain one or more property declarations. Property declarations look like this:

```css
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

### Formatting

* Use soft tabs (2 spaces) for indentation
* Prefer dashes over camelCasing in class names.
  - Underscores and PascalCasing are okay if you are using BEM (see [OOCSS and BEM](#oocss-and-bem) below).
* Do not use ID selectors
* When using multiple selectors in a rule declaration, give each selector its own line.
* Put a space before the opening brace `{` in rule declarations
* In properties, put a space after, but not before, the `:` character.
* Put closing braces `}` of rule declarations on a new line
* Put blank lines between rule declarations

```css
/* ✖️ bad */
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

```css
/* ✔️ Good */
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Comments

* Comments are special in Scss, because we use KSS.
* Inside rule-declaration, prefer line comments (`//` in Sass-land) to block comments.
* Prefer comments on their own line. Avoid end-of-line comments.
* The root component file should have a KSS comment block with the following:
  - Component Name 1️⃣
  - Description 2️⃣
  - Example markup 3️⃣
  - list of available component modifiers 4️⃣
  - Compatibility or browser-specific hacks 5️⃣
  - a Kss `Styleguide <reference.path>` 6️⃣

```scss
/*
Article Card 1️⃣

Displays an article summary in card form... 2️⃣
- More description here to outline component requirements and user stories

Markup: 3️⃣
<div class="article-card {{default classes modifier_class}}">
  <img class="article-card__thumbnail" src="...">
  <div class="article-card__header">
    <h3 class="article-card__title"> <!-- ... //--> </div>
    <span class="article-card__author"> <!-- ... //--> </div>
    <div class="article-card__timestamp"> <!-- ... //--> </div>
    <a class="c-button c-button--primary article-card__link-button"
       href="..."><!-- ... //--></a>
  </div>
</div>

- .article-card--slim - Slim version of the card 4️⃣
- .article-card--horizontal - Arranged horizontally

Experimental: yes 5️⃣
Deprecated: no

Styleguide ABC123.Components.Articles.ArticleCard 6️⃣
*/
```



### BEM

We encourage BEM for these reasons:

  * It helps create clear, strict relationships between CSS and HTML
  * It helps us create reusable, composable components
  * It allows for less nesting and lower specificity
  * It helps in building scalable stylesheets


**BEM**, or “Block-Element-Modifier”, is a _naming convention_ for classes in HTML and CSS. It was originally developed by Yandex with large codebases and scalability in mind, and can serve as a solid set of guidelines for implementing OOCSS.

  * CSS Trick's [BEM 101](https://css-tricks.com/bem-101/)
  * http://getbem.com/introduction/
  * Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

We use a variant of BEM with double dashed *modifiers*. Underscores and dashes are still used for modifiers and children.

### ID selectors

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.


### Border

Use `0` instead of `none` to specify that a style has no border.

```css
/* ✖️ bad */
.foo {
  border: none;
}
```

```css
/* ✔️ Good */
.foo {
  border: 0;
}
```

### Variables

Prefer BEM naming for variable names (e.g. `$settings__fonts--large`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`).

### Mixins

Avoid using Mixins *as much as you can*. They hide complexity quickly leading to bloat and lack of understanding about what is happening. They also make it hard to track down code "in effect".

### Extend directive

`@extend` should be avoided because it has unintuitive and potentially dangerous behavior, especially when used with nested selectors. Even extending top-level placeholder selectors can cause problems if the order of selectors ends up changing later (e.g. if they are in other files and the order the files are loaded shifts). Gzipping should handle most of the savings you would have gained by using `@extend`.

### Nesting selectors

!> **Do not** nest selectors more than three levels deep!

?> **Ideally** only have one level.

```scss
/* ❗️ bad */
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

When selectors become this long, you're likely writing CSS that is:

* Strongly coupled to the HTML (fragile) *—OR—*
* Overly specific (powerful) *—OR—*
* Not reusable

```scss
.page-container {
  #thing-content { /* ❗️ bad */
  }
}
```

!> Again: **never nest ID selectors!**

If you must use an ID selector in the first place (and you should really try not to), they should never be nested. If you find yourself doing this, you need to revisit your markup, or figure out why such strong specificity is needed. If you are writing well formed HTML and CSS, you should **never** need to do this.

### Settings
*Colour Pallete*, *Media Query Aliases*, *Typeface Styles* and Common *Layout Dimensions* are all described as setting maps. They each come with their associated function for pulling values out.

Each settings map is *almost* always a multi-dimensional map. An example would be:

```scss
$settings__colours: (
    blue: (
        mostlight: lighten(#00f, 20%),
        lightest: lighten(#00f, 15%),
        lighter: lighten(#00f, 12%),
        light: lighten(#00f, 10%),
        default: #00f
        dark: darken(#00f, 10%),
        darker: darken(#00f, 12%),
        darkest: darken(#00f, 15%),
        mostdark: darken(#00f, 20%),
    )
);
```
This example works in conjuction with the `colour-get` function.

```scss
.thing {
  color: colour-get(blue, darker);
  background-color: colour-get(blue);
}
```

?> 💭 If your variant names go beyond *darkest* and *lightest*, then you may need to work with your designer to move some variants into a new colour name.


#### Colours

?> **👁‍🗨 Needs Peer Review**

  * The variable is always called `$settings__colours`.
  * It is always a multidimensional key value map.
  * The first level keys are always a colour name *blue*, *stonegrey*, *maroon*, etc.
  * nested under each colour name is the various shades of that colour.
  * It is perferred that the colours be described as hex values, as this removes any chance that modifying a value will affect others. (*principle of least surprise*).
  * Every colour has a `default` key, this is the entry returned by the `colour-get` function when no variant is supplied.
  * Variant naming follows the a luminosity relevance pattern, so variants lighter go above `default` in order of `light`, `lighter`, `lightest`, and maybe even `solightmuchbright`. Emphasis on describing luminosity relevance between default and other variants.

#### Fonts

?> 🚧 **Todo**

#### Layout

Mostly defines things like spacings

?> 🚧 **Todo**

### File Structure

?> 🚧 **Todo**

Site themes exist separately from core like so:

```bash
client/
    src/
        core/
        site__abc123/  /* ✔️ your jira project code and all its frontend files here */
            components/
                ...
            icons/
                ...
            images/
                ...
            fonts/
                ...
            sprites/ :
                ...
        construct/
            settings/ 1️⃣
            functions/ 2️⃣

```

* **1️⃣** Project settings**  extends `@core/construct/settings` with `map-merge`. All settings are exposed as maps.
* **2️⃣** Project functions**  Usually you won't need to create any, but they should override `@core/construct/functions` and be imported after settings.
