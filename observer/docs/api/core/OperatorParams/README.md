# `OperatorParams`

This is an object of additional parameters that may be passed with any *operator*; e.g. `Observer.set()`,  `Observer.deleteProperty()`, etc.

## Properties

+ **`detail:   Any`** - An optional value to pass to observers. *See [Passing a Value to Observers](#passing-a-value-to-observers) below.*
+ **`responseObject:   Boolean`** - Whether to return the *responseObject* for the operation. *See the section [Returning Responses Back to Operators](../../subscribers/observe#returning-responses-back-to-operators) at [`Observer.observe()`](../../subscribers/observe).*
+ **`cancellable:      Boolean`** - Whether the default taken after this operation is cancellable by observers. *See the section [Returning Responses Back to Operators](../../subscribers/observe#returning-responses-back-to-operators) at [`Observer.observe()`](../../subscribers/observe).*

## Passing a Value to Observers

The `params.detail` property can be used to pass a value specifically to observers that might be responding to an operation like the *deleteProperty* operation. Any type of value can be passed.

```js
Observer.deleteProperty(obj, propertyName, {
    detail: 'This is observer-specific detail',
});
```

The *detail* above would now be available to every observer.

```js
Observer.observe(obj, propertyName, event => {
    console.log(event.detail);
});
```
