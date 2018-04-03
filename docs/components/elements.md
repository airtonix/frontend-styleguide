Elements
==========

Elements are things inside your component.

![](../images/component-elements.png)

### Naming elements
Each component may have elements. They should have classes that are a combination of the component and the element.

```scss
.search-form { /* ... */ }
  .search-form__field { /* ... */ }
  .search-form__action { /* ... */ }
```

### Element selectors
follow the BEM naming convention of component name and element name joined with a double underscore.

```scss
.article-card {
  .title     { /* ✖️ bad */ }
}
```

* complicates the specifity weight
* ambigous use of `.title`, nesting components here could result in unintended application of styles.

```scss
.article-card {
  &__title     { /* ✖️ bad */ }
}
```

* removes the one to one parity between code in your editor and code in the browsers dev tools.


```scss
.article-card {}
.article-card__title  { /* ✔️ better */ }
```

### Avoid tag selectors
Use classnames whenever possible. Tag selectors are fine, but they incur a large specifity cost, which makes it harder for implementation specific code to override.

```scss
.article-card > h3 { /* ✖️ avoid */ }

.article-card__name { /* ✔️ better */ }
```

### Elements class names
Despite the urge to do so, element class names should not be a representation of their position in the HTML Document Object Model.

```scss
/* ✖️ bad */
.article-card {}
.article-card__header {}
.article-card__header__title {}
.article-card__header__author {}
.article-card__header__timestamp {}
```

Instead element class names merely describe what component they belong to. (without raising the [css specifity score](https://www.google.com.au/search?q=css+specifity+score)).

```scss
/* ✔️ better */
.article-card {}
  .article-card__header {}
    .article-card__title {}
    .article-card__author {}
    .article-card__timestamp {}
```

Even nested components can themselves be elements of the containing component. In [BEM, this is called a mixin](components/mixins.md).

```html
<div class="article-card">
  <img class="article-card__thumbnail" src="...">
  <div class="article-card__header">
    <h3 class="article-card__title"> <!-- ... //--> </div>
    <span class="article-card__author"> <!-- ... //--> </div>
    <div class="article-card__timestamp"> <!-- ... //--> </div>
    <a class="c-button c-button--primary article-card__link-button"
       href="..."><!-- ... //--></a>
  </div>
</div>
```

```scss
// @core/components/buttons/button.scss
.c-button { /* ... */ }
.c-button--primary { /* ... */ }
```

```scss
// @site__abc123/components/articles/article-card.scss
.article-card {}
.article-card__link-button {
  flex-grow: 0;
  align-self: flex-end;
}
```

## Avoid describing the design in HTML

```html
```

```html
/* ✖️bad */
<div class="layout"> /* 🅰️ */
    <div class="row"> /* 1️⃣ */
        <div class="layout-content">  /* 🅱️ */
            <div class="large-12 columns layout-cell"> /* 2️⃣ */
                ...
            </div>
        </div>
    </div>
</div>
```

```html
/* ✔️ Better */
<div class="layout"> /* 1️⃣ */
    <div class="layout__container">

    </div>
</div>
```

**What's wrong?***

 * point one.
 * point one.
 * point one.
 * point one.


Not all elements should always look the same. There can be variations, Modiers can help.
[Continue →](components/modifiers.md)
<!-- {p:.pull-box} -->
