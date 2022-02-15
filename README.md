# SWC Paths & Extensions Issues

## Setup

Install dependencies:

```shell
yarn install
```

## Reproduction

SWC is configured to transform the input TypeScript files in `./src` into ESModules in `./build`.

1. Build source files with SWC:

```shell
yarn build
```

The source files have a `.js` extension in the imports for Node.js to resolve in ESM mode,
but a combination with the `jsc.paths` option in `.swcrc` strips them out.

Try removing the `jsc.paths` option in `.swcrc`:

```diff
{
  "jsc": {
    "baseUrl": "./src",
-   "paths": {
-     "@modules/*": ["./modules/*"]
-   },
    "target": "es2020",
    "parser": {
      "syntax": "typescript"
    }
  },
  "module": {
    "type": "es6"
  }
}
```

Then run the build again: now the paths keep their extension.

Note: extensions are also kept when the `jsc.paths` object exists, but is empty:

```diff
{
  "jsc": {
    "baseUrl": "./src",
+   "paths": {},
    "target": "es2020",
    "parser": {
      "syntax": "typescript"
    }
  },
  "module": {
    "type": "es6"
  }
}
```

The issue arises when there is **at least one** entry in the `jsc.paths` object.
