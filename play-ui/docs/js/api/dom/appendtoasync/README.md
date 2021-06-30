---
desc: Asynchronously append matched elements to target elements.
---
# `.appendToAsync()`

This method is used to asynchronously append matched elements to target elements.

The suffix *Async* differentiates this method from its *Sync* counterpart - [`.appendToSync()`](../appendtosync). Unlike the *Sync* counterpart, this method is promised-based and works in sync with the UI's reflow cycle. See [Async UI](../../concepts#async-ui).

## Syntax

```js
await $(el).appendToAsync(target);
```

**Parameters**

+ `target`: `String|Element` - A CSS selector of the element, or the element itself `Element`, to append to.

**Return**

+ `this` - The Play UI instance.

## Usage

Append some dynamically-created nodes to document body.

```js
$('<h1>Playful people!</h1>').appendToAsync(document.body);
```

Append an existing `div` to multiple targets. The `div` is cloned into all targets except the last target.

```js
let div = document.querySelector('div');
$(div).appendToAsync('.multiple');
```

------

## Static Usage

The `.appendToAsync()` instance method is internally based on the standalone `dom/appendToAsync()` function which may be used statically.

### Import

```js
const { appendToAsync } = $.dom;
```
```js
import { appendToAsync } from '@webqit/play-ui/src/dom/index.js';
```

### Syntax

See [the general way to use Play UI's standalone functions](../../../quickstart#use-as-descrete-utilities)