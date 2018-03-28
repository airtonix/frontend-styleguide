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
follow the BEM naming convention of component name and element name joined with a double underscore.

```scss
.article-card {
  .title     { /* ✗ bad */ }
}
.article-card {}
.article-card__title  { /* ✓ better */ }
```

## Avoid tag selectors
Use classnames whenever possible. Tag selectors are fine, but they incur a large specifity cost, which makes it harder for implementation specific code to override.

```scss
.article-card > h3 { /* ✗ avoid */ }

.article-card__name { /* ✓ better */ }
```

Not all elements should always look the same. Variants can help.
[Continue →](variants.md)
<!-- {p:.pull-box} -->
