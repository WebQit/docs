# DOM/appendSync\(\)

This function appends content to an element. It works exactly the same as [`ParentNode.append()`](https://developer.mozilla.org/en-US/docs/Web/API/ParentNode/append) except that when the implied content is undefined, it is converted to an empty string.

The suffix *Sync* differentiates this method from its *Async* counterpart - [`appendAsync()`](../appendasync). Unlike the *Async* counterpart, `appendSync()` is a normal function that runs in the same flow with that of the calling code.

## Import

```javascript
import appendSync from '@webqit/play-ui/src/dom/appendSync.js';
```

## Syntax

```javascript
appendSync(el[, ...content);
```

### Parameters

* `el` - `HTMLElement`: The target DOM element.
* `content` - `[String|HTMLElement]`: The set of content to append. Each could be a plain text, an HTML/XML markup, or even a DOM node.

### Return

* `HTMLElement` - The target DOM element.

## Usage

```markup
<body></body>
```

```javascript
// Prepend content
let body = appendSync(document.body, '!', 'world', ' ', 'Hello');
```

