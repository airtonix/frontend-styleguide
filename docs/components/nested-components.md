Nested components
================

![](../images/component-nesting.png)

```html
<div class='article-link'>
  <div class='article-link__vote-box vote-box'>
    ...
  </div>
  <h3 class='article-link__title'>...</h3>
  <p class='article-link__meta'>...</p>
</div>
```

Sometimes it's necessary to nest components. Here are some guidelines for doing that.

### Modifiers
A component may need to appear a certain way when nested in another component. Avoid modifying the nested component by reaching into it from the containing component.

```scss
.article-header {
  > .vote-box > .up { /* ✖️ avoid this */ }
}
```

  Instead, prefer to add a variant to the nested component and apply it from the containing component.

```html
<div class='article-header'>
  <div class='article-header__vote-box vote-box vote-box--highlight'>
    <button class="vote-box__up-vote"> 👍 </button>
  </div>
  ...
</div>
```

```scss
/* ✔️ better */
.vote-box--highlight .vote-box__up-vote { /* ... */ }
```

### Avoid `@extends`
Sometimes, when nesting components, your markup can get busy:

```html
<div class='search-form'>
  <input class='search-form__input' type='text'>
  <button class='search-form__input search-button search-button--red search-button--large'></button>
</div>
```

Resist the urge to simplify this by using your CSS preprocessor's `@extend` mechanism:

```html
<div class='search-form'>
  <input class='input' type='text'> /* ✖️ bad, no classname */
  <button class='submit'></button> /* ✖️ bad, classname not defined as element of search-form */
</div>
```

```scss
/* ✖️ Bad */
.search-form {
  > .submit {
    @extend .search-button;
    @extend .search-button--red;
    @extend .search-button--large;
  }
}
```

This results in problems like:

* descision paralysis when it comes time to extend `search-button`... will changing it break `search-form`?
* Searching for `.search-button`

Moral of the story is: Don't use `@extends`.

## Element before Block

```html
<div class="header">
    <div class="header__logo logo"></div> 1️⃣
</div>
```

```scss
.logo {
    width: rem-calc(120px);
    height: rem-calc(60px);
    background: url(logo.svg);
}

.header {
    display: flex;
}
    .header__logo {
        margin-right: rem-calc(layout-get(spacing)); 2️⃣
    }
```

1. An elements class name always comes before the block class name being mixed.
2. The element selector only defines enough attributes to adjust its implementation of the logo


## Avoid minutia

When initially developing new components, avoid disecting it into as many possible smaller components.
Only use mixes when you've already got existing components that fit exactly as a mix.


```html
<div class="career-application"> 1️⃣
    <h3 class="career-application__heading">Work with us</h3> 2️⃣
    <form class="career-application__form formbuilder" 3️⃣
          action="/api/career-application"
          method="post"
          data-ajaxform> 7️⃣
        <input class="formbuilder__field" 4️⃣
               type="text"
               name="fullname"
               autocomplete="fullname"
               data-ajaxform__payload> 8️⃣
        <input class="formbuilder__field"
               type="email"
               name="email"
               autocomplete="fullname"
               data-ajaxform__payload>
        <input class="formbuilder__field"
               type="file"
               name="resume"
               data-ajaxfileupload 9️⃣
               data-ajaxfileupload--endpoint="/api/career-application-file"
               data-ajaxfileupload--allowedextensions="doc,pdf,txt,html,md"
               data-ajaxform__payload>
        <button class="formbuilder__field button button--outline" 5️⃣
                type="reset">Reset</button>
        <button class="formbuilder__field button button--primary" 6️⃣
                type="submit">Send</button>
    </form>
</div>
```

Here is a form, one that seems to indicate it might be CMS user controlled. If you look at the class names there is a total of 3 components being used here, some on the same element.

The Components numbered above are:

- 1️⃣ a `career-application` component, which has
    - 2️⃣ a heading, and a
    - 3️⃣ a form
- 4️⃣ a `formbuilder` component, which describes
    - a `formbuilder__field`, which in this simplistic example could be any form element.
- 5️⃣ a `button`, which exists as two variants `button--outline` and `button--primary` 6️⃣
- 7️⃣ a js behaviour `ajaxform` on the `formbuilder` which has
    - 8️⃣ several child markers `ajaxform__payload`, which in this case indicate field values to collate on send
- 9️⃣ a js behaviour `ajaxfileupload` with some configuration for this instance.


What about repeating elements like lists? Learn about Layouts.
[Continue →](components/layouts.md)
<!-- {p:.pull-box} -->
