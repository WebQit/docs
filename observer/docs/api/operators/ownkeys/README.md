---
desc: Get an object's list of direct properties.
---
# `.ownKeys()`

This method is used to get an object's list of direct properties. It corresponds to the JavaScript's [`Reflect.ownKeys()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect/ownKeys) function. `Observer.ownKeys()` offers reactivity over this operation.

## Syntax

```js
// Show all keys.
let keys = Observer.ownKeys(obj[, params = {}]);
```

**Parameters**

+ **`obj:             Object|Array`** - an object or array.
+ **`params:          OperatorParams`** - Additional parameters for the operation. *See [OperatorParams](../../core/OperatorParams). Note that the `params.responseObject` parameter cannot be used with `Observer.ownKeys()`.*

**Return Value**

+ **`keys:            Array`**

## Usage

```js
let obj = {
    fruit:'orange',
    brand:'apple',
};
```

```js
let keys = Observer.ownKeys(obj);
```

## Usage as a Trap's "ownKeys" Handler

`Observer.ownKeys()` can be used as the "ownKeys" handler in Proxy traps.

```js
let _obj = new Proxy(obj, { ownKeys: Observer.ownKeys });
let _arr = new Proxy(arr, { ownKeys: Observer.ownKeys });
```

*Show keys* operations will now be forwarded to `Observer.ownKeys()` , triggering any [*interceptors*](../../../core/overview/interceptors) that may be bound to the object.

```js
let keys = Reflect.ownKeys(_obj);
let keys = Reflect.ownKeys(_arr);
```

## Intercepting `Observer.ownKeys()`

Using [`Observer.intercept()`](../../subscribers/intercept), it is possible to intercept calls to `Observer.ownKeys()`.

```js
Observer.intercept(obj, 'keys', (event, previous, next) => {

    // The read operation
    return Reflect.ownKeys(obj);
});
```

```js
let keys = Observer.ownKeys(obj);
```

## Related Methods

+ [`Observer.observe()`](../../subscribers/observe)
+ [`Observer.intercept()`](../../subscribers/intercept)
