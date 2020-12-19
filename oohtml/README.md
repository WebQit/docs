# OOHTML

<!-- BADGES/ -->

<span class="badge-npmversion"><a href="https://npmjs.org/package/@webqit/oohtml" title="View this project on NPM"><img src="https://img.shields.io/npm/v/@webqit/oohtml.svg" alt="NPM version" /></a></span>
<span class="badge-npmdownloads"><a href="https://npmjs.org/package/@webqit/oohtml" title="View this project on NPM"><img src="https://img.shields.io/npm/dm/@webqit/oohtml.svg" alt="NPM downloads" /></a></span>

<!-- /BADGES -->

[Object-Oriented HTML (OOHTML)](https://webqit.io/tooling/oohtml) is a suite of new DOM features that specifically facilitates writing modular HTML, CSS, and JavaScript *natively* and *more conveniently*. It addresses a number of limitions inherent with existing conventions and welcomes much of the paradigms associated with modern UI development.

> OOHTML is being proposed as a [W3C standard at the Web Platform Incubator Community Group](https://discourse.wicg.io/t/proposal-chtml/4716). Consider bringing your ideas to the discussion.

> [Visit this project on GitHub](https://github.com/webqit/oohtml).

## New Foundational Features
OOHTML brings certain new features to native web languages to make common UI design terminologies possible natively.

+ **Modular Naming and APIs** - *Naming and finding things is hard; modular design makes it easier!* But while the modular design thinking requires that we keep design decisions to small-sized scopes, HTML's naming system has only the idea of one global scope. We practically have to work agaisnt the globally-scoped nature of CSS selectors and IDs to try to have a modular markup and we end up in terrible *hacks* and much naming wars, or at best, clunky naming conventions like [BEM](https://getbem.com). OOHTML addresses this design and architectural difficulty using the [Namespaced HTML Specification](namespaced-html/README.md) and the [Named Templates Specification](named-templates/README.md).
+ **State and Observability** - The concept of building stateful applications and keeping track of all the moving parts have not particularly found a place among native web languages. Much engineering still goes into using JavaScript's change-detection mechanisms for "reactive" UI development, and everything quickly becomes too complex to reason about. OOHTML meets this challenge, first at the language level, with the simple [Observer API](the-observer-api/README.md) that lets us observe JavaScript objects and arrays with total transparency; then, at the DOM level, with [The State API](the-state-api/README.md) that implements the Observer API.

## New Syntactic Sugar
OOHTML provides features that offer *syntactic sugar* over its foundational features and existing DOM APIs.

+ **Scoped Scripts** - Scoped Scripts is a new feature that lets us write `<script>` elements that are scoped to their immediate host elements instead of the global browser scope. Scoped Scripts makes it easier to apply behaviour to modular markup as they execute in the context of their host elements. But they are especially powerful in being able to automatically keep the UI in sync with tha state of an application as they internally implement the [Observer API](the-observer-api/README.md). [Check out the details](scoped-scripts/README.md) to learn more.
+ **HTML Partials** - HTML Partials is a new templating feature that abstracts over the [Named Templates Specification](named-templates/README.md) to provide incredibly powerful composability in the simple language of tags and attributes. [Check out the details](html-partials/README.md) to learn more.

## Getting Started
To add the current OOHTML polyfill to your page, follow [the installation guide](installation/README.md).

And what does OOHTML code look like? The following examples give us a glimpse of what's possible. Links from these examples contain much further details.

### Namespaced HTML
The following modular markup implements its IDs in namespaces:

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

The above gives us a conceptual hierarchy of objects:

```html
continents
|- europe
|   |- about
|   |- countries
|- asia
    |- about
    |- countries
```

The *namespace API* translates that model into a real object tree:

```js
// Get the "continents" article
let continents = document.namespace.continents;

// Access scoped IDs with the new "namespace" DOM property
let europe = continents.namespace.europe;
let asia = continents.namespace.asia;

// And for deeply-nested IDs...
let aboutAsia = continents.namespace.asia.namespace.about;
```

We get a document structure that's easier to reason about and to work with.

*Details are in the [Namespaced HTML](namespaced-html/README.md) section.*

### State and Observability
The [Observer API](the-observer-api/README.md) lets us observe objects in real time:

```js
Observer.observe(continents.namespace, events => {
    console.log(events.map(event => event.type + ': ' + event.name));
});
```

With the code above, adding a new ID - `africa` - to the `continents` namespace would be reported in the console.

```js
continents.append('<section id="africa"></section>');
```

The document object and every DOM element also feature [The State API](the-state-api/README.md) that lets us maintian application state at the document and element levels:

```js
Observer.observe(continents.state, events => {
    // Set application data to element attributes
    // and keep them in sync
    events.forEach(event => {
        continents.setAttribute(event.name, event.value);
    });
});
```

With the code above, setting properties on `continents.state` would update the element.

```js
continents.state.title = 'List of continents';
// Or
continents.bind({
    title: 'List of continents',
});
```

We could easily update continents in the `#continents` tree this way:

```js
// Bind application data to section elements
Observer.observe(continents.state, events => {
    events.forEach(event => {
        let sectionElement = continents.namespace[event.name];
        sectionElement.namespace.about.innerHTML = event.value.about;
        sectionElement.namespace.countries.innerHTML = event.value.countries;
    });
});
// Update the "Asia" section
continents.state.asia = {
    about: 'About Asia (NEW)',
    countries: 'Countries in Asia (NEW)',
};
```

And we could easily add new continents to the `#continents` tree this way:

```js
// Bind application data to section elements
Observer.observe(continents.state, events => {
    events.forEach(event => {
        let sectionElement = continents.namespace[event.name];
        if (!sectionElement) {
            let sectionElement = document.createElement('section');
            sectionElement.setAttribute('id', event.name);
            // ------------
            let aboutElement = document.createElement('div');
            aboutElement.setAttribute('id', 'about');
            let countriesElement = document.createElement('div');
            countriesElement.setAttribute('id', 'countries');
            // ------------
            sectionElement.append(aboutElement, countriesElement);
            continents.append(sectionElement);
        }
        sectionElement.namespace.about.innerHTML = event.value.about;
        sectionElement.namespace.countries.innerHTML = event.value.countries;
    });
});
// Add an "Africa" section
continents.state.africa = {
    about: 'About Africa',
    countries: 'Countries in Africa',
};
```

But just so we keep markup out of application code, we could employ a *named* `<template>` element in our code above:

```html
<head>
    <template name="template1">

        <section export="continent" id="" namespace>
            <div id="about"></div>
            <div id="countries"></div>
        </section>

    </template>
</head>
```

```js
// Bind application data to section elements
Observer.observe(continents.state, events => {
    events.forEach(event => {
        let sectionElement = continents.namespace[event.name];
        if (!sectionElement) {
            let template1 = document.templates.template1;
            sectionElement = template1.exports.continent[0].cloneNode(true);
            sectionElement.setAttribute('id', event.name);
            continents.append(sectionElement);
        }
        sectionElement.namespace.about.innerHTML = event.value.about;
        sectionElement.namespace.countries.innerHTML = event.value.countries;
    });
});
```

Now, we could build web components more efficiently this way.

*Details are in the [Named Templates](named-templates/README.md) section.*

### Scoped Scripts
The following `<script>` element is scoped to the `#alert` element - its host element:

```html
<div id="alert">

    <div class="message"></div>
    <div class="exit" title="Close this message.">X</div>

    <script scoped>
        this.querySelector('.exit').addEventListener('click', () => {
            this.remove();
        });
    </script>

</div>
```

Properties in an element's *state* object can be automatically bound to its scoped script. We would just qualify the scoped script with `binding` keyword:

```html
<body>

    <div id="alert">

        <div class="message"></div>
        <div class="exit" title="Close this message.">X</div>

        <script scoped binding>
            // where to place the message within the alert block...
            this.querySelector('.message').innerHTML = this.state.message;
            // details of how the alert block should behave...
            this.querySelector('.exit').addEventListener('click', () => {
                this.remove();
            });
        </script>

    </div>

    <script>
        let alertElement = document.querySelector('#alert');
        alertElement.state.message: 'Task started!';
        setTimeout(() => {
            alertElement.state.message: 'Task complete!';
        }, 1000);
    </script>

</body>
```

Now, we could easily create more complex stuff this way. [Think a clock, a dynamic list](examples/README.md).

*Details are in the [Scoped Scripts](scoped-scripts/README.md) section.*

### HTML Partials
The following `<template>` elements contain reusable snippets called exports:

```html
<head>

    <template name="template1">
        <div export="export-1">This is export1 in template1</div>
        <div export="export-2">This is export2 in template1</div>
    </template>
    <template name="template2">
        <div export="export-3">This is export3 in template2</div>
        <div export="export-4">This is export4 in template2</div>
    </template>

</head>
```

An element in the `<body>` area can point to the `<template>` element and implement its exports:

```html
<body>

    <!-- Point to the template element -->
    <div template="template1">
        <h2>I have imports</h2>
        <!-- Place export-1 here -->
        <import name="export-1"></import>

        <div>
            <!-- Place export-2 here -->
            <import name="export-2"></import>
        </div>
    </div>

</body>
```

And `<import>` elements themselves can also point to a `<template>` element directly:

```html
<body>

    <!-- Point to the template element -->
    <div template="template1">
        <h2>I have imports</h2>
        <!-- Place export-1 here -->
        <import name="export-1"></import>

        <div>
            <!-- Point to template2 and place export-3 here -->
            <import name="export-3" template="template2"></import>
        </div>
    </div>

</body>
```

An element can be dynamically pointed to another `<template>`, and its *imports* will be automatically resolved from the new `<template>`:

```js
document.querySelector('div[template="template1"]').setAttribute('template', 'template2');
```

Now, we could create more dynamic stuff this way. [Think a Single Page Application](examples/README.md) (SPA).

*Details are in the [HTML Partials](html-partials/README.md) section.*

## FAQs
We are working on publishing some questions we've been asked, but you can always file an [issue](https://github.com/webqit/oohtml/issues) to ask a question or raise a suggestion.

## Relationship With Other Technologies
You may also visit the [comparison section](comparison/README.md) for a view of how OOHTML compares with existing technologies and standardization effort.

## Issues
To report bugs or request features, please submit an [issue](https://github.com/webqit/oohtml/issues).

## License
MIT.
