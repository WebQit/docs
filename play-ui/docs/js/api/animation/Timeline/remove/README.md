# `.remove()`
This [Timeline](..) instance method removes an [animation](../../Ani) instance from the timeline.

## Syntax

```js
// Obtain a Timeline instance and call remove()
timeline.remove(ani);
```

### Parameters
+ `ani` - `Ani`: An instance of [Ani](../../Ani).

### Return
+ `undefined`

## Usage

```js
// The animation instance
let ani  = new Ani(el, {opacity:0});
// The timeline
let timeline  = new Timeline([ani]);

// Add
timeline.remove(ani);
```