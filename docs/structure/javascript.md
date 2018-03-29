Javascript
==========


### Think in component behaviors

Think that a piece of JavaScript code to will only affect 1 "component", that is, a section in the DOM.

Their files are "behaviors": code to describe dynamic JS behavior to affect a block of static HTML. In this example, the JS behavior `collapsible-nav` only affects a certain DOM subtree, and is placed on its own file.

```html
<!-- Component -->
<div class='main-navbar' data-js-collapsible-nav>
  <button class='expand' data-js-expand>Expand</button>

  <a href='/'>Home</a>
  <ul>...</ul>
</div>
```

?> **note** We don't actually have a behaviour called collapsiable-nav. it's just an example. To discover what is available in core checkout [http://core.patterns.fusion.one](our core component library).

```js
/* Behavior - behaviors/collapsible-nav.js */

$(function () {
  var $nav = $('[data-js-collapsible-nav]')
  if (!$nav.length) return

  $nav
    .on('click', '[data-js-expand]', function () {
      $nav.addClass('-expanded')
    })
    .on('mouseout', function () {
      $nav.removeClass('-expanded')
    })
})
```

### One component per file

Each file should a self-contained piece of code that only affects a *single* element type.

Keep them in your project's `behaviors/` path. Name these files according to the `data-js-___` names ([see below](#use-a-data-attribute)) or class names they affect.

```
└── javascripts/
    └── behaviors/
        ├── collapsible-nav.js
        ├── avatar-hover.js
        ├── popup-dialog.js
        └── notification.js
```

### Load components in all pages

Your main .js file should be a concatenation of all your `behaviors`.

It should be safe to load all behaviors for all pages. Since your behaviors are localized to their respective components, they will not have any effect unless the element it applies to is on the page.

See [loading component files](#loading-component-files) for guides on how to do this for your project.

### Use a data attribute

It's preferred to mark your component with a `data-js-___` attribute.

You can use ID's and classes, but this can be confusing since it isn't obvious which class names are for styles and which have JS behaviors bound to them. This applies to elements inside the component too, such as buttons (see collapsible-nav example).

```html
<!-- ✗ Avoid using class names -->
<div class='user-info'>...</div>
$('.user-info').on('hover', function () { ... })
```

```html
<!-- ✓ Better to use data-js attributes -->
<div class='user-info' data-js-avatar-popup>...</div>
$('[data-js-avatar-popup]').on('hover', function () { ... })
```

You can also provide values for these attributes.

```html
<!-- ✓ Attributes with values -->
<button data-js-tooltip='Close'>...</button>
$('[data-js-tooltip]').on('hover', function () { ... })
```

### No inline scripts

Avoid adding JavaScript thats inlined inside your HTML markup. This includes `<script>...</script>` blocks and `onclick='...'` event handlers.

By putting imperative logic outside your `.js` files (eg, JavaScript in your `.html`), it makes your application harder to test and they pose a significant maintenance burden. Keep all your _imperative_ code in your JS files, and your _declarative_ code in your HTML.

```html
<!-- ✗ Avoid -->
<script>
  $('button').on('click', function () { ... })
</script>
```

```html
<!-- ✗ Avoid -->
<body onload="loadup()">
```

Prefer to use JS behaviors instead of inline scripts.

### Bootstrap data with meta tags

A common pattern is to use inline `<script>` tags to leave data that scripts will pick up later on. In the spirit of [avoiding inline scripts](#no-inline-scripts), these patterns should  also be avoided.

```js
<!-- ✗ Avoid -->
<script>
window.UserData = { email: 'john@gmail.com', id: 9283 }
</script>
```

If they're going to be used in a component, use the beahviours data attribute syntax to declare the component level configuration.

```html
<!-- ✓ Used by the user-info behavior -->
<div class='user-info'
     data-js-user-info
     data-js-user-info--email="john@gmail.com"
     data-js-user-info--id="9283">
```

If multiple components are going to use this data, put it in a meta tag in the `<head>` section.

```html
<head>
  ...
  <!-- option 1 -->
  <meta property="app:user_data" content='{"email":"john@gmail.com","id":9283}'>

  <!-- option 2 -->
  <meta property="app:user_data:email" content="john@gmail.com">
  <meta property="app:user_data:id" content="9283">
```

This keeps your HTML files declarative, and your JS files imperative. (Also see: [Imperative vs. Declarative](http://latentflip.com/imperative-vs-declarative) from latentflip.com)

```js
function getMeta (name) {
  return $('meta[property=' + JSON.stringify(name) + ']').attr('content')
}

getMeta('app:user_data:email')  // => 'john@gmail.com'
```

### Writing code

These are conventions that can be handled by other libraries. For straight jQuery however, here are some guidelines on how to write behaviors.

### Consider using onmount

[onmount] is a library that allows you to write safe, idempotent, reusable and testable behaviors. It makes your JavaScript code compatible with Turbolinks, and it'd allow you to write better unit tests.

```js
$.onmount('[data-js-push-button]', function () {
  new JsBehaviourComponent($(this));
})
```

## Writing code without onmount

The [onmount] library solves many pitfalls when writing component behaviors. If you choose not to use it, however, be prepared to deal with those caveats on your own. This next section describes these cases in detail.

### Use document.ready

Add your events and initialization code under the [document.ready] handler. This assures you that the element you're binding behaviors to already exists. (There is an exception to this--see the next section).

```js
$(function() {
  $('[data-js-expanding-menu]').on('hover', function () {
    // ...
  })
})
```

If you're using [onmount], just run `onmount()` on document.ready as its documentation describes it.

### Use event delegation

The only time document.ready is not necessary is when attaching event delegations to `document`. These are typically best done _before_ DOM ready, which would allow them to work even when the entire document hasn't loaded completely.

This allows you to listen for those events whether the element is on the page or not, unlike doing `$('[data-js-menu]').on(...)` which needs to be done when the element is on the page.

```js
$(document).on('click', '[data-js-menu]', function () {
  // ...
})
```

However, this technique comes at a runtime performance cost. If you bind events for `click` for 50 components, every click on the page will check for 50 possible event handlers. Instead, consider using [onmount] and bind your events directly to elements as needed.

Also see [extras](extras.md#event-delegation) for more info on event delegation.

### Use each() when needed

When your behavior needs to either initialize first and/or keep a state, consider using [jQuery.each]. See [extras](extras.md#example-of-each) for a more detailed example.

```js
$(function() {
  $('[data-js-expanding-menu]').each(function() {
    var $menu = $(this)
    var state = {}

    // - do some initialization code on $menu
    // - bind events to $menu
    // - use `state` to manage state
  })
})
```

If you are using [onmount], this is not necessary.

### Avoid side effects

Make sure that each of your JavaScript files will not throw errors or have side effects when the element is not present on the page.

```js
$(function () {
  var $nav = $("[data-js-hiding-nav]")

  // Don't bind the `scroll` event handler if the element isn't present.
  // This will avoid making the page sluggish unnecessarily. This also
  // avoids the error that $nav[0] will be undefined.
  if (!$nav.length) return

  $('html, body').on('scroll', function () {
    if ($nav[0].disabled) return
    var isScrolled = $(window).scrollTop() > $nav.height()
    $nav.toggleClass('-hidden', !isScrolled)
  })
})
```

If you're using [onmount], things like the `$nav.length` check is not necessary.

### Dynamic content

There are times when you may wish to apply behaviors to content that may appear on the page later on. This is the case for AJAX-loaded content such as modal dialogs.

You can do three approaches for this:

* Use [onmount] _(recommended)_. This is the easiest and most scalable solution.
* Use [event delegation](#use-event-delegation) on `document`. This only works if you're only binding events.
* Wrap your initialization code in a function and run in when the modal appears (described below).

If you're not using [onmount] or event delegation, you can wrap your initialization code in a function. Run that function on document.ready and when the DOM changes (eg, `show.bs.modal` in the case of Bootstrap modal dialogs).

Since your initializer may be called multiple times in a page, you will need to make them [idempotent] by bypassing elements that it has already been applied to. This can be done with an [include guard] pattern.

[idempotent]: https://en.wikipedia.org/wiki/Idempotent
[include guard]: http://en.wikipedia.org/wiki/Include_guard

```js
// compoents/jsKeyValuePair/component.js
const ELEMENT_SELECTOR = '[data-js-key-value-pair]';
const BEHAVIOUR_NAME = 'jsKeyValuePair';

export class jsKeyValuePair {
  constructor (element, options) {
    this.element = element;
    this.$element = $(element);

    ...

    this.$element.data(BEHAVIOUR_NAME, this);

    this.watch();
  }

  watch () {
    ...
  }

  do () {
    ...
  }
}
```

```js
// compoents/jsKeyValuePair/index.js
$(ELEMENT_SELECTOR).each(function () {
  // an include g
  uard to keep it idempotent
  if ($(this).data(BEHAVIOUR_NAME)) return

  System.import(
    /* webpackChunkName: "components/jsKeyValuePair" */
    './component').then(({jsKeyValuePair}) => {
      new jsKeyValuePair(this, compileAttributeOptions('jsKeyValuePair', this));
    })
  // init code here
})
```

If you're using [onmount], simply bind `onmount()` to these events (such as `show.bs.modal`). Idempotency will be taken care of for you.

## Namespacing

?> **TODO** we discourage global use all together

### Keep the global namespace clean

Place your publically-accessible classes and functions in an object like `App`.

```js
if (!window.App) window.App = {}

App.Editor = function() {
  // ...
}
```

### Organize your helpers

?> **TODO** we discourage global use all together

If there are functions that will be reusable across multiple behaviors, put them in a namespace. Place these files in `helpers/`.

```js
/* helpers/format_error.js */
if (!window.Helpers) window.Helpers = {}

Helpers.formatError = function (err) {
  return "" + err.project_id + " error: " + err.message
}
```

## Third party libraries

?> **TODO** Talk about :

  * where these files go
  * how to lazy load them
  * how to `import`/`require` them

```js
...
└── javascripts/
    └── behaviors/
        ├── colorpicker.js
        ├── select2.js
        └── wow.js
```

### Separate your vendor libs

Keep your 3rd-party libraries in something like `vendor.js`.

?> **TODO** talk about webpack entry points and optimisation techniques.

This will have browsers cache the 3rd-party libs separately so users won't need to re-fetch them on your next deployment.

It also makes it easier to create new app packages should you need more than one (eg, one for your public pages and another for private dashboards).


### Load 3rd-party resources asynchronously

For third-party resources that are loaded externally, consider loading them with an asynchronous loading mechanism such as [script.js].

Some 3rd party libraries are loaded via `<script>` tags, such as Google Maps's API. These vendors typically expect you to embed them like so:

```html
<script src='//maps.google.com/maps/api/js?v=3.1.3&amp;libraries=geometry'></script>
```

```js
$.onmount('.map-box', function () {
  Gmaps.buildMap({ ... })
})
```

If your website doesn't use them everywhere, it'd be wasteful to include them on every page. You can selectively include them only on pages that need them, but that would hamper reusability: if you want to reuse the `.map-box` component in another page, you'll need to re-include the script in that other page as well.

Instead, consider using [script.js] wrapped in a helper function to load them on an as-needed basis.

```js
Helpers.useGoogleMaps = function useGoogleMaps (fn) {
  var url = 'https://maps.google.com/maps/api/js?v=3.1.3&libraries=geometry'

  $script(url, function () {
    // pass the global `Gmaps` variable to the callback function.
    fn(window.Gmaps)
  })
}
```

This `useGoogleMaps` helper can be used like so:

```js
var useGoogleMaps = Helpers.useGoogleMaps

$.onmount('.map-box', function () {
  useGoogleMaps(function (Gmaps) {
    Gmaps.buildMap({ ... }) // use Gmaps here
  })
})
```

## Appendix

### Loading component files

**[Webpack](https://webpack.github.io/):** you can use `require.context` to load multiple CSS files. See [this StackOverflow answer](http://stackoverflow.com/a/30652110/873870) for details.

```js
// http://stackoverflow.com/a/30652110/873870
function requireAll (r) { r.keys().forEach(r) }

requireAll(require.context('./behaviors/', true, /\.js$/))
requireAll(require.context('./initializers/', true, /\.js$/))
```


## Conclusion

This document is a result of my own trial-and-error across many projects, finding out what patterns are worth adhering to on the next project.

This document prioritizes developer sanity first. The approaches here may not have the most performant, especially given its preference for event delegations and running `document.ready` code for pages that don't need it. Regardless, the pros and cons were weighed and ultimately favored approaches that would be maintainable in the long run.

The guidelines outlined here are not a one-size-fits-all approach. For one, it's not suited for [single page applications][SPA], or any other website relying on very heavy client-side behavior. It's written for the typical conventional web app in mind: a collection of HTML pages that occasionally need a bit of JavaScript to make things work.

As with every other guideline document out there, try out and find out what works for you and what doesn't, and adapt accordingly.

## Further reading

- [onmount] - for safe, idempotent JavaScript behaviors and easy Turbolinks support
- [script.js] - for loading external scripts

[document.ready]: http://api.jquery.com/ready/
[jQuery.each]: http://api.jquery.com/jQuery.each/
[extras]: extras.md
[onmount]: https://github.com/rstacruz/onmount
[script.js]: https://github.com/rstacruz/scriptjs/
[RSCSS]: http://rscss.io