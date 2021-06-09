# The Timeline Class
This class provides a convenient way to control multiple animation ([Ani](../Ani) instances.

## Import

```js
import Timeline from '@webqit/play-ui/src/ani/Timeline.js';
```

## Syntax

```js
let timeline = new Timeline(animations[, params = {}]);
```

### Parameters
+ `animations` - `Array`: Zero or more [Ani](../Ani) instances.
+ `params` - `Object`: A few parameters for the timeline. (Currently none).

## Usage
Play/pause multiple animations in one call.

```js
import Ani from '@webqit/play-ui/src/ani/Ani.js';

let ani1 = new Ani(el1, [{opacity:1}, {opacity:0}], {duration:600});
let ani2 = new Ani(el2, [{width:0}, {width:100}], {duration:900});

let timeline = new Timeline([ani1, ani2]);
timeline.pause();
setTimeout(() => {
   timeline.play().then(() => {
        console.log('The end; all animations!');
    });
}, 1000);
```

## Features

+ **Runtime manipulation of timeline.** *Timeline* allows you to add/remove animation instances at runtime without altering coordination and synchronization.
    ```js
    // Fade out from current opacity level
    let timeline = new Timeline([ani1, ani2]);
    // Kick off
    timeline.play().then(() => {
        console.log('The end; all animations!');
    });
    // On the fly
    timeline.add(ani3);
    timeline.remove(ani1);
    // Yet, our play.then() will work as expected
    ```

## Methods
+ [`.add()`](add) - Adds a new *Ani* instance to the timeline.
+ [`.remove()`](remove) - Removes an *Ani* instance from the timeline.
+ [`.onfinish()`](onfinish) - Accepts a callback that runs when all animations are completed.
+ [`.oncancel()`](oncancel) - Accepts a callback that runs when any of the animations is cancelled.
+ [`.progress()`](progress) - Returns the average percentage progress over all animations at any point during the animation.
+ [`.seek()`](seek) - Seeks each animation to the specific percentage of playback.
+ [`.reverse()`](reverse) - Toggles the playback direction for all animations. Animations added after this call are either reversed automatically or left as-is, depending on whether the last call to `.reverse()` implied *new reverse* or *reverse to original direction*.
+ [`.play()`](play) - Plays all animations and returns a promise that resolves when the animations complete. Animations added after this call are played automatically.
+ [`.pause()`](pause) - Pauses all animations. Animations added after this call are paused automatically.
+ [`.finish()`](finish) - Forces all animations to their finished state. Animations added after this call are forced to finish automatically.
+ [`.cancel()`](cancel) - Cancels all animations. Animations added after this call are canclled automatically.
+ [`.clear()`](clear) - Clears the timeline of all animations. 
