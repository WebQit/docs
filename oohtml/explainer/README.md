---
_after: polyfill
---
# OOHTML Features Explainer

This information is being gathered to present OOHTML's design and architectural choices in the light of the problem space. We intend to maintain this as a living document; watch this space, or help us improve it by submitting a pull request or by [filing an issue](https://github.com/webqit/oohtml/issues).

## Object-Oriented Markup

*Naming and finding things* is hard; modular design makes it easier! This isn't necessarily some modern wisdom, we just couldn't before now address HTML's old idea of one global scope for IDs and CSS selectors. Here's how we currently struggle with creating modular markup and how OOHTML makes it a lot easier.

+ **[BEM](https://getbem.com)** has been an agreeable workaround for many people. It's, however, clunky.
    + Compare that with the more succinct **[Namespaced Selectors](../namespaced-html#namespaced-selectors)** in [Namespaced HTML](../namespaced-html).

    *BEM also doesn't really go beyond usage in CSS. Namespaced HTML, on the other hand, is able to translate Namespaces in markup into namespace objects in JavaScript by means of the Namespace API.*

+ **[Stuart P.'s Parts and Walls](https://github.com/stuartpb/pwalls-spec)** proposal from 2015 also points to the same need for a modular naming that's accessible in JavaScript.
    + But while the above is base on frequent DOM traversal, Namespaces HTML let's you access the same elements as properties of a live object. In other words, instead of having to query the DOM each time to access named elements, the DOM exposes them as properties and let's us receive updates when any of the properties change. See details in the [Namespace API](../namespaced-html#api) of [Namespaced HTML](../namespaced-html).*

    *And just for fun, it also looks like the DOM itself likes the idea of accessing strategic UI objects in JavaScript using a simple `object.property` convention. See [`document.head`](https://developer.mozilla.org/en-US/docs/Web/API/Document/head) and [`document.body`](https://developer.mozilla.org/en-US/docs/Web/API/Document/body). These are indeed more concise than `document.querySelector('head')` and `document.querySelector('body')`.*

Overall, [Namespaced HTML](../namespaced-html) is the far-reaching solution to authoring modular HTML and CSS.

## HTML Modules and Imports

On the expansive subject of [*HTML Modules*](https://github.com/WICG/webcomponents/issues/645), OOHTML takes a very, very different approach from the many existing opinions to how reusable markup should be delivered and consumed in the browser - *[OOHTML's HTML Modules](../html-modules)*. What is the most important difference?

+ OOHTML sees HTML Modules as simply a way to ship and consume **static, inert content**, and is thus more oriented towards HTML's standard primitive for *static, inert content* - the `<template>` element. OOHTML simply introduces [the **src** attribute](../html-modules#remote-content) as a way to *load a template's content from a remote HTML file*.
+ [This explainer](https://github.com/WICG/webcomponents/blob/gh-pages/proposals/html-modules-explainer.md), among others, however, follows the JavaScript route of using ES6 module infrastructure to load **both static content and active script elements**.

How do these compare?

+ **OOHTML's approach let's you _author and deliver HTML in HTML_.** And it's as declarative as this:
    
    *Remote File: /bundle.html*

    ```html
    <div exportgroup="blogPost">
        <p>Content...</p>
    </div>
    ```

    *Main Document:*

    ```html
    <template name="bundle" src="/bundle.html"></template>
    ```

    *See also: [[proposal - **src** attribute]](https://discourse.wicg.io/t/add-src-attribute-to-template/2721), [[proposal - **src** attribute]](https://github.com/whatwg/html/issues/2791)*

    **But, the other approach gets you to deal with JavaScript code to import remote HTML**

    *Remote File: /import.html*

    ```html
    <div id="blogPost">
        <p>Content...</p>
    </div>
    <script type="module">
        let blogPost = import.meta.document.querySelector("#blogPost");
        export { blogPost }
    </script>
    ```
    
    *Main Document:*`

    ```js
    <script type="module">
        import { blogPost } from "import.html";
    </script>
    ```

+ **OOHTML's approach provides for consuming the imported HTML _both in JavaScript and in HTML_.** And it's as declarative as the following two cases:
    
    *In JavaScript - as detailed in [HTML Modules API](../html-modules#api)*

    ```html
    <script>
        let [ blogPost ] = document.templates.bundle.exports.blogPost;
        document.body.appendChild(blogPost.cloneNode(true));
    </script>
    ```

    *See also: [[proposal - `document.templates`]](https://discourse.wicg.io/t/document-templates/1057)*
    
    *In HTML - as detailed in [OOHTML Imports](../html-imports)*

    ```html
    <body>
        <import name="blogPost" template="bundle"></import>
    </body>
    ```   

    *See also: [[HTML Modules - direction]](https://github.com/WICG/webcomponents/issues/863)*

    **But, the other approach remains based _only in JavaScript_**

    ```html
    <body>
        <script type="module">
            import { blogPost } from "import.html"
            document.body.appendChild(blogPost);
        </script>
    </body>
    ```

+ **OOHTML's approach makes for _lazy-loading plus load events_, as with elements like `<img>`...**

    *In JavaScript - see [Module events](../html-modules#module-events)*
    
    *In HTML - as detailed in [OOHTML Imports](../html-imports)*

    ```html
    <body>
        <!-- Resolves whenever module loads -->
        <import name="blogPost" template="bundle"></import>
    </body>
    ```  

    **But, the other approach simply does not.**

Overall, it seems more traditional to us to implement HTML Modules in HTML than in JavaScript, letting us do all HTML concerns in HTML (templates) and all JavaScript concerns in JavaScript (ES6 modules).

But then, loading **static, inert content**? What about cases where some reusable HTML has to go with some logic? [Subscript](../subscript) could be a good answer! We've had success with the following:

*File: /bundle.html*

```html
<div exportgroup="blogPost">
    <p id="content">Content...</p>
    <script type="subscript">
        this.querySelector('#content').innerHTML = document.state.content;
    </script>
    <!-- previous syntax: <script type="scoped"></script> -->
</div>
```

*Document*

```html
<body>
    <template name="bundle" src="/bundle.html"></template>
    <!-- Resolves whenever module loads, and the slotted element's scoped script activates -->
    <import name="blogPost" template="bundle"></import>
</body>
```

And how far can this go? We run this in production in <webqit.io>.

## State, Observability and Reactivity

Primitives for building *reactive* applications and keeping track of very dynamic state have not particularly found their way to native web languages.

+ **State:** We indeed have some primitives related to the concept of *state*, but they have very limited usecases. An example is [HTML5's DataSet API](https://developer.mozilla.org/en-US/docs/Web/API/HTMLOrForeignElement/dataset) that lets us keep state with HTML elements. But, it turns out to be tied to just HTML data `(data-*)` attributes - making it less useful as a general way to work with application state in the UI.

    Compare OOHTML's [State API](../the-state-api) which gives us robust *state management* - covering both document-level and element-level state, and the concept of automatic observability.

+ **Observability:** Much engineering still goes into using JavaScript's change-detection mechanisms for *reactive* UI development, and some things don't even scale. Check out a consideration of some of those difficulties in [this explainer](https://webqit.io/tooling/observer/explainer).

    Compare the [Observer API](https://github.com/webqit/observer) which provides generic APIs for observing and intercepting JavaScript objects. See its *universal* role across the rest of OOHTML, and potentially other technologies.

+ **Reactivity:** A Reactuve UI binding language? See [Subscript](../subscript). It comes bringing the full power of JavaScript for the job.

    Compare [this early idea for a template syntax by Jonathan Kingston](https://discourse.wicg.io/t/extension-of-template/447) from 2014, [this proposal](https://github.com/whatwg/html/issues/2254) from 2017, and [Apple's proposal](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Template-Instantiation.md) from 2017.
