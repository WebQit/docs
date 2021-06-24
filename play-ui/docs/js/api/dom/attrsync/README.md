---
desc: Set or get an element's attribute.
---
# `.attrSync()`

This method is used to set or get an element's attribute. It is a shorter alternative to the native [`Element.setAttribute()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/setAttribute), [`Element.getAttribute()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/getAttribute), and [`Element.removeAttribute()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/removeAttribute). It also has special support for list-based attributes like `class`.

The suffix *Sync* differentiates this method from its *Async* counterpart - [`attrAsync()`](../attrasync). Unlike the *Async* counterpart, this method is promised-based and works in sync with the UI's reflow cycle. See [Async UI](../../concepts#async-ui).

+ [Set Attributes](#a-set-attributes)
+ [Get Attributes](#b-get-attributes)
+ [Unset Attributes](#c-unset-attributes)
+ [Modyfying Delimited Attributes](#d-modyfying-delimited-attributes)

## a. Set Attributes

### Syntax

```js
// Set a single attribute
$(el).attrSync(name, value);

// Set multiple attributes
$(el).attrSync({
    [name]: value,
});
```

**Parameters**

* `name`: `String` - The attribute name to set.
* `value`: `String|Boolean` - The attribute value to set. When `true`, the string `"true"` is set on the attribute. When `false`, the attribute is unset from the element; [see below](#unset-attributes).

**Return**

* `this` - The Play UI instance.

### Usage

Set the ID attribute on an `<input />` element. Then set other attributes.

```js
// Set a single attribute
$(el).attrSync('id', 'email-input');
$(el).attrSync({
    type: 'email',
    required: true,
});
```

## b. Get Attributes

### Syntax

```js
// Get a single attribute
let attribute = $(el).attrSync(name);

// Get multiple attributes
let attributes = $(el).attrSync([...name]);
```

**Parameters**

* `name`: `String` - The attribute name.

**Return**

+ `value`: `Any` - The value of the named attribute.
+ `values`: `Object` - A key/value hash of the listed attributes.

### Usage

Get the attribute on an `<input />` element.

```js
// Set a single attribute
let value = $(el).attrSync('id');
// email-input
```

## c. Unset Attributes

### Syntax

```js
// Remove a single attribute
$(el).attrSync(name, false);

// Remove multiple attributes
$(el).attrSync({
    [name]: false,
});
```

**Parameters**

* `name`: `String` - The attribute name.

**Return**

+ `this` - The Play UI instance.

### Usage

Unset an element's ID attribute.


```js
$(el).attrSync('id', false);
```

## d. Modyfying Delimited Attributes

### Syntax

```js
// Add a member to a single delimited attribute
$(el).attrSync(name, member, mutation === true);

// Add a member to multiple delimited attributes
$(el).attrSync({
    [name]: member,
}, mutation === true);

// Remove a member from a single delimited attribute
$(el).attrSync(name, member, mutation === false);

// Remove a member from multiple delimited attributes
$(el).attrSync({
    [name]: member,
}, mutation === false);
```

**Parameters**

* `name`: `String` - The attribute name to modify.
* `member`: `String` - The attribute member to add or remove.
* `mutation`: `Boolean` - The *add/remove* directive. When `true`, the given string is added to the attribute's value list. When `false`, the given string is removed from the attribute's value list.

**Return**

* `this` - The Play UI instance.

### Usage

Modify an element's *class* attribute, then confirm the operation.

```html
<div class="class1 class2" role="article"></div>
```

```js
let el = document.querySelector('.class1');
// Insert a class entry
$(el).attrSync('class', 'class3', true);
// Confirm the operation
console.log($(el).attrSync('class')); // class1 class2 class3
```

------

## Static Usage

The `.attrSync()` instance method is internally based on the standalone `dom/attrSync()` function which may be used statically.

### Import

```js
const { attrSync } = $.dom;
```
```js
import { attrSync } from '@webqit/play-ui/src/dom/index.js';
```

### Syntax

See [the general way to use Play UI's standalone functions](../../../quickstart#use-as-descrete-utilities)