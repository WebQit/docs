---
title: NavigationEvent
desc: <code>class NavigationEvent {}</code>
_index: first
---
# `class NavigationEvent {}`

This is the `NavigationEvent` that is fired on every request which route handlers receive on their `event` parameter.

## Properties

+ **`url: URLX`** - An object representing the request URL. *(See class [`URLX`](../URLX).)*
+ **`request: IncomingRequest`** - The incoming request object. *(See interface [`IncomingRequest`](../IncomingRequest).)* On the client side, this would be an extended instance of the standard [`Request`](https://developer.mozilla.org/en-US/docs/Web/API/Request) interface. On the server side, this would be an extended instance of node.js's [`http.IncomingMessage`](https://nodejs.org/api/http.html#http_class_http_incomingmessage) class.
+ **`Response: Class`** - A *class* for instantiating a new reponse. *(See class [`event.Response`](../event_Response).)*