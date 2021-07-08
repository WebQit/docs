---
desc: Play UI overview.
_index: first
---
# Overview

Play UI is designed to meet a wide range of usage scenarios.

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

## Next Steps

Explore further: [The concepts](../concepts).
