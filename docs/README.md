# Fusion Frontend
<!-- {h1:.massive-header.-with-tagline} -->

> Styling CSS without losing your sanity

Fusion standards for frontend coding.<br>
A set of simple ideas to guide your process of building maintainable javascript and css.

Introduction
------------

Any amount of code greater than 1000 lines will get unwieldy. You'll eventually run into these common pitfalls:

* "What does this class mean?"
* "Is this jQuery even used anymore?"
* "Is this class still being used?"
* "What can the relevant markup look like?"
* "If I make a new class `green`, will there be a clash?"
* "Why is this page taking ages to load?"

**Fronted Styleguide** is an attempt to make sense of all these. It is not a framework. It's simply a set of ideas to guide your process of building maintainable code for any modern website or application.

## In a nutshell

**Fronted Styleguide** seeks to make Css and JavaScript easy to maintain in a typical web application. All recommendations here have these goals in mind:

- Keep your HTML _declarative_ (get rid of inline scripts).
- Put all your _imperative_ code in JS files.
- Reduce ambiguity by having straight-forward conventions.
- HTML elements can be _components_ that have _behaviors_.
- Behaviors apply JavaScript to a `[data-js-behavior-name]` selector.

Let's get started by learning about components.
[Continue →](docs/components/components.md)
<!-- {p:.pull-box} -->

----
<!-- {hr: style='display:none'} -->
