---
desc: Play UI's design and architectural concepts
_index: first
---
# Concepts

Much good thinking!

## Succinct API Design

Good API design is key to a good developer experience! Expect nothing less with Play UI! Its every method comes trying to *cut the long story short*. For example, consider how the short [`.play()`](../api/ui/play) method gives you an intuitive way to play animations in however you define the keyframes. Similarly, check out the [`.on()`](../api/ui/on) method that lets you listen to both DOM events and user gestures.

We hope you have a good developer experience!

## Async UI

Surgically updating the UI is generally a costly operation for browsers. It happens when we write to the DOM and read from it in quick succession in a rendering cycle - causing document reflows, or better put, forced synchronous layout. (But a common word is *layout thrashing*.) This is covered in detail in [this article on Web Fundamentals](https://developers.google.com/web/fundamentals/performance/rendering/avoid-large-complex-layouts-and-layout-thrashing).

Play UI meets this challenge with a simple timing strategy that keeps UI manipulation in sync with the browser's rendering cycle. To do this, DOM operations are internally held in read/write queues, then executed in read/write batches within each rendering cycle, eliminating the forced synchronous layout problem. This is what happens under the hood with all of the Play UI functions that have the *Async* suffix; e.g [`htmlAsync()`](../api/dom/htmlasync). The asynchronous nature of these functions bring them under the term *Async UI*.

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

## Web-Native

Play UI maintains a native-centric approach of letting vanilla JavaScript (and ES6+) meet every challenge as a first step before thinking out of the box. This lets you and your app get all the goodness of modern JavaScript!

+ Conventional syntax.
+ A minimal footprint and streamlined feature set.
+ A web-native performance.

