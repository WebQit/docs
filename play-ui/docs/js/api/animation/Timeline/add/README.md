# `.add()`
This [Timeline](..) instance method adds a new [animation](../../Ani) instance to the timeline.

## Syntax

```js
// Obtain a Timeline instance and call add()
timeline.add(ani);
```

### Parameters
+ `ani` - `Ani`: An instance of [Ani](../../Ani).

### Return
+ `undefined`

## Usage

```js
// The timeline
let timeline  = new Timeline;
// The animation instance
let ani  = new Ani(el, {opacity:0});
// Add
timeline.add(ani);
```