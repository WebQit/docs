# Namespaced HTML
Namespacing is a DOM feature that let's an element establish its own naming context for descendant elements. It makes it possible to keep IDs scoped to a context other than the document's global scope.

Namespaced HTML is a document that is structured as a hierarchy of *scopes* and *subscopes*.

> OOHTML is [being proposed as a native browser technology](https://discourse.wicg.io/t/proposal-chtml/4716) but currently available through a polyfill. Be sure to check the [Polyfill Support](#polyfill-support) section below for the features on this page.

## Convention
Namespaces are designated with the `namespace` *Boolean* attribute.

In the code below, the given ID is scoped to the element with the `namespace` attribute.

```html
<div namespace>
    <div>
        <div id="some-id"></div>
    </div>
</div>
```

At scale, what we get is a **hierarchy of *scopes* and *subscopes***.

```html
<article id="continents" namespace>
    <section id="europe" namespace>
        <div id="about">About Europe</b></div>
        <div id="countries">Countries in Europe</div>
    </section>
    <section id="asia" namespace>
        <div id="about">About Asia</b></div>
        <div id="countries">Countries in Asia</div>
    </section>
</article>
```

A conceptual model of the hierarchy would be:

```html
continents
|- europe
|   |- about
|   |- countries
|- asia
    |- about
    |- countries
```

## Namespaced Selectors
Being able to layout elements in namespaces makes it possible to write collision-free CSS selectors. OOHTML introduces the concept of *Namespaced Selectors* which are regular CSS expressions written with a path notation.

Namespaced Selectors use the forward slash `/` to denote a namespace boundary.

```html
<style>
#continents / #europe / #countries {
    color: darkblue;
}
#continents / #asia / div {
    color: orange;
}
</style>
```

Namespacing would follow the same convention on existing DOM Selector APIs.

```js
let aboutAsia = document.querySelector('#continents / #asia / #about');
let divsAsia = document.querySelectorAll('#continents / #asia / div');
```

## Namespace APIs
*Namespaced HTML* offers a set of APIs for traversing namespaces as objects and properties.

+ **document.namespace: Object** - This *readonly* property is a reflection of the state of the document's namespaced IDs - IDs scoped to the document.

    ```js
    let continents = document.namespace.continents; // Returns the "#continents" element in the markup above
    ```

+ **Element.prototype.namespace: Object** - This *readonly* property is a reflection of the state of an element's namespaced IDs.

    ```js
    // Get the "continents" article
    let continents = document.namespace.continents;

    // Access scoped IDs with the new "namespace" DOM property
    let europe = continents.namespace.europe;
    let asia = continents.namespace.asia;

    // And for deeply-nested IDs...
    let aboutAsia = continents.namespace.asia.namespace.about;
    ```

    > An object tree helps to facilitate Object-Orientend Development and minimize selector-based queries. It also turns out to offer better DOM performance than selector-based DOM traversal as we are practically not querying the DOM to reach elements.

## Namespace Observability
With observability at OOHTML's core, the `document.namespace` property and the `Element.prototype.namespace` property are implemented as *live objects* that can be observed for realtime changes in the namespace tree. Live objects are observed using the [Observer API](../the-observer-api/README.md).

```js
// Obtain the Observer API and use the Observer.observe() method
Observer.observe(continents.namespace, events => {
    console.log(events.map(event => event.type + ': ' + event.name));
});
```

We could as well specify just the name to observe on the function's second parameter.

```js
Observer.observe(continents.namespace, 'africa', event => {
    // We're now also logging the event's value, that is, the element
    console.log(event.type, event.value);
});
```

With the code above, adding a new ID - `africa` - to the `continents` namespace would be reported in the console.

```js
let section = document.createElement('section');
section.setAttribute('id', 'africa');
continents.append(section);
```

Removing this element would trigger our observer in the same way.

```js
continents.namespace.africa.remove();
```

To observe changes down the namespace hierarchy, we would set the observer's `params.subtree` to `true`.

```js
Observer.observe(continents.namespace, events => {
    // We're now also logging the event's path property
    console.log(events.map(event => event.type + ': ' + event.path));
}, {subtree: true});
```

We could as well specify just the path to observe.

```js
Observer.observe(continents.namespace, ['africa', 'namespace', 'countries'], event => {
    // We're now also logging the event's value property
    console.log(event.type, event.path, event.value));
});
```

## Polyfill Support
The current [OOHTML polyfill implementation](../installation/README.md) has good support for the Namespaced HTML Specification. With the exception of [Namespaced Selectors](#namespaced-selectors), all aspects of the specification are supported. The polyfill additionally makes it possible to customise much of its implementation of the syntax using the [OOHTML meta tag](../the-oohtml-meta-tag/README.md). The following are areas of customization:

+ **[attr.namespace](#convention)** - The *namespace keyword* attribute. The standard *namespace keyword* attribute is `namespace`, but you may use a custom attribute name, where necessary.
        
    ```html
    <head>
        <meta name="oohtml" content="attr.namespace=data-namespace;" />
        <div data-namespace>
            <div id="id01"></div>
            <div id="id02"></div>
        </div>
    </head>
    ```

+ **[attr.id](#convention)** - The *namespaced-ID* attribute. The standard *namespaced-ID* attribute is `id`, but you may use a custom attribute name, where necessary.
        
    ```html
    <head>
        <meta name="oohtml" content="attr.id=data-id;" />
        <div namespace>
            <div data-id="id01"></div>
            <div data-id="id02"></div>
        </div>
    </head>
    ```

+ **[api.namespace](#namespace-apis)** - The *namespace* property exposed on the document object and on elements. The standard *namespace* property is `namespace`, but you may use a custom property name, where necessary.
        
    ```html
    <head>
        <meta name="oohtml" content="api.namespace=ns;" />
    </head>
    ```
    
    ```js
    // Get the "continents" article
    let continents = document.ns.continents;
    ```

Learn more about customization and the OOHTML meta tag [here](../the-oohtml-meta-tag/README.md).