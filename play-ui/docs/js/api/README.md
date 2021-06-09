# API Reference

## Individual Functions Documentation

Play UI is designed as a collection of standalone functions organized in the following categories.

* [The DOM](dom) - DOM manipulation utilities.
* [CSS](css) - CSS-processing utilities.
* [Animation](animation) - Animation utilities.
* [Interaction](interaction) - Utilities for binding events and gestures.
* [The UI](ui) - Utilities for manipulating the rendered UI.

## Constructible Build Documentation

Play UI is also implemented as a *constructible function* (like the jQuery function `$`). An instance can be created by using the `new` operator or by calling the function statically.

```js
// Import PlayUI
import PlayUI from '@webqit/play-ui';
// Or, if loaded via a script tag...
const PlayUI = window.WQ.PlayUI;

// Obtain the constructible function
const $ = PlayUI();

// Create an instance with the new operator
let instance = new $(element);
// Or by calling the function statically
let instance = $(element);
```

Instances of Play UI are built from the same Play UI utility functions above. Each instance method thus has the same function definition as its corresponding utility function.

The `.htmlSync()` instance method, for example, is based on the utility function at `./src/dom/htmlSync.js`.

```js
// The .htmlSync() instance method
let instance = $(element).htmlSync(content);

// --------------------------

// The actual standalone htmlSync() function
import htmlSync from './src/dom/htmlSync.js';
htmlSync(element, content);
```

Most instance methods, like the one above, return the Play UI instance itself for easy method chaining. The *async* counterpart of these methods would return a promise that resolves to the Play UI .

```js
// The .htmlAsync() instance method
$(element).htmlAsync(content).then(instance => {
    // .somethingElse()
});
// Or within an async function
let instance = await $(element).htmlAsync(content);

// --------------------------

// The actual standalone htmlAsync() function
import htmlAsync from './src/dom/htmlAsync.js';
htmlAsync(element, content).then(() => {
    // somethingElse(element)
});
// Or within an async function
await htmlAsync(element, content);
```

### DOM Manipulation Methods

DOM manipulation methods. The most of these methods come in a *sync* and *async* flavour, and a third unsiffixed flavour that is an alias to the *async* method.

#### `.htmlAsync()`

This method *asynchronously* sets or gets the HTML content for the current matched element. It uses the [`dom/htmlAsync()`](dom/htmlasync) function.

This method _asynchronously_ sets or gets the HTML content for the current matched element. It uses the [`dom/htmlAsync()`](dom/htmlasync) function.

```javascript
// Set the HTML content, do something else afterward
$('#el').htmlAsync('Hello player!').then(instance => {
    // Do something else
});

// Get the HTML content, log it to the console when available
$('#el').htmlAsync().then(content => {
    console.log(content);
});
```

See [`dom/htmlAsync()`](dom/htmlasync) for details.

#### `.htmlSync()`

This method *synchronously* sets or gets the HTML content for the current matched element. It uses the [`dom/htmlSync()`](dom/htmlsync) function.

