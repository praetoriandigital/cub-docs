## Sentry SDK integration

Cub Widget may use Sentry SDK to annotate exceptions with user data and
widget-related breadcrumbs. To enable Sentry SDK integration, pass the global
Sentry object during the widget setup in the `siteSentry` property:

```js
cub.start({
  ...
  siteSentry: window.Sentry
});
```
