Behaviours
=========
Behaviours are complex logic that enables interactions beyond those provided by the standard html elements (*forms*, *links*, *alt tags*, *modals*, etc ).


When a component requires javascript to actualise its promised features, we need to keep several things in mind:

  * Identifying the correct elements for enhancing.
  * Minimising the amount of code loaded unless needed.
  * Accepting different runtime configuration between several components.


### Avoid mindless jQuery
Consider the following typical example:

```html
<script src='blogpost.js'></script>
<div class='author footnote'>
  This article was written by
  <a class='profile-link' href="/user/rstacruz">Rico Sta. Cruz</a>.
</div>
```

```js
$(function () {
  $('.author a').on('hover', function () {
    var username = $(this).attr('href')
    showTooltipProfile(username, { below: $(this) })
  })
})
```

**What's wrong?**

This anti-pattern leads to many issues, which the Fusion Styleguide attempts to address.

 * **Ambiguious sources:** It's not obvious where to look for the JS behavior. (Is the handler attached to *.author*, *.footnote*, or *.profile-link*?)
 * **Non-reusable:** It's not obvious how to reuse the behavior in another page. (Should *blogpost.js* be included in the other pages that need it? What if that file contains other behaviors you don't need for that page?)
 * **Lack of organization:** Making new behaviors gets confusing. (Do you make a new `.js` file for each page? Do you add them to the global `application.js`? How do you load them?)


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
