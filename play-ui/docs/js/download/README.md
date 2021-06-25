---
title: Download
desc: Play UI's download options
_index: 1
---
# Download Options

Play UI can be used via a script tag or downloaded as an npm package.

## Option 1: Embed as script

Add the following script tag to your page to include the all-in-one Play UI module:

```html
<script src="https://unpkg.com/@webqit/play-ui/dist/main.js"></script>
```

The above tag loads Play UI into a global "WebQit" object.

```html
<script>
    const $ = window.WebQit.$;
</script>
```

## Option 2: Install via NPM

With [npm available on your terminal](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm), run the following command to install Play UI:

```text
$ npm i @webqit/play-ui
```

Import and initialize the installed package:

```js
// Import the initializer
import PlayUI from '@webqit/play-ui';
// Initialize
const $ = PlayUI();
```

And you can selectively import Play UI's descrete parts to streamline your imports to your needs. The [API reference](../api) has the *import* syntax for each of Play UI's functions. For example:

```js
// Import a function...
import { htmlSync as html } from '@webqit/play-ui/src/dom/index.js';

// Supply an element as first argument...
html(selector, 'Play away!');
```

Details are in the [Quick Start](../quickstart#use-as-descrete-utilities) guide.

## Next Steps

Go to the [API Reference](../api).