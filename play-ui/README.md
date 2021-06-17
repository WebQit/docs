## Overview

Play UI provides a fast and delightful way to create active User Interfaces. There is [the JavaScript library](docs/js) that lets you programmatically work with the DOM and UI using the sweetest syntax posssible. There is [the set of CSS libraries](docs/css) that lets you do responsive layouts and the vivid, dramatic styling that's expected of modern UI. But the best part yet? The approach!

You want a library that abstracts the painful details of DOM APIs, but without introducing the usual overhead or getting in the way of vanilla syntax and familiar conventions; just something as succinct as [jQuery](https://jquery.com), but with a more futuristic appeal. Consider Play UI!

+ The JavaScript library follows a native-first approach of letting native JavaScript (and ES6+) features meet the challenge, before thinking out of the box.
+ The CSS libraries are streamlined to using modern language features like *CSS grids*, *CSS flexbox*, *CSS variables*, etc, to offer all the power without introducing hacky and bloated stylesheets.
+ For UI performance, instead of adopting the *Virtual DOM* approach, Play UI follows [a simple timing strategy](docs/js/reflow) that keeps UI manipulations in sync with the browser's native rendering cycle.
+ Certain APIs, like the [`play()`](docs/js/api/animation/play) function, are designed CSS-smart to help you *reuse* declarative snippets defined in CSS programmatically in JavaScript without introducing difficult-to-reverse patterns like CSS-in-JS.
+ Certain APIs, like the [`itemize()`](docs/js/api/dom/itemize) function, are designed OOHTML-smart to help you do more with a [OOHTML](../oohtml)-based UI.

## What You Can Do With Play UI

Play UI gives you all the usage flexibility of a library.

+ **Use as foundational library.** Build bigger things, like web components, that use Play UI under the hood.
+ **Use with [OOHTML](../oohtml).** Create reactive UIs with OOHTML and Play UI.
+ **Use as [jQuery](https://jquery.com).** Include Play UI on any page to use as would jQuery.

## Documentation

Visit the [docs](docs) for installation guide, API documentation and examples.

## License

Play UI is made available under the MIT license.