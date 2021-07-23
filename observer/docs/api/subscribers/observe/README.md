---
desc: Observe operations on an object or array.
---
# `.observe()`

This method is used to observe operations on an object or array.

+ [Observe All Properties](#a-observe-all-properties)
+ [Observe a Path](#b-observe-a-path)
+ [Observe Multiple Paths](#c-observe-multiple-paths)

## a. Observe All Properties

Here, a callback is used to observe operations on all properties of an object or array.

### Syntax

```js
// Observe all properties
Observer.observe(obj, callback[, params = {}]);
```

+ **`obj:         Object|Array`** - An object or array.
+ **`callback:    Function`** - A callback function that receives the change events.

    **Syntax**

    ```js
    function(deltas, response) {}
    ```

    **Parameters**

    + **`deltas: Array`** - An array of *[Delta](../../core/Delta) objects* that each contains the details of a change.
    + **`response: Response`** - A *[Response](../../core/Response)* object.

    **Return Value**

    *See [Returning Responses Back to Operators](#returning-responses-back-to-operators) below.*

+ **`params:      Object`** - Additional parameters for the method.
    + **`type:    Boolean`** - The mutation type to observe. *See [Specifying a Mutation Type](#specifying-a-mutation-type) below*.
    + **`subtree:    Boolean`** - Whether to observe deep-tree changes. *See [Using the `params.subtree` Parameter](#using-the-paramssubtree-parameter) below*.
    + **`tags:    Array`** - Observer tags. *See [Tagging an Observer](#tagging-an-observer) below*.

**Return Value**

*See [return value](#the-returned-observer-instance) below.*

### Usage

*The callback function:*

```js
let callback = deltas => {
    deltas.forEach(delta => {
        console.log(delta.type, delta.name, delta.path, delta.value, delta.oldValue);
    });
};
```

*Case 1 - An object observer:*

```js
let obj = {};
Observer.observe(obj, callback);
```

The code above will report changes as the *object* gets modified on any of its properties.

```js
Observer.set(obj, 'fruit', 'apple');
```

With the *set* operation above, the value of `delta.name` and `delta.path` in the console will be `'fruit'` and `[ 'fruit' ]` respectively.

*Case 2 - An array observer:*

```js
let arr = [];
Observer.observe(arr, callback);
```

The code above will report changes as entries are added to, or removed from the *array*.

```js
Observer.set(arr, 0, 'apple');
```

With the *set* operation above, the value of `delta.name` and `delta.path` in the console will be `0` and `[ 0 ]` respectively.

If we made multiple changes in one batch, our observer would recieve multiple events at once - `deltas.length > 1`.

```js
Observer.set(obj, {
    fruit: 'orange',
    brand: 'apple',
});
```

#### Using the `params.subtree` Parameter

To observe changes on nested objects or arrays, we would use *the `params.subtree` parameter*.

*The object tree:*

```js
let obj = {
    preferences: {},
};
```

*The `params.subtree` parameter:*

```js
Observer.observe(obj, callback, { subtree: true });
```

The code above will report changes happening down the object tree.

```js
Observer.set(obj.preferences, 'fruits', []);
```

With the *set* operation at level 2 above, the value of `delta.name` and `delta.path` in the console will be `'preferences'` and `[ 'preferences', 'fruits' ]` respectively.

And we could specify the maximum depth in the subtree to be observed by setting the `params.subtree` to a number.

```js
Observer.observe(obj, callback, { subtree: 1 });
```

Now, changes happening further deep will not be reported.

```js
Observer.set(obj.preferences.fruits, 0, 'Apple');
```

## b. Observe a Path

An observer can be made to observe changes on a specific property - using a *property name*, or along a specific path in the object tree - using a *path expression*.

### Syntax

```js
// Observe a propertyName
Observer.observe(obj, propertyName, callback[, params = {}]);

// Observe a path
Observer.observe(obj, path, callback[, params = {}]);
```

**Parameters**

+ **`obj:          Object|Array`** - An object or array.
+ **`propertyName: String`** - A property name to observe.
+ **`path:         Array`** - A deep path to observe.
+ **`callback:    Function`** - A callback function that receives the change events.

    **Syntax**

    ```js
    function(delta, response) {}
    ```

    **Parameters**

    + **`delta: Delta`** - A *[Delta](../../core/Delta) object* that contains the details of a change.
    + **`response: Response`** - A *[Response](../../core/Response)* object.

    **Return Value**

    *See [handler return value](#handler-return-value) below.*

+ **`params:      Object`** - Additional parameters for the method.
    + **`type:    Boolean`** - The mutation type to observe. *See [Specifying a Mutation Type](#specifying-a-mutation-type) below*.
    + **`subtree:    Boolean`** - Whether to observe deep-tree changes. *See [Using the `params.subtree` Parameter](#using-the-paramssubtree-parameter) below*.
    + **`suptree:    Boolean`** - Whether to observe ancestor-level changes. *See [Using the `params.suptree` Parameter](#using-the-paramssuptree-parameter) below*.
    + **`tags:    Array`** - Observer tags. *See [Tagging an Observer](#tagging-an-observer) below*.

*See [return value](#the-returned-observer-instance) below.*

### Usage

*The callback function:*

```js
let callback = delta => {
    console.log(delta.type, delta.name, delta.path, delta.value, delta.oldValue);
};
```

*The object tree:*

```js
let obj = {
    preferences: {},
};
```

*The path expression:*

```js
Observer.observe(obj, [ 'preferences', 'fruits' ], callback);
```

The code above will report changes that occur at the specified path - `[ 'preferences', 'fruits' ]`.

```js
Observer.set(obj.preferences, 'fruits', []);
```

With the *set* operation at level 2 above, the value of `delta.name` and `delta.path` in the console will be `'preferences'` and `[ 'preferences', 'fruits' ]` respectively.

```js
Observer.set(obj.preferences, 'brands', []);
```

With the *set* operation at level 2 above, the value of `delta.name` and `delta.path` will be `'preferences'` and `[ 'preferences', 'brands' ]` respectively. This time, the change isn't happening along the path we're observing. And we don't see anything in the console.

#### Using the `params.subtree` Parameter

To observe changes even deeper down the path, we would use *the `params.subtree` parameter*. In this mode, the argument passed to an observer would be an array of *delta objects* instead of just a *delta object*.

*The callback function:*

```js
let callback = deltas => {
    deltas.forEach(delta => {
        console.log(delta.type, delta.name, delta.path, delta.value, delta.oldValue);
    });
};
```

*The object tree:*

```js
let obj = {
    preferences: {
        fruits: [],
        brands: [],
    },
};
```

*The `params.subtree` parameter:*

```js
Observer.observe(obj, [ 'preferences', 'fruits' ], callback, { subtree: true });
```

The code above will report changes happening at `[ 'preferences', 'fruits' ]` and further down.

```js
Observer.set(obj.preferences.fruits, 0, 'Orange');
```

With the *set* operation at level 3 above, the value of `delta.name` and `delta.path` in the console will be `'preferences'` and `[ 'preferences', 'fruits', 0 ]` respectively.

If we made multiple changes in one batch, our observer would recieve multiple events at once - `deltas.length > 1`.

```js
Observer.set(obj.preferences.fruits, {
    1: 'orange',
    2: 'apple',
});
```

And we could specify the maximum depth in the subtree to be observed.

*Setting the `params.subtree` to a number:*

```js
Observer.observe(obj, [ 'preferences', 'fruits' ], callback, { subtree: 1 });
```

Now, changes happening further deep will not be reported.

```js
Observer.set(obj.preferences.brands, 0, 'Apple');
```

#### Using the `params.suptree` Parameter

With a path, it is not only possible to observe *subtree* changes using *the `params.subtree` parameter*, we could also observe *super-tree* changes - changes happening up the path - using the `params.suptree` parameter.

For example, if we observed the path `[ 'preferences', 'fruits' ]`, any changes made to the root `[ 'preferences' ]` path itself would not be reported. But the `params.suptree` parameter will enable just that.

*An empty object:*

```js
let obj = {};
```

*The `params.suptree` parameter:*

```js
Observer.observe(obj, [ 'preferences', 'fruits' ], callback, { suptree: true });
```

Now, changes happening earlier in the path will be reported.

```js
Observer.set(obj, 'preferences', {});
```

Changes happening at the exact path will also be reported, as expected.

```js
Observer.set(obj.preferences, 'fruits', []);
```

Now, as with the `params.subtree` parameter, we could also specify a number, this time, as the upper threshold for the *super-tree* observation.

```js
Observer.observe(obj, [ 'preferences', 'fruits', 0 ], callback, { suptree: 1 });
```

Now, changes happening beyond this threshold will not be reported.

```js
Observer.deleteProperty(obj, 'preferences');
```

## c. Observe Multiple Paths

Multiple paths can be observed at once by specifying an array of path expressions. The observer is called with the paths along which an event happens.

### Syntax

```js
// Observe a path
Observer.observe(obj, [ path, ... ], callback[, params = {}]);
```

**Parameters**

+ **`obj:          Object|Array`** - An object or array.
+ **`path:         Array`** - A deep path to observe.
+ **`callback:     Function`** - A callback function that receives the change events.

    **Syntax**

    ```js
    function(deltas, response) {}
    ```

    **Parameters**

    + **`deltas: Array`** - An array of *[Delta](../../core/Delta) objects* that each contains the details of a change.
    + **`response: Response`** - A *[Response](../../core/Response)* object.

    **Return Value**

    *See [handler return value](#handler-return-value) below.*

+ **`params:      Object`** - Additional parameters for the method.
    + **`type:    Boolean`** - The mutation type to observe. *See [Specifying a Mutation Type](#specifying-a-mutation-type) below*.
    + **`subtree:    Boolean`** - Whether to observe deep-tree changes. *See [Using the `params.subtree` Parameter](#using-the-paramssubtree-parameter) below*.
    + **`suptree:    Boolean`** - Whether to observe ancestor-level changes. *See [Using the `params.suptree` Parameter](#using-the-paramssuptree-parameter) below*.
    + **`tags:    Array`** - Observer tags. *See [Tagging an Observer](#tagging-an-observer) below*.

**Return Value**

*See [return value](#the-returned-observer-instance) below.*

### Usage

*The callback function:*

```js
let callback = deltas => {
    deltas.forEach(delta => {
        console.log(delta.type, delta.name, delta.path, delta.value, delta.oldValue);
    });
};
```

*The object tree:*

```js
let obj = {
    preferences: {
        fruits: [],
        brands: [],
    },
};
```

*The paths:*

```js
let obj = {};
let path1 = [ 'preferences', 'fruits' ];
let path2 = [ 'preferences', 'brands', 1 ];
Observer.observe(obj, [
    path1,
    path2
], callback);
```

The code above will report changes as any of the paths recieve a change.

```js
Observer.set(obj.preferences, 'fruits', []);
```

If we made multiple changes in one batch, our observer would recieve multiple events at once - `deltas.length > 1`.

```js
Observer.set(obj.preferences, {
    fruits: [ 'orange' ],
    brands: [ 'apple' ],
});
```

#### Using Wildcard Paths

It is possible to observe multiple paths dynamically using one *wildcard path* expression. Wildcard paths are paths with empty slots, designed to match *event paths* as they happen.

*The object tree:*

```js
let obj = {
    preferences: {
        fruits: [],
        brands: [],
    },
};
```

*The wildcard path (notice below that this is a 3-level path but with an empty middle slot):*

```js
Observer.observe(obj, [ 'preferences', null, 0 ], callback);
```

The code above will report changes when an event fires at a path that fulfills our wildcard path. Now, both `[ 'preferences', 'fruits', 0 ]` and `[ 'preferences', 'brands', 0 ]` would do just that.

```js
Observer.set(obj.preferences.fruits, 0, 'mango');
```

If we made multiple changes in one batch, our observer would recieve multiple events at once - `deltas.length > 1`.

```js
Observer.set(obj.preferences.fruits, {
    0: 'orange',
    1: 'apple',
});
```

<!--
## Automatic Subtree Observability
-->

## Specifying Mutation Types

The `params.type` parameter can be used to tell an observer to respond to a specific mutation types like "set" and "del".

```js
Observer.observe(obj, path, callback, { type: 'del' });
```

## Tagging an Observer

The `params.tags` parameter can be used to tag an observer. Tags are an *array* of values (*strings*, *numbers*, *objects*, etc) that can be used to uniquely identify the observer for later retrieval. *See [`Observer.unobserve()`](../unobserve).*

```js
Observer.observe(obj, path, callback, { tags: [ '#tag' ] });
```

## The Returned Observer Instance

The `Observer.observe()` method returns an *Observer* instance with certain useful methods.

*Obtain the Observer instance:*

```js
let instance = Observer.observe(obj, callback);
```

*Synthetically fire the Observer handler:*

```js
instance.fire({
    type:'set',
    name: 'propertyName'
    value:'...',
});
```

*Disconnect the observer:*

```js
instance.disconnect();
```

## Returning Responses Back to Operators

Observers may respond to an event from their callback functions by calling an appropriate method on the [Response](../../core/Response) object they recieve on their second parameter, or by returning an equivalent return value. The initiator of the operation may obtain the *Response* object to inspect the state of the object.

**`response.stopPropagation()`:** Calling this method stops further processing of the operation, that is, stops the event from reaching other event handlers, and prevents the initiator of the operation from taking any default action that it normally would take after the operation. Returning `false` from the handler has the same effect. 

```js
Observer.observe(obj, propertyName, (delta, response) => {
    response.stopPropagation();
    // Or, return false;
});
```

The initiator of the operation would need to have flagged the event as *cancellable* for the above to be honoured.

```js
Observer.deleteProperty(obj, propertyName, {
    cancellable: true,
});
```

The initiator may also obtain the response object to determine the state of the responses made by observers. Notice the `params.responseObject` parameter that tells `Observer.deleteProperty()` to return a *response object* for use.

```js
let response = Observer.deleteProperty(obj, propertyName, {
    cancellable: true,
    responseObject: true,
});
```

*Now, it determines the state of the responses made by observers:*

```js
// Determine response state...
if (response.propagationStopped) {
}
```

**`response.preventDefault()`:** Calling this method prevents the initiator of the operation from taking any default action that it normally would take after the operation. Returning `false` from the handler has the same effect. (The event still reaches other handlers.)

```js
Observer.observe(obj, propertyName, (delta, response) => {
    response.preventDefault();
    // Or, return false;
});
```

The initiator of the operation may obtain the response object to determine the state of this response.

```js
// Determine response state...
if (response.defaultPrevented) {
}
```

**response.waitUntil(promise)** Calling this method tells the initiator of the operation to wait until a *Promise* is resolved before continuing with further operations. Returning a `Promise` from the handler has the same effect. (The event still reaches other handlers without waiting.)

```js
Observer.observe(obj, propertyName, (delta, response) => {
    let promise = new Promise(resolve => {
        setTimeout(resolve, 2000);
    });
    response.waitUntil(promise);
    // Or, return promise;
});
```

The initiator of the operation may obtain the response object to determine the state of this response. The state of this response becomes a promise when one or more handlers respond with a promise.

```js
// Determine response state...
if (response.promises) {
    response.promises.then(() => {
    });
}
```

## Related Methods

+ [`Observer.unobserve()`](../unobserve)