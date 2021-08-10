---
desc: Take a one-minute overview of the Observer API.
_index: first
---
# Overview

Webflo is the universal JavaScript framework for Web, Mobile, and API Backends, and the single tool that takes you from development, to deployment, to continous delivery. The Webflo experience is: *all of the possibilities* delivered in a *long-story-short approach*!

Take an overview!

## Starting

Every Webflo project starts on an empty directory that you can create on your machine. We are creating one below - named `webflo-app` - and navigating into it on the terminal.

```bash
mkdir webflo-app
cd webflo-app
```

Next is to have Webflo installed, following the [installation guide](../installation). This makes the `webflo` command available on the terminal. And that is all the setup required!

> Webflo does not need to be imported and instantiated. In fact, it keeps itself and its files outside of the project folders. This lets everything that is framework-related stay outside of your code. A zero-footprint philosophy gives us a clean slate to think and code.

 Everything from here will be application-specific!

## Project Layout

Any project will normally live in files and folders. Webflo lets us follow a layout that just defines the capabilities of the application.

The following directories are used as the application's URL-handling facilities.

+ **`/public`** - for static files serving
+ **`/server`** - for server-side routing
+ **`/client`** - for client-side routing
+ **`/worker`** - for worker-level routing

Any other directory (e.g. `/src`) may exist here at root level for other purposes.

## Routing

> **Main Topic:** [Routing](../../fundamentals/routing)

