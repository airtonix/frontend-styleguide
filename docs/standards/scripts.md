
?> 🚧 Attaching, Lifecycle, Options from data attributes


### Behaviour Pattern

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

// abc123/components/authorcard/component.js
import debug from 'debug'; // 1️⃣
import addPlugin, {jQueryPlugin} from '@core/behaviour/base'; // 2️⃣

const log = debug('abc123/components/author-card'); // 3️⃣

class authorCardComponent extends jQueryPlugin { // 4️⃣
    constructor (element) {
        log('creating', {element});
        this.element = element;
        this.$element = $(element); // 5️⃣
        this.options = this.optionsFromDataAttributes(); // 6️⃣

        // associate elements
        this.authorLink = this.$element.find(this.options.authorlinkselector);

        this.guardElement() // 7️⃣
            .then(this.watch.bind(this));

        log('ready');
    }

    watch () {
        log('watching', this.element});
        this.authorLink.on('hover', this.showToolTip.bind(this));
    }

    showTooltip () {
        const url = this.authorLink.attr('href');
        log('showTooltip', this.element, url});
        if (!url) return;

        $.get(url)
            .done((data) => {
                ...
            });
    }
}

addPlugin(authorCardComponent, {
    authorlinkselector: '.author-card__profile-link'
});
```

```js
// abc123/components/authorcard/index.js

```
