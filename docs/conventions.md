# Convetions

While compiling, editing and merging other works into this document we ended up using some conventions to indicate purpose and tone throughout.

?> 🔨 install [*emojisense*](https://marketplace.visualstudio.com/items?itemName=bierner.emojisense) for vscode to make using the emojis easier. Additionally a full list of unicode emojis can be explored on the [unicode website](https://unicode.org/emoji/charts/full-emoji-list.html)

## Publishing Indicators

These are variants of blockquotes with emojis to indicate attention from within the team.

?> 👁‍🗨 **Needs Peer Review**, indicates that other team members are invited to provide input via comments on github pull requests.

?> 🚧 **Todo** An area that needs to be created. Lets consumers of document know that a concept has been considered and will eventually be covered.

...

!> 🤚 **Deprecated** Indicates following information that will become deprecated

## Conversational Tone

?> 💭 **Topic** Helpful optional considerations are written like this.

Example of code to avoid.
```js
/* ✖️ Avoid */
/* ✖️ Bad */
const password = alert('enter your password'); /* ❗️ Don't do this */
```

Example of desired code
```html
<div class="form-field">
    <label class="form-field__label"
           for="password">Your Password</label> /* ✔️ Better */
    <input class="form-field__input"
           type="password"
           id="password"> /* 👈 matches with ↖️ "for" attribute */
</div>
```
