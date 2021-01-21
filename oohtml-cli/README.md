---
icon: terminal
desc: OOHTML build tools.
categories: Build Tools
tags: OOHTML, CLI
_after: oohtml
---
# OOHTML CLI

<!-- BADGES/ -->

<span class="badge-npmversion"><a href="https://npmjs.org/package/@webqit/oohtml-cli" title="View this project on NPM"><img src="https://img.shields.io/npm/v/@webqit/oohtml-cli.svg" alt="NPM version" /></a></span>
<span class="badge-npmdownloads"><a href="https://npmjs.org/package/@webqit/oohtml-cli" title="View this project on NPM"><img src="https://img.shields.io/npm/dm/@webqit/oohtml-cli.svg" alt="NPM downloads" /></a></span>

<!-- /BADGES -->


*[OOHTML CLI](https://webqit.io/tooling/oohtml-cli)* is a Command-Line utility for OOHTML development.

> [Visit this project on GitHub](https://github.com/webqit/oohtml-cli).

## Installation
Install OOHTML-CLI globally via npm

```text
$ npm i -g npm
$ npm i -g @webqit/oohtml-cli
```

## Commands
View all available commands and their descriptions:

```text
$ oohtml help
```

+ **Config** - Configure the behaviour of a command:

    ```text
    $ oohtml config <command>
    ```

    ...where `<command>` is the command name to configure. Use the the ellipsis `...` for options.

+ **Bundle** - Run the HTML Bundler:

    ```text
    $ oohtml bundle
    ```

    See [Bundler Docs](bundling/README.md) for API details.

## Issues
To report bugs or request features, please submit an [issue](https://github.com/webqit/oohtml-cli/issues).

## License
MIT.
