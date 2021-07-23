---
desc: The <i>HTML Modules</i> specification.
_index: first
---
# HTML Modules

*HTML Modules* are a templating feature that lets us write reusable HTML markup using the *module*, *export* and  *import* paradigm. This feature establishes the standard `<template>` element as the foundation of a module infrastructure for HTML and introduces new attributes, properties and events that together closes the loop.

> OOHTML is [being proposed as a native browser technology](https://discourse.wicg.io/t/proposal-chtml/4716) while currently available through a polyfill. Be sure to check the [Polyfill Support](#polyfill-support) section below for the features on this page.

## Convention

An HTML module is a standard `<template>` element with a `name` attribute - which serves as the *module identifier*.

```html
<head>

    <template name="module1">

        <label for="age">How old are you?</div>
        <input id="age" />

    </template>

</head>
```

The contents of a module are simply the *module exports*.

Now, exports may be more properly wrapped within an `<export>` element of a designated name - the *export identifier*.

```html
<head>

    <template name="module1">

        <export name="question">
            <label for="age">How old are you?</label>
            <input id="age" />
        </export>

        <div>This is another export</div>

    </template>

</head>
```

Or they may be individually *tagged* to an export identifier using the `exportgroup` attribute.

```html
<head>

    <template name="module1">

        <label exportgroup="question" for="age">How old are you?</label>
        <input exportgroup="question" name="age" />
        
        <div>This is another export</div>

    </template>

</head>
```

Contents not associated with an export identifier are regarded as *default exports*. They implicitly have 'default' as their export identifier.

### Module Nesting

For organizational purposes, modules may be nested.

```html
<head>

    <template name="module1">
        
        <div>This is snippet 1</div>
        <div>This is snippet 2</div>

        <template name="module-nested">
            <div>This is snippet 3</div>
            <div>This is snippet 4</div>
        </template>

    </template>

</head>
```

The full *module ID* of a nested module would be a path expression, e.g `module1/module-nested`.

Unnested modules are called *top-level modules*.

### Module Referencing

Modules can be referenced by their module ID from anywhere in a page using the `template` attribute. Think Custom Elements that often depend on templates for their Shadow DOM's source markup. These dependencies would be simply declared using the `template` attribute.

```html
<body>

    <my-prompt template="module1"></my-prompt>

</body>
```

Or in the case of a nested module.

```html
<body>

    <my-prompt template="module1/module-nested"></my-prompt>

</body>
```

The internal JavaScript code for `<my-prompt>` would simply get a copy of the referenced module delivered via the [modules API](#api).

```js
const myPrompt = document.querySelector('my-prompt');
console.log(myPrompt.template); // HTMLTemplateElement {}
```

Now, a few rules apply to module IDs:

+ A module ID must not contain any special characters (e.g `~`, `#`, `/`, `&`, `!`, `^`, `%`, `+`, `.`, etc) except the following: `@`, `-`, `_`.

## Remote Content

Template elements may reference remote content using the `src` attribute.

```html
<head>
    <template name="module1">

        <div exportgroup="export5"></div>
        <div exportgroup="export6"></div>

        <template name="module-remote" src="/bundle.html"></template>

    </template>
</head>
```

**Remote file: /bundle.html**

```html

<div exportgroup="export1"></div>
<div exportgroup="export2"></div>
<template name="module-loaded">
    <div exportgroup="export3"></div>
    <div exportgroup="export4"></div>
</template>

```

Remote contents automatically become the template's content on load.

> The `src` attribute isn't honoured when a template already has content.

## API

*HTML Modules* offers a set of APIs that lets us access modules, imports and exports as JavaScript objects and properties. One advantage this gives us is that it minimizes selector-based queries.

+ **document.templates: Object** - This is a *readonly* property on the `document` object that gives the document's top-level templates as an object.

    ```js
    let module1 = document.templates.module1; // Returns the "module1" element in the markup somewhere above
    ```

+ **HTMLTemplateElement.prototype.templates: Object** - This is a *readonly* property on the `<template>` element that gives the template's own nested templates as an object.

    ```js
    let module1 = document.templates.module1;
    let nestedModule = module1.templates['module-nested']; // Returns the nested "module-nested" element in the markup somewhere above
    ```

+ **HTMLTemplateElement.prototype.exports: Object** - This is a *readonly* property on the `<template>` element that gives the template's *exports* as an object. Each export is given as an *array* of elements.

    ```js
    let module1 = document.templates.module1;

    // Named exports
    let questionExport = module1.exports.question; // Returns the "label" and "input" elements in the markup somewhere above
    console.log(questionExport.length); // 2

    let defaultExport = module1.exports.default; // Returns the "default" export in the markup somewhere above
    console.log(defaultExport.length); // 1
    ```

+ **Element.prototype.template: HTMLTemplateElement** - This is a *readonly* property of any element that returns the element's referenced module - the `<template>` element that is referenced in its `template` attribute.

    ```js
    let templateDependency = myPrompt.template;
    ```

    Here's how this could be used in the internal JavaScript code of the `<my-prompt>` custom element.

    ```js
    customElements.define('my-prompt', class extends HTMLElement {
        constructor() {
            super();
            // Get the referenced template element
            let moduleReference = this.template;
            // Clone its "question" export
            let shadowContent = moduleReference.exports.question.map(el => el.cloneNode(true));
            // Create Shadow DOM and send in the content
            let shadow =  this.attachShadow({mode: 'open'});
            shadow.append(...shadowContent);
        }
    });
    ```

    Here's how the HTML consuming the component could look.

    ```html
    <body>
        <!-- Flavour 1 of <my-prompt> -->
        <my-prompt template="module1"></my-prompt>
        <!-- Flavour 2 of <my-prompt> -->
        <my-prompt template="module2"></my-prompt>
    </body>
    ```

## Module Events

The following events are fired on `<template>` elements that load remote content.

+ **load: Event** - This event is fired on the `<template>` element on loading its remote content.
+ **loaderror: Event** - This event is fired on the `<template>` element when there is an error loading its remote content.

The following events are fired on the document object when the document's modules or their composition change.

+ **templatemutation: Event** - This event is fired on the `document` object when templates are added to or removed from the document, or when exports are added to or removed from a module. The *event* object has a `.detail` property that gives the details of the event.

    + **event.detail.path: String** - This gives the path to the event source, the module under which the event is fired. This is empty when top-level modules are added to or removed from the document.
    + **event.detail.addedExports: Array** - This gives the list of exports added to a module. Each *entry* is an object describing the added export.
        + **entry.name: String** - The name of the export.
        + **entry.items: Array** - Elements in the export.
    + **event.detail.removedExports: Array** - This gives the list of exports removed from a module. Each *entry* is an object describing the removed export.
        + **entry.name: String** - The name of the export.
        + **entry.items: Array** - Elements in the export.
    + **event.detail.addedTemplates: Array** - This gives the list of templates added to a module or the top-level scope. Each *entry* is an object describing the added template.
        + **entry.name: String** - The name of the template.
        + **entry.item: HTMLTemplateElement** - The template element.
    + **event.detail.removedTemplates: Array** - This gives the list of templates removed from a module or the top-level scope. Each *entry* is an object describing the removed template.
        + **entry.name: String** - The name of the template.
        + **entry.item: HTMLTemplateElement** - The template element.

    With the listener below, adding a new template to the document, or removing one, will be reported in the console.

    ```js
    document.addEventListener('templatemutation', event => {
        console.log(event.detail);
    });
    ```

    With the code below, the fired event's `.detail.path` property will be empty, while its `.detail.addedTemplates` property will give a list of one added template whose *name* is `module2`.

    ```js
    let template = document.createElement('template');
    template.setAttribute('name', 'module2');
    document.body.append(template);
    ```

    With the code below, when the nested module is done loading its contents, its exports are given in a `templatemutation` event on its `.detail.addedExports` property. If loaded contents include template elements themselves, they will be given in the event's `.detail.addedTemplates` property. The event's `.detail.path` property itself will be `module2/module-remote`.
        
    ```html
    <head>
        <template name="module2">

            <div exportgroup="export5"></div>
            <div exportgroup="export6"></div>

            <template name="module-remote" src="/bundle.html"></template>

        </template>
    </head>
    ```

+ **templatecontentloaded: Event** - This event is fired on the `document` object when a template completes loading its remote content. The event object has a `.detail` property that gives the template element and its path.

    With the code below, when the nested template is done loading its contents, a report is logged to the console with path being `module2/module-remote`.

    ```js
    document.addEventListener('templatecontentloaded', event => {
        console.log(event.detail.path, event.detail.template);
    });
    ```
        
    ```html
    <head>
        <template name="module2">

            <div exportgroup="export5"></div>
            <div exportgroup="export6"></div>

            <template name="module-remote" src="/bundle.html"></template>

        </template>
    </head>
    ```

+ **templatecontentloaderror: Event** - This event is fired on the `document` object when a template fails loading its remote content. The event object has a `.detail` property that gives the template element and its path.

## Polyfill Support

The current [OOHTML polyfill implementation](../../getting-started/polyfill) has full support for the HTML Modules specification. The polyfill additionally makes it possible to customise the following areas of its implementation of the syntax using the [OOHTML META tag](../../resources/meta-tag):

+ **[attr.moduleid](#convention)** - The *module ID* attribute. The standard attribute is `name`, but you may use a custom attribute name, where necessary.
        
    ```html
    <head>
        <meta name="oohtml" content="attr.moduleid=data-name;" />
        <template data-name="module2">
            <div exportgroup="export-1"></div>
            <div exportgroup="export-2"></div>
        </template>
    </head>
    ```

+ **[element.export](#convention)** - The tag name for the `<export>` element. The standard `<export>` element is `<export>`. This can be changed where necessary.
        
    ```html
    <head>
        <meta name="oohtml" content="element.export=html-export;" />
        <template name="module2">
            <html-export name="export-1">
                <div></div>
            </html-export>
        </template>
    </head>
    ```

+ **[attr.exportid](#convention)** - The *export ID* attribute. The standard attribute is `name`, but you may use a custom attribute name, where necessary.
        
    ```html
    <head>
        <meta name="oohtml" content="attr.exportid=data-name;" />
        <template name="module2">
            <export data-name="export-1">
                <div></div>
            </export>
            <export data-name="export-2">
                <div></div>
            </export>
        </template>
    </head>
    ```

+ **[attr.exportgroup](#convention)** - The *exportgroup* attribute. The standard attribute is `exportgroup`, but you may use a custom attribute name, where necessary.
        
    ```html
    <head>
        <meta name="oohtml" content="attr.exportgroup=data-exportgroup;" />
        <template name="module2">
            <div data-exportgroup="export-1"></div>
            <div data-exportgroup="export-2"></div>
        </template>
    </head>
    ```

+ **[attr.moduleref](#convention)** - The *module reference* attribute. The standard attribute is `template`, but you may use a custom attribute name, where necessary.
        
    ```html
    <head>
        <meta name="oohtml" content="attr.moduleref=data-template;" />
        <div data-template="module2">
            <import name="export-1"></import>
        </div>
    </head>
    ```

+ **[api.templates](#api)** - The *templates* property exposed on the document object and on HTML template elements. The standard property is `templates`, but you may use a custom property name, where necessary.
        
    ```html
    <head>
        <meta name="oohtml" content="api.templates=templatelist;" />
    </head>
    ```
    
    ```js
    let module1 = document.templatelist.module1;
    ```

+ **[api.exports](#api)** - The *exports* property exposed on HTML template elements. The standard property is `templates`, but you may use a custom property name, where necessary.
        
    ```html
    <head>
        <meta name="oohtml" content="api.exports=exportlist;" />
    </head>
    ```
    
    ```js
    let export1 = module1.exportlist.export1;
    ```

+ **[api.moduleref](#api)** - The *module reference* property exposed on HTML elements. The standard property is `template`, but you may use a custom property name, where necessary.
        
    ```html
    <head>
        <meta name="oohtml" content="api.moduleref=tpl;" />
    </head>
    ```
    
    ```js
    let templateDependency = myPrompt.tpl;
    ```

Learn more about customization and the OOHTML meta tag [here](../../resources/meta-tag).