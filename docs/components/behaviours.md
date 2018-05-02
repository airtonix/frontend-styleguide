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

Their files are "behaviors": code to describe dynamic JS behavior to affect a block of static HTML. In this example, the JS behavior `collapsiblenav` only affects a certain DOM subtree, and is placed on its own file.

```html
<!-- Component -->
<div class='main-navbar'
     data-collapsiblenav 1️⃣
     data-collapsiblenav--expandbuttonselector='.main-navbar__expand-button' 2️⃣
     data-collapsiblenav--collapsibleselector='.main-navbar__navigation-group--primary'>
  <button class='main-navbar__expand-button'>Expand</button>
  <div class="main-navbar__navigation-group main-navbar__navigation-group--primary">
    <ul class="main-navbar__items">
      <div class="main-navbar__item">
        <a class="main-navbar__item-link" href='/'>Home</a>
        <ul class="main-navbar__item-children main-navbar__items">...</ul>
      </div>
    </ul>
  </div>
</div>
```

1. Marks a component as having the `collapsiblenav` behaviour
2. Component specific configuration for the behaviour is provided as extra attributes

?> **note** We don't actually have a behaviour called collapsiable-nav. it's just an example. To discover what is available in core checkout [http://core.patterns.fusion.one](our core component library).

```js
/* Behavior - behaviors/collapsible-nav/index.js */

export const ELEMENT_SELECTOR = '[data-collapsiblenav]'; 1️⃣

$(() => {
  const $elements = $(ELEMENT_SELECTOR);
  if (!$elements.length) { return } 2️⃣

  System.import(
      /* webpackChunkName: 'components/collapsiblenav' */  3️⃣
      './components').then(({collapsibleNav}) => { // note the destructuring {}
        $elements.each((index, element) => new collapsibleNav(element); ); 4️⃣
      });
})
```

```js
/* Behavior - behaviors/collapsible-nav/index.js */

// Base is our class that provides data attribute option destructuring, instance tracking on element, etc. its constructor calls init().
export class collapsibleNav extends Base {
  init () {
    this.$expandButton = this.$element.find(this.options.expandbuttonselector);
    this.$collapsible = this.$element.find(this.options.collapsibleselector);
    this.watch();
  }

  watch () {
    this._expandClickHandler = this.$expandButton.on('click', this.toggle.bind(this));
  }

  toggle () {
    this.$collapsible.toggleClass(this.options.collapsibleclassname);
  }

  destroy () {
    this.$expandButton.off('click);
    super.destroy();
  }
}

collapsibleNav.DEFAULTS = {
  expandbuttonselector: '[data-collapsiblenav__expand]',
  collapsibleselector: '[data-collapsiblenav__collapsible]',
  collapsibleclassname: 'collapsible--expanded',
};
```