This method _synchronously_ sets or gets the HTML content for the current matched element. It uses the [`dom/htmlSync()`](https://github.com/web-native/docs/tree/201e05f348ffbec3ef9e613f36713ffdc10badc9/play-ui/api/dom/htmlsync) function.

```javascript
// Set the HTML content
let instance = $('#el').htmlSync('Hello player!');

// Get the HTML content, log it to the console
let content = $('#el').htmlSync();
console.log(content);
```

See [`dom/htmlSync()`](dom/htmlsync) for details.

#### `.html()`

This is an alias of the [`.htmlAsync()`](#instancehtmlasync) method.

#### `.textAsync()`

This method *asynchronously* sets or gets the text content for the current matched element. It uses the [`dom/textAsync()`](dom/textasync) function.

This method _asynchronously_ sets or gets the text content for the current matched element. It uses the [`dom/textAsync()`](dom/textasync) function.

```javascript
// Set the text content, do something else afterward
$('#el').textAsync('Hello player!').then(instance => {
    // Do something else
});

// Get the text content, log it to the console when available
$('#el').textAsync().then(content => {
    console.log(content);
});
```

See [`dom/textAsync()`](dom/textasync) for details.

#### `.textSync()`

This method *synchronously* sets or gets the text content for the current matched element. It uses the [`dom/textSync()`](dom/textsync) function.

This method _synchronously_ sets or gets the text content for the current matched element. It uses the [`dom/textSync()`](https://github.com/web-native/docs/tree/201e05f348ffbec3ef9e613f36713ffdc10badc9/play-ui/api/dom/textsync) function.

```javascript
// Set the text content
let instance = $('#el').textSync('Hello player!');

// Get the text content, log it to the console
let content = $('#el').textSync();
console.log(content);
```

See [`dom/textSync()`](dom/textsync) for details.

#### `.text()`

This is an alias of the [`.textAsync()`](#instancetextasync) method.

#### `.appendAsync()`

This method *asynchronously* appends text content to the current matched element. It uses the [`dom/appendAsync()`](dom/appendasync) function.

This method _asynchronously_ appends text content to the current matched element. It uses the [`dom/appendAsync()`](dom/appendasync) function.

```javascript
// Append content, do something else afterward
$('#el').appendAsync('Hello player!').then(instance => {
    // Do something else
});
```

See [`dom/appendAsync()`](dom/appendasync) for details.

#### `.appendSync()`

This method *synchronously* appends text content to the current matched element. It uses the [`dom/appendSync()`](dom/appendsync) function.

This method _synchronously_ appends text content to the current matched element. It uses the [`dom/appendSync()`](https://github.com/web-native/docs/tree/201e05f348ffbec3ef9e613f36713ffdc10badc9/play-ui/api/dom/appendsync) function.

```javascript
// Append content
let instance = $('#el').appendSync('Hello player!');
```

See [`dom/appendSync()`](dom/appendsync) for details.

#### `.append()`

This is an alias of the [`.appendAsync()`](#instanceappendasync) method.

#### `.prependAsync()`

This method *asynchronously* prepends text content to the current matched element. It uses the [`dom/prependAsync()`](dom/prependasync) function.

This method _asynchronously_ prepends text content to the current matched element. It uses the [`dom/prependAsync()`](dom/prependasync) function.

```javascript
// Prepend content, do something else afterward
$('#el').prependAsync('Hello player!').then(instance => {
    // Do something else
});
```

See [`dom/prependAsync()`](dom/prependasync) for details.

#### `.prependSync()`

This method *synchronously* prepends text content to the current matched element. It uses the [`dom/prependSync()`](dom/prependsync) function.

This method _synchronously_ prepends text content to the current matched element. It uses the [`dom/prependSync()`](dom/prependsync) function.

```javascript
// Prepend content
let instance = $('#el').prependSync('Hello player!');
```

See [`dom/prependSync()`](dom/prependsync) for details.

#### `.prepend()`

This is an alias of the [`.prependAsync()`](#instanceprependasync) method.

#### `.attrAsync()`

This method *asynchronously* sets or gets an attribute for the current matched element. It uses the [`dom/attrAsync()`](dom/attrasync) function.

This method _asynchronously_ sets or gets an attribute for the current matched element. It uses the [`dom/attrAsync()`](dom/attrasync) function.

```javascript
// Set an attribute, do something else afterward
$('#el').attrAsync('name', 'value').then(instance => {
    // Do something else
});

// Get an attribute, log it to the console when available
$('#el').attrAsync('name').then(value => {
    console.log(value);
});
```

See [`dom/attrAsync()`](dom/attrasync) for details.

#### `.attrSync()`

This method *synchronously* sets or gets an attribute for the current matched element. It uses the [`dom/attrSync()`](dom/attrsync) function.

This method _synchronously_ sets or gets an attribute for the current matched element. It uses the [`dom/attrSync()`](dom/attrsync) function.

```javascript
// Set an attribute
let instance = $('#el').attrSync('name', 'value');

// Get an attribute, log it to the console
let value = $('#el').attrSync('name');
console.log(value);
```

See [`dom/attrSync()`](dom/attrsync) for details.

#### `.attr()`

This is an alias of the [`.attrAsync()`](#instanceattrasync) method.

#### `.classAsync()`

This method is used to *asynchronously* set/unset the *class* attribute or add/remove entries on the *class* list for the current matched element. It uses the [`dom/classAsync()`](dom/classasync) function.

This method is used to _asynchronously_ set/unset the _class_ attribute or add/remove entries on the _class_ list for the current matched element. It uses the [`dom/classAsync()`](dom/classasync) function.

```javascript
// Add a class, do something else afterward
$('#el').classAsync('class1', true).then(instance => {
    // Do something else
});

// Remove a class
$('#el').classAsync('class1', false).then(instance => {
    // Do something else
});
```

See [`dom/classAsync()`](dom/classasync) for details.

#### `.classSync()`

This method is used to *synchronously* set/unset the *class* attribute or add/remove entries on the *class* list for the current matched element. It uses the [`dom/classSync()`](dom/classsync) function.

This method is used to _synchronously_ set/unset the _class_ attribute or add/remove entries on the _class_ list for the current matched element. It uses the [`dom/classSync()`](dom/classsync) function.

```javascript
// Add a class
let instance = $('#el').classAsync('class1', true);

// Remove a class
let instance = $('#el').classAsync('class1', false);
```

See [`dom/classSync()`](dom/classsync) for details.

#### `.class()`

This is an alias of the [`.classAsync()`](#instanceclassasync) method.

### CSS-Processing Methods

CSS-processing methods. A few of these methods come in a *sync* and *async* flavour, and a third unsiffixed flavour that is an alias to the *async* method.

#### `.cssAsync()`

This method *asynchronously* sets or gets CSS rules for the current matched element. It uses the [`css/cssAsync()`](css/cssasync) function.

This method _asynchronously_ sets or gets CSS rules for the current matched element. It uses the [`css/cssAsync()`](css/cssasync) function.

```javascript
// Set a CSS rule, do something else afterward
$('#el').cssAsync('color', 'blue').then(instance => {
    // Do something else
});

// Get a CSS rule, log it to the console when available
$('#el').cssAsync('color').then(value => {
    console.log(value);
});
```

See [`css/cssAsync()`](css/cssasync) for details.

#### `.cssSync()`

This method *synchronously* sets or gets CSS rules for the current matched element. It uses the [`css/cssSync()`](css/csssync) function.

This method _synchronously_ sets or gets CSS rules for the current matched element. It uses the [`css/cssSync()`](css/csssync) function.

```javascript
// Set a CSS rule
let instance = $('#el').cssSync('color', 'blue');

// Get a CSS rule, log it to the console
let value = $('#el').cssSync('color');
console.log(value);
```

See [`css/cssSync()`](css/csssync) for details.

#### `.css()`

This is an alias of the [`.cssAsync()`](#instancecssasync) method.

#### `.cssInline()`

This method returns one or more style properties from the current matched element's style attribute. It uses the [`css/readInline()`](css/readinline) function.

This method returns one or more style properties from the current matched element's style attribute. It uses the [`css/readInline()`](css/readinline) function.

```javascript
// Read an inline CSS rule
let value = $('#el').cssInline('color');
console.log(value);
```

See [`css/readInline()`](css/readinline) for details.

#### `.cssStylesheet()`

This method returns one or more style properties associated with the current matched element accross the document's stylesheets. It uses the [`css/readStylesheet()`](css/readstylesheet) function.

This method returns one or more style properties associated with the current matched element accross the document's stylesheets. It uses the [`css/readStylesheet()`](css/readstylesheet) function.

```javascript
// Read a CSS rule from stylesheet
let value = $('#el').cssStylesheet('color');
console.log(value);
```

See [`css/readStylesheet()`](css/readstylesheet) for details.

#### `.cssTransaction()`

This method establishes a CSS operation on the current matched element that can be rolledback safely. It uses the [`css/transaction()`](css/transaction) function.

This method establishes a CSS operation on the current matched element that can be rolledback safely. It uses the [`css/transaction()`](css/transaction) function.

```javascript
// Obtaine a Transaction instance
let transaction = $('#el').cssTransaction('color');

// Create a savepoint - savepoint1
// We can rollback to this point later
transaction.save();

// Restyle the element
$('#el').css('color', 'green');

// Rollback to anypoint after some time
setTimeout(() => {
    // Rollback all the way to the element's initial state
    transaction.rollback(0);
}, 2000);
```

See [`css/transaction()`](css/transaction) for details.

### Animation Methods

Methods for animation.

#### `.play()`

This method creates and play an animation on the current matched element. It uses the [`animation/play()`](animation/play) function.

This method creates and play an animation on the current matched element. It uses the [`animation/play()`](animation/play) function.

```javascript
// Play fadeout
$('#el').play([{opacity:1}, {opacity:0}], {duration:600}).then(instance => {
    console.log('The end!');
});
```

See [`animation/play()`](animation/play) for details.

### Event Methods

Methods for binding events and gestures.

#### `.on()`

This method binds an event/gesture handler to the current matched element. It uses the [`interaction/on()`](interaction/on) function.

This method binds an event/gesture handler to the current matched element. It uses the [`interaction/on()`](interaction/on) function.

```javascript
// Bind a "doubletap" gesture handler
$('#el').on('doubletap', event => {
    alert('You doubletapped me!');
});
```

See [`interaction/on()`](interaction/on) for details.

#### `.off()`

This method unbinds event/gesture handlers oreviously bound using the [`.on()`](#instanceon) method. It uses the [`interaction/off()`](interaction/off) function.

This method unbinds event/gesture handlers oreviously bound using the [`.on()`](./#instanceon) method. It uses the [`interaction/off()`](interaction/off) function.

```javascript
// Unbind "doubletap" gesture handlers
$('#el').off('doubletap';
```

See [`interaction/off()`](interaction/off) for details.

#### `.trigger()`

This method triggers event/gesture handlers oreviously bound using the [`.on()`](#instanceon) method. It uses the [`interaction/trigger()`](interaction/trigger) function.

This method triggers event/gesture handlers oreviously bound using the [`.on()`](./#instanceon) method. It uses the [`interaction/trigger()`](interaction/trigger) function.

```javascript
// Unbind "doubletap" gesture handlers
$('#el').trigger('doubletap';
```

See [`interaction/trigger()`](interaction/trigger) for details.

### Other Methods

Methods on cross-cutting concerns.

#### `.data()`

This method sets or gets custom data/values associated with the current matched element. It uses the [`dom/data()`](dom/data) function.

This method sets or gets custom data/values associated with the current matched element. It uses the [`dom/data()`](dom/data) function.

```javascript
// Set a value
let instance = $('#el').data('key', value);

// Get a value
let value = $('#el').data('key');
console.log(value);
```

See [`dom/data()`](dom/data) for details.

### Observers

Observer APIs.

#### `.onconnected()`

This method observes when the current matched element is added to the document or given context. It uses the [`dom/connectedCallback()`](dom/connectedcallback) function.

This method observes when the current matched element is added to the document or given context. It uses the [`dom/connectedCallback()`](dom/connectedcallback) function.

```javascript
// Observe "onconnected"
$('#el').onconnected(() => {
    console.log('I am now connected to the DOM!');
});
```

See [`dom/connectedCallback()`](dom/connectedcallback) for details.

#### `.ondisonconnected()`

This method observes when the current matched element is removed from the document or given context. It uses the [`dom/disonconnectedCallback()`](dom/disconnectedcallback) function.

This method observes when the current matched element is removed from the document or given context. It uses the [`dom/disonconnectedCallback()`](dom/disconnectedcallback) function.

```javascript
// Observe "ondisonconnected"
$('#el').ondisonconnected(() => {
    console.log('I am now disconnected from the DOM!');
});
```

See [`dom/disconnectedCallback()`](dom/disconnectedcallback) for details.

#### `.onmutated()`

This method observes when the current matched element is added to, or removed from the document or given context. It uses the [`dom/mutationCallback()`](dom/mutationcallback) function.

This method observes when the current matched element is added to, or removed from the document or given context. It uses the [`dom/mutationCallback()`](dom/mutationcallback) function.

```javascript
// Observe "onmutated"
$('#el').onmutated(connectedState => {
    // If connectedState === 1, connected else if connectedState === 0, disconnected
    console.log('I am now ' + (connectedState ? 'connected to' : 'disconnected from') + ' the DOM!');
});
```

See [`dom/mutationCallback()`](dom/mutationcallback) for details.

#### `.onattrchange()`

This method observes changes on attributes of the current matched element. It uses the [`dom/attrChangeCallback()`](dom/attrchangecallback) function.

This method observes changes on attributes of the current matched element. It uses the [`dom/attrChangeCallback()`](dom/attrchangecallback) function.

```javascript
// Observe "onattrchange"
$('#el').onattrchange(changes => {
    changes.forEach(change => {
        console.log(change);
    });
}, 'attrName');
```

See [`dom/attrChangeCallback()`](dom/attrchangecallback) for details.

#### `.onresize()`

This method observes changes on the size of the current matched element. It uses the [`ui/resizeCallback()`](ui/resizecallback) function.

This method observes changes on the size of the current matched element. It uses the [`ui/resizeCallback()`](ui/resizecallback) function.

```javascript
// Observe "onresize"
$('#el').onresize(newSize => {
    console.log(newSize);
});
```

See [`ui/resizeCallback()`](ui/resizecallback) for details.
