---
desc: Modify an element's class attribute.
---
# `.classSync()`

This method is used to modify an element's class attribute. It provides convenience over using the [`attrSync()`](../attrsync) method to modify an element's class attribute.

The suffix *Sync* differentiates this method from its *Async* counterpart - [`classAsync()`](../classasync). Unlike the *Async* counterpart, this method is promised-based and works in sync with the UI's reflow cycle. See [Async UI](../../concepts#async-ui).

+ [Set Whole Class Attribute](#a-set-whole-class-attribute)
+ [Get Whole Class Attribute](#b-get-whole-class-attribute)
+ [Unset Whole Class Attribute](#c-unset-whole-class-attribute)
+ [Modify Member Classes](#d-modify-member-classes)

## a. Set Whole Class Attribute

### Syntax

```js
// Set the whole class attribute
$(el).classSync(classList);
```

**Parameters**

* `classList`: `String` - The whole class attribute.

**Return**

* `this` - The Play UI instance.

### Usage

Set whole class attribute of an element, then confirm operation. (Existing attribute value is replaced.)

```html
<div class="class1 class2"></div>
```

```js
let el = document.querySelector('.class1');
$(el).classSync('class3 class4');
// Confirm operation
console.log($(el).attrAsync('class')); // class3 class4
```

## b. Get Whole Class Attribute

### Syntax

```js
// Get the whole class attribute
let classList = $(el).classSync();
```

**Return**

* `classList`: `String` - The whole class attribute.

### Usage

Get whole class attribute of an element.

```html
<div class="class1 class2"></div>
```

```js
let el = document.querySelector('.class1');
console.log($(el).classSync(); // class1 class2
```

## c. Unset Whole Class Attribute

### Syntax

```js
// Unset the whole class attribute
$(el).classSync(false);
```

**Return**

* `this` - The Play UI instance.

### Usage

Unset whole class attribute of an element, then confirm operation.

```html
<div class="class1 class2"></div>
```

```js
let el = document.querySelector('.class1');
$(el).classSync(false);
// Confirm operation
console.log($(el).attrSync('class')); // undefined
```

## d. Modify Member Classes

### Syntax

```js
// Add member classes
$(el).classSync(classList, mutation === true);

// Remove member classes
$(el).classSync(classList, mutation === false);
```

**Parameters**

* `classList`: `String` - The class list to add or remove.
* `mutation`: `Boolean` - The *add/remove* directive. When `true`, the given string is added to the class list. When `false`, the given string is removed from the class list.

**Return**

* `this` - The Play UI instance.

### Usage

Add member classes to an element, then confirm operation.

```html
<div class="class1 class2"></div>
```

```js
let el = document.querySelector('.class1');
$(el).classSync('class3 class4', true);
// Confirm operation
console.log($(el).attrSync('class')); // class1 class2 class3 class4
```

------

## Static Usage

The `.classSync()` instance method is internally based on the standalone `dom/classSync()` function which may be used statically.

### Import

```js
const { classSync } = $.dom;
```
```js
import { classSync } from '@webqit/play-ui/src/dom/index.js';
```

### Syntax

See [the general way to use Play UI's standalone functions](../../../quickstart#use-as-descrete-utilities)