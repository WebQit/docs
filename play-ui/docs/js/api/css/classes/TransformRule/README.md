---
title: TransformRule
desc: The TransformRule class.
---
# `class TransformRule {}`

This class is an object model of the CSS transform rule.

## `TransformRule.parse()`

This function is used to parse an element's computed transform rule into an instance of `TransformRule`.

### Syntax

```js
let transformRuleObject = TransformRule.parse(rule);
```

**Parameters**
+ `rule` - `String`: A CSS transform rule.

**Return**
+ `TransformRule` - An object model of the parsed transform rule.

## Usage

```html
<style>
div {
    transform: translate(30, 40) scale(3);
}
</style>
<div id="el"></div>
```

```js
let transformRule = document.querySelector('#el').style.transform;

// Set attribute
let transformRuleObject = TransformRule.parse(transformRule);

// Show
console.log(transformRuleObject);
/**
{
    translate: [30, 40],
    sclae: 3,
}
*/

//Convert to string
console.log(transformRuleObject.toString());
// translate(30, 40) scale(3)
```

We can also create a CSS transfrom rule from an object.

```js
let transformRuleObject = new TransformRule({
    translate: [30, 40],
    sclae: 3,
});

//Convert to string
console.log(transformRuleObject.toString());
// translate(30, 40) scale(3)
```