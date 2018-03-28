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