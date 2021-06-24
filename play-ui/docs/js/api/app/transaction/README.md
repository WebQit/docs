---
desc: Start a transaction on an element for operations that eventually might need to be rolled back.
---
# `.transaction()`

This method is used to start a transaction on an element for operations that eventually might need to be rolled back. It is a convenient way to use the Play UI's [Transaction](../classes/transaction) class.

## Syntax

```js
let transaction = $(el).transaction(props, readCallback, writeCallback);
```

**Parameters**

+ `props`: `String|Array` - A property or list of properties that describes the operation on the element.
+ `readCallback`: `function(el, props)` - A functation that is called to capture the state of the element using the listed *props*. See the [Transaction](../classes/transaction#constructor) class.
+ `writeCallback`: `function(el, record)` - A functation that is called to re-apply the recorded state of the element as previously derived by the `readCallback` function. See the [Transaction](../classes/transaction#constructor) class.

**Return**

+ `Transaction` - The [Transaction](../classes/transaction) instance.

## Usage

See the [Transaction](../classes/transaction#constructor) class.

------

## Static Usage

The `.transaction()` instance method is internally based on the standalone `app/transaction()` function which may be used statically.

### Import

```js
const { transaction } = $.app;
```
```js
import { transaction } from '@webqit/play-ui/src/app/index.js';
```

### Syntax

See [the general way to use Play UI's standalone functions](../../../quickstart#use-as-descrete-utilities)