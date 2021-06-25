---
desc: Create a live list within an element in an OOHTML-based document.
---
# `.itemize()`

This method is used to create a live list within an element in an [OOHTML](/tooling/oohtml)-based document. (It requires the OHHTML polyfill to be available in the document.)

## Syntax

```js
$(el).itemize(items[, renderCallback[, overflowCallback[, params]]]);
```

**Parameters**

+ `items`: `Array` - A list of data items to populate into a list container.
+ `renderCallback`: `function(directive, itemElement, itemData, index[, isUpdate])` - An optional callback that is called to handle rendering or *unrendering* of `itemData`.

    **Parameters**

    + `directive`: `String` - This will be `render` when `itemData` is to be rendered into the given `itemElement`. It will be `unrender` when `itemData` is to be disposed off the given `itemElement`.
    + `itemElement`: `Element` - A newly created *list item element*, or an existing *list item element*, that corresponds to the current `itemData`.
    + `itemData`: `Any` - The current entry in the input `items` associated with `itemElement`.
    + `index`: `Int` - The index of `itemData` in the input `items`.
    + `isUpdate`: `Boolean` - A flag that indicates whether or not the given `itemData` is an update to a previous entry in the input `items`. This flag is only supplied when `directive` is `render`.

    **Return**

    `viod|false` - A return value is not required for this function. But `false` can be returned to tell `temize()` that `itemData` has been successfully *rendered/unrendered*. Otherwise, `temize()` will automatically *render/unrender* `itemData` using the `itemElement`'s `setState()` and `clearState()` methods respectively.

+ `overflowCallback`: `function(directive, itemElement, itemData, index, collapsedCount)` - An optional callback that is called to handle overflow events. When omitted, `temize()` doesn't watch any items for overflow.

    **Parameters**

    + `directive`: `String` - This will be `collapse` when `itemElement` is at the verge of overflowing the list container. It will be `restore` when `itemElement` can be again restored from its *collapsed* state.
    + `itemElement`: `Element` - The *list item element* to be collapsed or restored.
    + `itemData`: `Any` - The current entry in the input `items` associated with `itemElement`.
    + `index`: `Int` - The index of `itemData` in the input `items`.
    + `collapsedCount`: `Int` - The number of items still in the *collapsed* state. This will be `0` if `itemElement` is the last element in *collapsed* state.

    **Return**

    `viod|Promise` - A return value is not required for this function. But a `Promise` can be returned to tell `temize()` to wait while collapsing or restoring `itemElement`.

+ `params`: `Object` - An optional list of additional parameters for the operation.
    + `live`: `String` - Specifies whether or not for `temize()` to observe input `items` for mutations. This is `true` by default.
    + `itemIndexAttribute`: `String` - The attribute name on `itemElement` on which to set the index of the item. This defaults to `data-index`.
    + `itemExportId`: `String` - The *export ID* used for importing *item elements* for each item in the input `items`. When this sub parameter is omitted, the fragment identifier found in the module reference on the list container is used. Otherwise, the *default* export of the  referenced module is used.
    + `parentalOverflowBounds`: `Boolean` - Whether or not to use the container of the list container or the list container itself as the overflow bounds. By default, the container of the list container is used. Set to `false` to use the list container itself.
    + `orientation`: `String` - This is either `horizontal` or `vertical` to indicate the direction of the list and along which axis to watch for overflow. Default is `horizontal`.
    + `collapsionPoint`: `String` - A keyword specifying at what point along the list container to clip overflowing elements. Valid values are:
    
        + `start` - Clip overflows at the left (for horizontal lists) or top (for vertical lists) of the list container.
        + `center-start` - Clip overflows at the center of the list container, but when visible elements are even in number, go one item less to the left (for horizontal lists) or top (for vertical lists) of the list container.
        + `center` - Clip overflows at the center of the list container. This behaves as `center-start` when visible elements are even in number.
        + `center-end` - Clip overflows at the center of the list container, but when visible elements are even in number, go one item less to the right (for horizontal lists) or bottom (for vertical lists) of the list container.
        + `end` - Clip overflows at the right (for horizontal lists) or bottom (for vertical lists) of the list container.

**Return**

* `this` - The Play UI instance.

## Usage

The document in context. The list items `<li>` in the module will be used to derive the items for the list container `<ul>`.

```html
<head>

    <template name="module1">
        <li></li> <!-- the default export -->
        <li exportgroup="export1" class="fancy-item"></li> <!-- a named export -->
    </template>

</head>
<body>

    <ul id="list" template="module1"></ul>

</body>
```

Create a list of cities.

```js
const items = [
    {name: 'London'},
    {name: 'Lagos'},
    {name: 'Berlin'},
];
$('ul').itemize(items);
```

A list item `<li>` is created for each city, and `itemize()`has bound the item data for each item element to the state of the element using the element's `.setState()` method. But binding the data alone doesn't get it rendered.

```html
<ul id="list" template="module1">
    <li data-index="0"></li>
    <li data-index="1"></li>
    <li data-index="2"></li>
</ul>
```

Now, we provide a `renderCallback` function to handle rendering of item data.


```js
$('ul').itemize(items, (directive, itemElement, itemData) => {
    if (directive === 'render') {
        itemElement.innerHTML = itemData.name;
    }
});
```

Item data are now rendered.

```html
<ul id="list" template="module1">
    <li data-index="0">London</li>
    <li data-index="1">Lagos</li>
    <li data-index="2">Berlin</li>
</ul>
```

+ Automatic data rendering can be achieved intwo ways.
+ Specifying the item export to use can be achieved intwo ways.
+ Reactive lists.
+ Overflow lists.

------

## Static Usage

The `.itemize()` instance method is internally based on the standalone `dom/itemize()` function which may be used statically.

### Import

```js
const { itemize } = $.dom;
```
```js
import { itemize } from '@webqit/play-ui/src/dom/index.js';
```

### Syntax

See [the general way to use Play UI's standalone functions](../../../quickstart#use-as-descrete-utilities)
