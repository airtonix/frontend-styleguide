# Elements

Elements are things inside your component.

![](images/component-elements.png)

## Naming elements
Each component may have elements. They should have classes that are a combination of the component and the element.

```scss
.search-form { /* ... */ }
  .search-form__field { /* ... */ }
  .search-form__action { /* ... */ }
```

## Element selectors
follow the BEM convention.

```scss
.article-card {
  .title     { /* ✗ bad */ }
}
.article-card {}
.article-card__author  { /* ✓ better */ }
```

## Avoid tag selectors
Use classnames whenever possible. Tag selectors are fine, but they may come at a small performance penalty and may not be as descriptive.

```scss
.article-card > h3 { /* ✗ avoid */ }

.article-card__name { /* ✓ better */ }
```

Not all elements should always look the same. Variants can help.
[Continue →](variants.md)
<!-- {p:.pull-box} -->
