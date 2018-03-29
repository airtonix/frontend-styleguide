Hacks
==========

For general-purpose classes meant to override values, put them in a separate file and name them beginning with an underscore. They are typically things that are tagged with *!important*. Use them *very* sparingly.

```css
._unmargin { margin: 0 !important; }
._center { text-align: center !important; }
._pull-left { float: left !important; }
._pull-right { float: right !important; }
```

### Naming hacks
Prefix classnames with an underscore. This will make it easy to differentiate them from modifiers defined in the component. Underscores also look a bit ugly which is an intentional side effect: using too many hacks should be discouraged.

  ```html
  <div class='order-graphs order-graphs--slim _unmargin'>
  </div>
  ```

### Organizing helpers
Place all hacks in one file called `hacks.scss` or `{component-name}--hacks.scss`. This helps us to identify last minute pressure code to fix up or improve later on.
