---
desc: Commit up to a savepoint in a transaction.
---
# `.commit()`

> The `transaction.commit()` method.

This method is used to commit up to a savepoint in a transaction. It is used to certify that all records up to the specified savepoint are permanent changes.

## Syntax

```js
transaction.commit(savepoint = 0);
```

+ `savepoint`: `Int` - The savepoint to commit up to. `0` refers to the record of the first call to [`.savepoint()`](../savepoint); `1` to the second call; and so on.

**Return**

+ `this` - The [Transaction](../) instance.

## Usage

See the [Transaction](../#usage) class.