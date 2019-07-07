# Frontend Styleguide
<!-- {h1:.massive-header.-with-tagline} -->

Our standards for frontend coding.<br>
A set of simple ideas to guide your process of building maintainable html, javascript and css.

Introduction
------------

Any amount of code greater than 1000 lines will get unwieldy. You'll eventually run into these common pitfalls:

* "What does this class mean?"
* "Is this javascript even used anymore?"
* "Is this class still being used?"
* "What can the relevant markup look like?"
* "If I make a new class `.red`, will there be a clash?"
* "Why is this page taking ages to load?"

**Frontend Styleguide** is an attempt to make sense of all these. It is not a framework. It's simply a set of ideas to guide your process of building maintainable code for any modern website or application.

## In a nutshell

**Frontend Styleguide** seeks to make Css and JavaScript easy to maintain in a typical web application. All recommendations here have these goals in mind:

- Keep your HTML _declarative_ (get rid of inline scripts).
- Put all your _imperative_ code in JS files.
- Reduce ambiguity by having straight-forward conventions.
- HTML elements can be _components_ that have _behaviors_.
- Behaviors apply JavaScript to a `[data-js-behavior-name]` selector.

?> 🚡 At this point, you can get started by learning [about components →](components/components.md)

## In depth
?> 🏆 https://benfrain.com/enduring-css-writing-style-sheets-rapidly-changing-long-lived-projects/

### BEM {docsify-ignore}

?> 🗻 Getting your rules to match the elements you want, without them accidentally matching the elements you don’t.

To go into detail, lets look at some quotes from others in the industry.

As outlined by [Robin Rendle](https://css-tricks.com/bem-101/#article-header-id-0)

> 1. If we want to make a new style of a component, we can easily see which modifiers and children already exist. We might even realize we don't need to write any CSS in the first place because there is a pre-existing modifier that does what we need.
1. If we are reading the markup instead of CSS, we should be able to quickly get an idea of which element depends on another (in the previous example we can see that .btn__price depends on .btn, even if we don't know what that does just yet.)
1. Designers and developers can consistently name components for easier communication between team members. In other words, BEM gives everyone on a project a declarative syntax that they can share so that they're on the same page.

### The confidence to change {docsify-ignore}

[Harry Roberts](http://csswizardry.com/2015/03/more-transparent-ui-code-with-namespaces) talks about the fear of change
> This is the main reason we end up with bloated code bases, full of legacy and unknown CSS that we daren't touch. We lack the confidence to be able to work with and modify existing styles because we fear the consequences of CSS' globally operating and leaky nature. Almost all problems with CSS at scale boil down to confidence (or lack thereof): People don't know what things do any more. People daren't make changes because they don't know how far reaching the effects will be.

[Philip Walton](https://philipwalton.com/articles/side-effects-in-css/) speaks to the solution
> While 100% predictable code may never be possible, it's important to understand the trade-offs you make with the conventions you choose. If you follow strict BEM conventions, you will be able to update and add to your CSS in the future with the full confidence that your changes will not have side effects.

