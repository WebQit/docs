---
desc: Asynchronously set or get an element's attribute.
---
# `.attrAsync()`

This method is used to asynchronously set or get an element's attribute. It is a shorter alternative to the native [`Element.setAttribute()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/setAttribute), [`Element.getAttribute()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/getAttribute), and [`Element.removeAttribute()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/removeAttribute). It also has special support for list-based attributes like `class`.

The suffix *Async* differentiates this method from its *Sync* counterpart - [`attrSync()`](../attrsync). Unlike the *Sync* counterpart, this method is promised-based and works in sync with the UI's reflow cycle. See [Async UI](../../concepts#async-ui).

+ [Set Attributes](#a-set-attributes)
+ [Get Attributes](#b-get-attributes)
+ [Unset Attributes](#c-unset-attributes)
+ [Modyfying Delimited Attributes](#d-modyfying-delimited-attributes)

## a. Set Attributes

### Syntax

```js
// Set a single attribute
$(el).attrAsync(name, value);

// Set multiple attributes
await $(el).attrAsync({
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
$(el).attrAsync('id', 'email-input').then($el => {
    $el.attrAsync({
        type: 'email',
        required: true,
    });
});
```

## b. Get Attributes

### Syntax

```js
// Get a single attribute
let attribute = await $(el).attrAsync(name);

// Get multiple attributes
let attributes = await $(el).attrAsync([...name]);
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
let value = await $(el).attrAsync('id');
// email-input
```

## c. Unset Attributes

### Syntax

```js
// Remove a single attribute
$(el).attrAsync(name, false);

// Remove multiple attributes
await $(el).attrAsync({
    [name]: false,
});
```

**Parameters**

* `name`: `String` - The attribute name.

**Return**

* `this` - The Play UI instance.

### Usage

Unset an element's ID attribute.

```js
$(el).attrAsync('id', false);
```

## d. Modyfying Delimited Attributes

### Syntax

```js
// Add a member to a single delimited attribute
$(el).attrAsync(name, member, mutation === true);

// Add a member to multiple delimited attributes
$(el).attrAsync({
    [name]: member,
}, mutation === true);

// Remove a member from a single delimited attribute
$(el).attrAsync(name, member, mutation === false);

// Remove a member from multiple delimited attributes
await $(el).attrAsync({
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
$(el).attrAsync('class', 'class3', true).then($el => {
    // Confirm the operation
    console.log($el.attrSync('class')); // class1 class2 class3
});
```

------

## Static Usage

The `.attrAsync()` instance method is internally based on the standalone `dom/attrAsync()` function which may be used statically.

### Import

```js
const { attrAsync } = $.dom;
```
```js
import { attrAsync } from '@webqit/play-ui/src/dom/index.js';
```

### Syntax

See [the general way to use Play UI's standalone functions](../../../quickstart#use-as-descrete-utilities)