---
desc: Apply a new state to the element in a transaction.
---
# `.apply()`

> The `transaction.apply()` method.

This method is used to apply a new state to the element in a transaction. It is just another way to call the [`writeCallback`](../#constructor) function that was used to create the instance.

## Syntax

```js
transaction.apply(state);
```

**Parameters**

+ `state`: `Object` - A new state object that is passed to the instance's [`writeCallback`](../#constructor).

**Return**

+ `Any` - The return value of the `writeCallback` function.

## Usage

See the [Transaction](../#usage) class.