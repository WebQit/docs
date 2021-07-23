---
desc: Reactively define a property on an object or array or reconfigures an existing property.
---
# `.defineProperty()`

This method is used to reactively define a property on an object or array or reconfigures an existing property. It corresponds to the native [`Reflect.defineProperty()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect/defineProperty). `Observer.defineProperty()` offers reactivity over this operation.

The `Observer.def()` function is an alias of this function and can be used interchangeably.

## Syntax

```js
// Define a property
Observer.defineProperty(obj, propertyName, propertyDescriptor[, params = {}]);

// Define a list of properties
Observer.defineProperty(obj, [ propertyName, ... ], propertyDescriptor[, params = {}]);
```

**Parameters**

+ **`obj:                 Object|Array`** - An object or array.
+ **`propertyName:        String`** - The property to define. If multiple, all listed properties will be defined with the same *propertyDescriptor*.
+ **`propertyDescriptor:  Object`** - The property descriptor as specified for [`Reflect.defineProperty()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect/defineProperty).
+ **`params:          OperatorParams`** - Additional parameters for the operation. *See [OperatorParams](../../core/OperatorParams).

**Return Value**

+ **Boolean** - This is either `true` or `false` on the outcome of the operation.
+ **Response** - The returned [Response](../../../core/Response) object for the operation, where the `params.responseObject` is set to `true`. *See the section [Returning Responses Back to Operators](../../subscribers/observe#returning-responses-back-to-operators) at [`Observer.observe()`](../../subscribers/observe).*

## Usage

*Case 1 - Define a specific property:*

```js
// On an object
Observer.defineProperty(obj, 'fruit', { value: 'orange' });
```

*Case 2 - Define a list of properties:*

```js
// On an object
Observer.defineProperty(obj, [ 'fruit', 'brand' ], { value: 'orange' });
```

## Usage as a Trap's defineProperty Handler

`Observer.defineProperty()` returns a *Boolean* value by default, and can, therefore, be used as the "defineProperty" handler in Proxy traps.

```js
let _obj = new Proxy(obj, {defineProperty: Observer.defineProperty});
let _arr = new Proxy(arr, {defineProperty: Observer.defineProperty});
```

*Define* operations will now be forwarded to `Observer.defineProperty()`, triggering any [*interceptors*](../../../core/overview/interceptors) and [*observers*](../../../core/overview/observers) that may be bound to the object.

```js
Reflect.defineProperty(_obj, 'fruit', { value:'apple' });
```

## Intercepting `Observer.defineProperty()`

Using [`Observer.intercept()`](../../subscribers/intercept), it is possible to intercept calls to `Observer.defineProperty()`. During a "defineProperty" operation, interceptors will receive an event object containing the property name being defined and the descriptor object.

```js
Observer.intercept(obj, 'def', (event, previous, next) => {
    
    // What we recieved...
    console.log(event.name, event.descriptor);

    // Reconfigure the descriptor that the next
    // interceptor (or the default handler) sees
    let _descriptor = { ...event.descriptor };
    if (_descriptor.configurable === true) {
        _descriptor.configurable = false;
    }

    return next(_descriptor);
});
```

```js
Observer.defineProperty(obj, 'fruit', {
    value:'apple',
    configurable: true,
});
```

The interceptor is expected to return *true* if the custom operation was successful; *false* otherwise.

When the "defineProperty" operation is of multiple properties, the interceptor gets fired for each property while also recieving the total list of properties as a hint - via `event.related`.

```js
Observer.intercept(obj, 'def', (event, previous, next) => {
    
    // What we recieved...
    console.log(event.name, event.related);

    // Just forward the operation
    return next();
});
```

```js
Observer.deleteProperty(obj, [ 'fruit', 'brand' ], { value:'apple' });
```

The above should trigger our interceptor twice with `event.related` being `['fruit', 'brand']` each time.

## Related Methods

+ [`Observer.observe()`](../../subscribers/observe)
+ [`Observer.intercept()`](../../subscribers/intercept)
