Pitfalls
==========


### Bleeding through nested components
Be careful about nested components with elements sharing the same name as elements in its container.

```html

<article class='article-link'>
 <div class='vote-box'>
    <button class='up'></button>
    <button class='down'></button>
    <span class='count'>4</span> /* ✖️ Don't do this */
  </div>

  <h3 class='title'>Article title</h3>
  <p class='count'>3 votes</p> /* ✖️ Don't do this */
</article>
```

In this case, if `.article-link > .count` did not have the `>` (child) selector, it will also apply to the `.vote-box .count` element. This is one of the reasons why child selectors are preferred.

Instead:


```html
<article class='article-link'>
 <div class='vote-box article-link__vote-box'>
    <button class='vote-box__button vote-box__button--up'></button>
    <button class='vote-box__button vote-box__button--down'></button>
    <span class='vote-box__count'>4</span> /* ✔️ Better */
  </div>

  <h3 class='article-link__title'>Article title</h3>
  <p class='article-link__count'>3 votes</p> /* ✔️ Better */
</article>
```
