---
title: event.Response
desc: <code>class event.Response {}</code>
_index: 3
---
# `class event.Response {}`

This class is used for returning responses from route handlers. It is available to route handlers via the `event.Response` parameter.

Returning responses with this class allows a handler to include an HTTP status code and some HTTP headers.

```js
export default async function(event, app, next) {
    if (next.stepname) {
        return next();
    }
    return new event.Response({
        status: 200,
        headers: { 'Content-Type': 'application/json', },
        body: { prop1: value1, }
    });
}
```

But in many cases, creating an `event.Response` instance will not be necessary as Webflo will dynamically figure out an appropriate HTTP status code along with a `Content-Type` header for the response as detailed in: [API Calls and Page Requests](../../fundamentals/requests-and-responses#api-calls-and-page-requests). Here, returning the plain data object will just suffice.

```js
export default async function(event, app, next) {
    if (next.stepname) {
        return next();
    }
    return { prop1: value1, };
}
```

> With the first handler above returning a `Content-Type` header, no automatic content negotiation will be made by Webflo.



## Properties

+ **`query: Object`** - An object model of the URL query parameters. This maintains a live, two-way relationship between itself and the `.search` string property of its base URL object. Changes made on one end are automatically reflected on the other.

## Header Shortcuts

### Content-Type

### Redirect

### Cache-Control

### Cookies

### Cross-Origin Requests (CORs)
