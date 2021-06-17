# Installation Guide

Play UI can be used via a script tag or as an npm package.

## Option 1: Embed as script

Add the following script tag to your page to include the all-in-one Play UI module:

```html
<script src="https://unpkg.com/@webqit/play-ui/dist/main.js"></script>
```

The above tag loads Play UI into a global "WQ" object.

```html
<script>
    const $ = window.WQ.$;
</script>
```

## Option 2: Install via NPM

With [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) available on your terminal, run the following command to install Play UI:

```text
$ npm i @webqit/play-ui
```

Import and initialize the installed package:

```js
import PlayUI from '@webqit/play-ui';
const $ = PlayUI();
```

It is also possible to use Play UI with a server-side *window* object such as the one provided by [jsdom](https://github.com/jsdom/jsdom). Here is how that could look:

```js
// Utilities we'll need
import fs from 'fs';
import path from 'path';
// Import jsdom
import jsdom from 'jsdom';

// Import Play UI
import PlayUI from '@webqit/play-ui';

// Read the HTML document file from the server
const documentFile = fs.readFileSync(path.resolve('./index.html'));
// Instantiate jsdom so we can obtain the "window" for building Play UI
// Detailed instruction on setting up jsdom is available in the jsdom docs
const JSDOM = new jsdom.JSDOM(documentFile.toString());

// Create the build using the window above
const $ = PlayUI({window: JSDOM.window});
```

<!--
## All-in-one or derivatively

While the most common usecase is to install and use Play UI as a single, built object, it can also be used derivatively on its smaller modules.
-->