---
title: Download
desc: Observer's download options
_index: 1
---
# Download Options

Observer can be used via a script tag or as an npm package.

## Option 1: Embed as script

Add the following script tag to your page to include the all-in-one Observer module:

```html
<script src="https://unpkg.com/@webqit/observer/dist/main.js"></script>
```

The above tag loads Observer into a global "WebQit" object.

```html
<script>
    const Observer = window.WebQit.Observer;
</script>
```

## Option 2: Install via NPM

With [npm available on your terminal](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm), run the following command to install Observer:

```text
$ npm i @webqit/observer
```

Import the installed package:

```js
// Import the initializer
import Observer from '@webqit/observer';
```

## Next Steps

The [API Reference](../api).