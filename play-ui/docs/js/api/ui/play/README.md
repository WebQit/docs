---
desc: Creates and play an animation.
---
# `Animation/play()`
This function creates and play an animation. It is a convenient way to use the PlayUI's [Ani](../Ani) class.

## Import

```javascript
import play from '@webqit/play-ui/src/interaction/play.js';
```

## Syntax

```javascript
let promise = play(el, effect[, timing = {}]);
```

### Parameters
+ `el` - `HTMLElement`: The source DOM element.
+ `effect` - `Array|Object|String`: The effect to play. This could be a standard keyframes array, a CSS object or a stylesheet-based animation name.
+ `timing` - `Object`: Options for the animation. See [Ani](../Ani#parameters).

### Return

* `Promise` - A promise that resolves when the animation finishes.

## Usage

### Play from Standard Keyframes

Below, we fade out an element with keyframes.

```javascript
let el = document.querySelector('#el');
play(el, [{opacity:1}, {opacity:0}], {duration:600}).then(el => {
    console.log('The end!');
});
```

### Play a CSS Transition

Below, we fade out an element by simply specifying a end-state keyframe and letting Ani derive the starting keyframe from the element's current state.

```javascript
play(el, {opacity:0}, {duration:600}).then(el => {
    console.log('The end!');
})
```

### Play a CSS Animation Name

Below, we fade out an element with an animation keyframes defined in the document's stylesheet.

```markup
<style>

@keyframes fadeout {
  0% { opacity: 1;}
  100% { opacity: 0;}
}

</style>
```

```javascript
play(el, 'fadeout', {duration:600}).then(el => {
    console.log('The end!');
})
```

