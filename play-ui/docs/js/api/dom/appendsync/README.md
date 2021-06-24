---
desc: Append content to an element.
---
# `.appendSync()`

This method is used to append content to an element. It works exactly the same as the native [`ParentNode.append()`](https://developer.mozilla.org/en-US/docs/Web/API/ParentNode/append) except that when the implied content is undefined, it is converted to an empty string.

The suffix *Sync* differentiates this method from its *Async* counterpart - [`.appendAsync()`](../appendasync). Unlike the *Async* counterpart, this method is promised-based and works in sync with the UI's reflow cycle. See [Async UI](../../concepts#async-ui).

## Syntax

```js
// Append content(s) to an element
$(el).appendSync(content[, ...content]);
```

**Parameters**

+ `content`: `String|Node` - The text or HTML content, or some DOM node, to append.

**Return**

+ `this` - The Play UI instance.

## Usage

Append an element node and some text content to an element.

```js
let div = document.createElement("div");
$(el).appendSync(div, 'Playful', ' ', 'people', '!');
```

------

## Static Usage

The `.appendSync()` instance method is internally based on the standalone `dom/appendSync()` function which may be used statically.

### Import

```js
const { appendSync } = $.dom;
```
```js
import { appendSync } from '@webqit/play-ui/src/dom/index.js';
```

### Syntax

See [the general way to use Play UI's standalone functions](../../../quickstart#use-as-descrete-utilities)