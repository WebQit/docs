---
desc: OOHTML CLI installation guide.
_index: last
---
# Installation

OOHTML-CLI is a command-line tool available as an npm package.

With [npm available on your terminal](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm), run the following command to install OOHTML CLI as a *dev dependency* to your project:

```text
$ npm i -g @webqit/oohtml-cli --save-dev
```

The `-g` flag makes it a global installation. It makes the `oohtml` command available from any location on your terminal. Omit this flag to install OOHTML CLI to just your project directory.

To test, run `oohtml help`; an overview of available commands will be shown.

<html-import name="oohtml-help" template="page/tooling/oohtml-cli/assets/img"></html-import>

## Next Steps

Find the documentation for each command in the [commands section](../../commands).