# CSS structure

## One component per file
For each component, place them in their own file.

  * filename matches the component name

  ```scss
  /* ./components/search/search-form.scss */
  .search-form { /* ... */ }
    .search-form__button { /* ... */ }
    .search-form__field { /* ... */ }
    .search-form__label { /* ... */ }

    // variants
    .search-form--small { /* ... */ }
    .search-form--wide { /* ... */ }
  ```

## One media query per file
Component styles for different media queries have separate files.

  * `./components/search/search-form` : all sizes, small up **required**
  * `./components/search/search-form--smallonly`: small only
  * `./components/search/search-form--largeup`: large up
  * `./components/search/search-form--largeup-portrait`: large up, but only portrait
  * `./components/search/search-form--print`: print styles

## Use media queries at the component root level
Component screen and print media queries are bundled together in the root of the component folder:

```bash
 components/
    search/
        screen.scss
        print.scss
```

In scss it looks like:

  ```scss
  // ./components/screen.scss
  @import './search-form/screen';
  ```

In your `./component/search-form/screen.scss` file, it looks like:

```scss
@import "./search-form";

@media #{breakpoint-lte(small)} { // less than or equal to small topend
    @import "./search-form--smallonly";
}

@media #{breakpoint-gt(small)} { // greater than small topend
    @import "./search-form--mediumup";
}
```

## Print styles separate from screen styles

Components should a component level  `print.scss` file:

```scss
// ./components/search-form/print.scss
@media print {
    @import "./search-form--print";
}
```

Then if we're using a root `print.scss`:

```scss
@import "./components/search-form/search-form--print";
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
