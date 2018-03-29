# Attaching Javascript

When a component requires javascript to deliver its features, we need to keep several things in mind:

  * Identifying the correct elements for enhancing.
  * Minimising the amount of code loaded unless needed.
  * Accepting different runtime configuration between several components.

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

### What's wrong?

This anti-pattern leads to many issues, which the Frontend Styleguide attempts to address.

 * **Ambiguious sources:** It's not obvious where to look for the JS behavior. (Is the handler attached to *.author*, *.footnote*, or *.profile-link*?)
 * **Non-reusable:** It's not obvious how to reuse the behavior in another page. (Should *blogpost.js* be included in the other pages that need it? What if that file contains other behaviors you don't need for that page?)
 * **Lack of organization:** Making new behaviors gets confusing. (Do you make a new `.js` file for each page? Do you add them to the global `application.js`? How do you load them?)

### Use data attributes



```html
<div class='author-card author-card--compact'
     data-author-card
     data-author-card--author-link-selector="[data-magical-profile-link]"
     data-author-card--url="/user/rstacruz">
  This article was written by
  <a class='author-card__profile-link'
     data-magical-profile-link
     href="/user/rstacruz">Rico Sta. Cruz</a>.
</div>
```

```js
import addPlugin, {jQueryPlugin} from '@core/behaviour/base';

class authorCardComponent extends jQueryPlugin {
    constructor (element) {
        this.element = element;
        this.$element = $(element);
        this.options = this.optionsFromDataSet(); // comes from jQueryPlugin
        this.authorLink = this.$element.find(this.options.authorlinkselector);
        this.guardElement() // comes from jQueryPlugin
            .then(this.validateRequiredElements.bind(this)) // comes from jQueryPlugin
            .then(this.watch.bind(this));
    }

    watch () {
        this.authorLink.on('hover', this.showToolTip.bind(this));
    }

    showTooltip () {
        $.get(this.authorLink.attr('href'))
            .done((data) => {
                ...
            });
    }
}
authorCardComponent.REQUIRED_ELEMENTS = [
    'authorlink'
];

addPlugin(authorCardComponent, {
    authorlinkselector: '.author-card__profile-link'
});
```
