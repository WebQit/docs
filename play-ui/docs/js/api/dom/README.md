# The DOM

The set of DOM APIs for traversing and manipulating the DOM.

Below, you will notice PlayUI's radical new approach to the DOM that offers a pair of **synchronous** and **asynchronous** methods for operations that affect the browser's _Layout_. The _asynchronous_ methods employ a technique that prevents [Layout Thrashing](https://developers.google.com/web/fundamentals/performance/rendering/avoid-large-complex-layouts-and-layout-thrashing) to keep the UI at maximum performance. These _asynchronous_ methods are what you want! The _synchronous_ versions would, however, need to go with the [`Reflow`](concepts#async-dom) DOM abstraction utility to achieve the same level of performance.

Modules in this set can be imported individually or collectively.

```javascript
// Import all modules
import * as DOM from '@webqit/play-ui/src/dom/index.js';
let select = DOM.select;

// Import a module
import select from '@webqit/play-ui/src/dom/select.js';
```

## API
+ [`DOM/select()`](select)
+ [`DOM/selectAll()`](selectall)
+ [`DOM/el()`](el)
+ [`DOM/appendSync()`](appendsync)
+ [`DOM/appendAsync()`](appendasync)
+ [`DOM/prependSync()`](prependsync)
+ [`DOM/prependAsync()`](prependasync)
+ [`DOM/htmlSync()`](htmlsync)
+ [`DOM/htmlAsync()`](htmlasync)
+ [`DOM/textSync()`](textsync)
+ [`DOM/textAsync()`](textasync)
+ [`DOM/attrSync()`](attrsync)
+ [`DOM/attrAsync()`](attrasync)
+ [`DOM/classSync()`](classsync)
+ [`DOM/classAsync()`](classasync)
+ [`DOM/data()`](data)
+ [`DOM/getTextNodes()`](gettextnodes)
+ [`DOM/mutationCallback()`](mutationcallback)
+ [`DOM/connectedCallback()`](connectedcallback)
+ [`DOM/disconnectedCallback()`](disconnectedcallback)
+ [`DOM/attrChangeCallback()`](attrchangecallback)
