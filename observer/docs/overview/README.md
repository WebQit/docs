---
desc: An overview.
_index: first
---
# Overview

Take a rundown of the Observer API.

## Observers

*Observer* lets us observe operations on any object or array...

```js
let obj = {};
```

...using the [`Observer.observe()`](../api/subscribers/observe) method.

```js
Observer.observe(obj, changes => {
    changes.forEach(c => {
        console.log(c.type, c.name, c.path, c.value, c.oldValue);
    });
});
```

Now changes will be delivered *synchronously* - as they happen.

## Operators

Programmatically make *reactive* changes using the *[native-like](../concepts#with-javascripts-reflection-apis)* [set of operators](../api/operators)...

```js
// A single set operation
Observer.set(obj, 'prop1', 'value1');
```
```js
// A batch set operation
Observer.set(obj, {
    prop2: 'value2',
    prop3: 'value3',
});
```

...or switch to using *object accessors* - using the [`Observer.accessorize()`](../api/operators/accessorize) method...

```js
// Accessorize all (existing) properties
Observer.accessorize(obj);
// Accessorize specific properties (existing or new)
Observer.accessorize(obj, ['prop1', 'prop5', 'prop9']);
```
```js
// Make reactive operations
obj.prop1 = 'value1';
obj.prop5 = 'value5';
obj.prop9 = 'value9';
```

...or even go with a *reactive Proxy* of your object, and imply properties on the fly.

```js
// Obtain a reactive Proxy
let _obj = Observer.proxy(obj);
```
```js
// Make reactive operations
_obj.prop1 = 'value1';
_obj.prop4 = 'value4';
_obj.prop8 = 'value8';
```

And no problem if you inadvertently cascade the approaches. No bizzare behaviours.

```js
// Accessorized properties are already reactive
Observer.accessorize(obj, ['prop1', 'prop6', 'prop10']);

// But no problem if we still want a proxy over an accessorized object
let _obj = Observer.proxy(obj);

// And yet no problem if we still made a programmatic call over an already reactive Proxy
Observer.set(_obj, 'prop1', 'value1');
```

## Interceptors

How about some level of indirection - the ability to hook into operators like `Observer.set()` and  `Observer.deleteProperty()` to repurpose their operation? That's all possible - using the [`Observer.intercept()`](../api/subscribers/intercept) method!

Below, we catch any attempt to set an URL and force it to an HTTPS URL.

```js
Observer.intercept(obj, 'set', (event, previous, next) => {
    if (event.name === 'url' && event.value.startsWith('http:')) {
        event.value = event.value.replace('http:', 'https:');
    }
    return next();
});
```

Now, only the first of the following will fly as-is.

```js
Observer.set(obj, 'url', 'https://webqit.io');
```
```js
Observer.set(obj, 'url', 'http://webqit.io');
```

## Cleaning Up

You'll sometimes want to clean up? There are the methods for that!

+ [`Observer.unobserve()`](../api/subscribers/unobserve)
+ [`Observer.unintercept()`](../api/subscribers/unintercept)
+ [`Observer.unproxy()`](../api/operators/unproxy)
+ [`Observer.unaccessorize()`](../api/operators/unaccessorize)

## But Beyond this Point Lies...

Everything from the [download](../download) page, to the [concepts](../concepts) page, to the [API Reference](../api).