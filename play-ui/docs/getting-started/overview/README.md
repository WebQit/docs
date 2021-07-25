---
desc: Play UI overview.
_index: first
---
# Overview

Play UI JavaScript is a UI library designed to meet a wide range of usage scenarios.

## Use as You Would [jQuery](https://jquery.com)

[Include Play UI](../download) on any page and use as you would jQuery. Play UI looks and feels just like it!

Simply obtain the *constructable Play UI function* (`$`).

*If Play UI was loaded via a script tag:*

```js
// The constructable function would be available in a global WebQit object
const $ = window.WebQit.$;
```

*If Play UI was installed as a package:*

```js
// We would first call the initializer to obtain the constructable function
import PlayUI from '@webqit/play-ui';
const $ = PlayUI();
```

Construct instances with the `new` operator or by calling the function statically.

*With the new operator:*

```js
(new $(selector)).html('Play away!');
```

*Statically:*

```js
$(selector).html('Play away!');
```

Now, while a 100% parity with jQuery isn't the goal, Play UI comes helpfully close, covering most day to day use cases. Examples: [`.html()`](../api/dom/html), [`.append()`](../api/dom/append), [`.prepend()`](../api/dom/prepend), [`.attr()`](../api/dom/attr), [`.css()`](../api/css/css), [`.data()`](../api/app/data), [`.on()`](../api/ui/on), [`.off()`](../api/ui/off), [`.trigger()`](../api/ui/trigger).

## Use With Server-Side DOM Instances

Use Play UI with server-side *window* objects such as the type provided by the [jsdom](https://github.com/jsdom/jsdom) library. (Think server-side rendering, web crawling, or just server-side DOM manipulation.) Here is how that could look.

*Create a `window` object:*

```js
// Utilities we'll need
import fs from 'fs';
import path from 'path';
// Import jsdom
import jsdom from 'jsdom';

// Read the HTML document file from the server
const documentFile = fs.readFileSync(path.resolve('./index.html'));
// Instantiate jsdom so we can obtain the "window" for building Play UI
// Detailed instruction on setting up jsdom is available in the jsdom docs
const JSDOM = new jsdom.JSDOM(documentFile.toString());
const window = JSDOM.window;
```

*Initialize Play UI with the window object passed in as its `this` context. (Note the `PlayUI.call()` syntax.):*

```js
// Import Play UI
import PlayUI from '@webqit/play-ui';
// Initialize
const $ = PlayUI.call(window);
// Query...
$(selector).append('Ready!');
```

Now, the per-window initialization approach lets us have multiple *window* instances running simulteneously, if we need to, without getting weird behaviours as you would with the idea of a global window.

*Initialize another Play UI object on another window instance:*

```js
const $$ = PlayUI.call(window_2);
$$(selector).append('Ready!');
```

## Use as Descrete Utilities

Play UI's instance methods are internally based on it's core standalone functions which may be imported and used individually. The [`.on()`](../api/ui/on) instance method, for example, is based on the standalone [`ui/on()`](../api/ui/on#static-usage) function.

```js
const ( on ) = $.ui;
```
```js
import ( on ) from '@webqit/play-ui/src/ui/index.js';
```

*Standalone functions have their import syntax documented alongside their instance counterpart.*

Generally, these standalone functions work the same way as their instance counterpart, except that they initially take an *element selector* as their first argument - where *element selector* is any of the input types accepted by the `$()` function.


```js
$(selector).on('swipeleft', e => {
    // Handle swipe gesture
});
```
```js
on(selector, 'swipeleft', e => {
    // Handle swipe gesture
});
```

If you were to run your code against a *window* context other than the global browser window, you could use the `Function.call()` syntax to pass in the *window* object as the `this` context for the function.

```js
on.call(window, selector, 'swipeleft', e => {
    // Handle swipe gesture
});
```

However, functions obtained from the already initialized Play UI object `$` automatically inherit the *window* of the Play UI object `$`.

```js
// Import Play UI
import PlayUI from '@webqit/play-ui';
// Initialize
const $ = PlayUI.call(window);

// Worry no more about a window object
$.ui.on(selector, 'swipeleft', e => {
    // Handle swipe gesture
});
```

*Note that functions that are chainable as instance methods are not chainable when used statically as these always return the `this` context passed in.*

Now, build bigger things, like web components, with Play UI kicking under the hood. Selectively import just the functions you need into your projects.

## Meet Async UI

Surgically updating the UI is generally a costly operation for browsers. It happens when we write to the DOM and read from it in quick succession in a rendering cycle - causing document reflows, or better put, forced synchronous layout. (But a common word is *layout thrashing*.) This is covered in detail in [this article on Web Fundamentals](https://developers.google.com/web/fundamentals/performance/rendering/avoid-large-complex-layouts-and-layout-thrashing).

Play UI meets this challenge with a simple timing strategy that keeps UI manipulation in sync with the browser's rendering cycle. To do this, DOM operations are internally held in read/write queues, then executed in read/write batches within each rendering cycle, eliminating the forced synchronous layout problem. This is what happens under the hood with all of the Play UI functions that have the *Async* suffix; e.g [`htmlAsync()`](../api/dom/htmlAsync). The asynchronous nature of these functions bring them under the term *Async UI*.

The order of execution of the code below demonstrates the asynchronous nature of these functions.

```js
// Set content
$(document.body).htmlAsync('Hi').then(() => {
    console.log('Completed: write operation 1');
});

// Get content
$(document.body).htmlAsync().then(content => {
    console.log('Completed: read operation 1');
});

// Set content
$(document.body).htmlAsync('Hi again').then(() => {
    console.log('Completed: write operation 2');
});

// Get content
$(document.body).htmlAsync().then(content => {
    console.log('Completed: read operation 2');
});

// ------------
// console
// ------------
Completed: read operation 1
Completed: read operation 2
Completed: write operation 1
Completed: write operation 2
```

Notice that *read* operations are executed first, then *write* operations.

Where the order of execution matters, subsequent code could be moved into the `then()` block each of the *async* functions.

```js
// Set content
$(document.body).htmlAsync('Hi').then(() => {
    console.log('Completed: write operation 1');
    // Get content
    $(document.body).htmlAsync().then(content => {
        console.log('Completed: read operation 1');
    });
});

// ------------
// console
// ------------
Completed: write operation 1
Completed: read operation 1
```

Now, where immediate DOM manipulation is still a necessity, the *Sync* counterpart of the functions above will be just as good.

```js
// Set content
$(document.body).htmlSync('Hi');
console.log('Completed: write operation 1');
// Get content
$(document.body).htmlSync();
console.log('Completed: read operation 1');

// ------------
// console
// ------------
Completed: write operation 1
Completed: read operation 1
```

Note that the *Sync* option is what is implied where no suffix is explicitly used in the function name.

```js
$(document.body).html('Hi');
console.log('Completed: write operation 1');
```

## Next Steps

Explore further: [The API Reference](../api).
