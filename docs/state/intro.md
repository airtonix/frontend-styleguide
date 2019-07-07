So we have some components that follow a design.

But we need them to display useful information, typically this is done in three ways:

## [Api Driven](./api.md)

Information can come from an API as either XML or JSON.

## [State Hydration]('./rehydration.md)

This method is usually used to kick start component rendering to minimise the amount of initial API requests to as close to zero as possible.

It's also used as a way to help statically rendered components transition to dynamically rendered components.

## Mounting Attribute Directives

Slightly similar to [State Hydration](./rehydration.md) but more in line with treating components as extended HTML tags.
