# CSS/writeAsync\(\)

This function sets one or more style properties on the given element. It is the programmatic alternative to [`ElementCSSInlineStyle.style`](https://developer.mozilla.org/en-US/docs/Web/API/ElementCSSInlineStyle/style). It also has special support for vendor-prefixed properties.

The suffix *Async* differentiates this method from its *Sync* counterpart - [`writeSync()`](../writesync). Unlike the *Sync* counterpart, `writeAsync()` is a promised-based function that runs in a different flow from that of the calling code. It follows a performance strategy that lets the browser engine decide the most convenient time to honour its call.

## Import

```javascript
import writeAsync from '@webqit/play-ui/src/css/writeAsync.js';
```

## Syntax
See [`cssAsync() - Set/Unset Inline Styles`](../cssasync#greater-than-set-unset-inline-styles)
