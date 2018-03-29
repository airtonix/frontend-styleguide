Site Themes
===========

Site themes exist separately from core like so:

```bash
client/
    src/
        core/
        site__abc123/  /* ✔️ your jira project code and all its frontend files here */
            components/
                ...
            icons/
                ...
            images/
                ...
            fonts/
                ...
            sprites/
                ...
        construct/
            functions/
            settings/

```

### Configuration

?> **TODO** happens in `./construct/settings`, extends `@core/construct/settings` with `map-merge`. All settings are exposed as maps.

#### Colours

?> **TODO** Describes hues and their variants.

```scss

$settngs__colours: (
    blue: (
        mostlight: lighten(#00f, 20%),
        lightest: lighten(#00f, 15%),
        lighter: lighten(#00f, 12%),
        light: lighten(#00f, 10%),
        default: #00f
        dark: darken(#00f, 10%),
        darker: darken(#00f, 12%),
        darkest: darken(#00f, 15%),
        mostdark: darken(#00f, 20%),
    )
);
```
this works in conjuction with the `colour-get` function.

```scss
.thing {
  color: colour-get(blue, darker);
}
```

#### Media Queries

?> **TODO**

#### Fonts

?> **TODO**

#### Layout

Mostly defines things like spacings

?> **TODO**