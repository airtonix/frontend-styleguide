# CSS structure

## One component per file
For each component, place them in their own file.

  ```scss
  /* css/components/search-form.scss */
  .search-form { /* ... */ }
    .search-form__button { /* ... */ }
    .search-form__field { /* ... */ }
    .search-form__label { /* ... */ }

    // variants
    .search-form--small { /* ... */ }
    .search-form--wide { /* ... */ }
  ```

## Use media queries at the component root level
In scss it looks like:

  ```scss
  @import 'component-name/screen';
  ```

In your `screen.scss` file, it looks like:

```scss
@import "./component";

@media #{breakpoint-gt(small)} {
    @import "./component--mediumup";
}

```

## Print styles separate from screen styles

Components should a component level  `print.scss` file:

```scss
@media print {
    @import "./component--print";
}
```

Then if we're using a root `print.scss`:

```scss
@import "./components/component-name/component--print";
```

Otherwise we'll lazyload the print style with either `System.import` or using it as an entrypoint.

## Avoid over-nesting
Use no more than 1 level of nesting. It's easy to get lost with too much nesting.

  ```scss
  /* ✗ Avoid: 3 levels of nesting */
  .image-frame {
    > .description {
      /* ... */

      > .icon {
        /* ... */
      }
    }
  }

  /* ✓ Better: 2 levels */
  .image-frame {
    .image-frame__description { /* ... */ }
    .image-frame__description-icon { /* ... */ }
  }

  /* ✓ Idea: 1 level */
  .image-frame { /* ... */ }
    .image-frame__description { /* ... */ }
    .image-frame__description-icon { /* ... */ }
  ```
