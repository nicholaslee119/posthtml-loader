# PostHTML Loader <img align="right" width="220" height="200" title="PostHTML logo" src="http://posthtml.github.io/posthtml/logo.svg">

[![npm][npm]][npm-url]
[![tests][travis]][travis-url]
[![dependencies][deps]][deps-url]
[![coverage][cover]][cover-url]
[![Code Style][style]][style-url]

A PostHTML loader for webpack

## Installation

```sh
npm i html-loader posthtml-loader --save
```

## Usage

The posthtml loader must be used with at least one other loader in order to integrate with webpack correctly. For most use cases, the [html-loader](https://github.com/webpack/html-loader) is recommended. If you want to export the html string directly for use in javascript or webpack plugins, we recommend the [source-loader](https://github.com/static-dev/source-loader). Whichever loader you choose, it should be the first loader, followed by posthtml, as you will see in the examples below.

Options can be passed through a `posthtml` option directly on the webpack config object. It accepts an array, an object, or a function that returns an array or object. If it's an array, it should contain plugins. If it's an object, it can contain a `plugins` key, which is an array of plugins and an optional `parser` key which allows you to pass in a custom parser. Any other key will apply to the `pack` querystring parameter, documented below.

Basic configuration example:

```js
// webpack.config.js
module: {
  loaders: [{
    test: /\.html$/,
    loader: 'html!posthtml'
  }]
},
posthtml: [/* plugins here */]
```

### Plugin Packs

If you need to apply different sets of plugins to different groups of files, you can use a **plugin pack**. Just add `pack=[name]` as a querystring option, and return an object from the `posthtml` config option with a key matching the pack name, and the value being an array of plugins.

```js
// webpack.config.js
module: {
  loaders: [{
    test: /\\.special\.html$/,
    loader: 'html!posthtml?pack=special'
  }]
},
posthtml: {
  plugins: [/* plugins that apply anything that's not using a pack */],
  special: [ /* plugins specific to the "special" pack */ ],
}
```

### Using a Function

You can also return a function from the `posthtml` config value, if you need to for any reason. The function passes along the [loader context](https://webpack.github.io/docs/loaders.html#loader-context) as an argument, so you can get information about the file currently being processed from this and pass it to plugins if needed. For example:

```js
// webpack.config.js
module: {
  loaders: [{
    test: /\.html$/,
    loader: 'html!posthtml'
  }]
},
posthtml: (ctx) => {
  return [examplePlugin({ filename: ctx.resourcePath })]
}
```

### Custom Parser

If you want to use a custom parser, you can pass it in under the `parser` key. Below is an example with the [sugarml parser](https://github.com/posthtml/sugarml):

```js
// webpack.config.js
const sugarml = require('sugarml')

module: {
  loaders: [{
    test: /\\.special\.html$/,
    loader: 'html!posthtml?pack=special'
  }]
},
posthtml: {
  plugins: [/* posthtml plugins */],
  parser: sugarml
}
```

## License & Contributing

- Licensed under [MIT](LICENSE)
- See [contributing guidelines](CONTRIBUTING.md)

[npm]: https://img.shields.io/npm/v/posthtml-loader.svg
[npm-url]: https://npmjs.com/package/posthtml-loader

[deps]: https://david-dm.org/posthtml/posthtml-loader.svg
[deps-url]: https://david-dm.org/posthtml/posthtml-loader

[devdeps]: https://david-dm.org/posthtml/posthtml-loader/dev-status.svg
[devdeps-url]: https://david-dm.org/posthtml/posthtml-loader#info=devDependencies

[style]: https://img.shields.io/badge/code%20style-standard-yellow.svg
[style-url]: http://standardjs.com/

[travis]: http://img.shields.io/travis/posthtml/posthtml-loader.svg
[travis-url]: https://travis-ci.org/posthtml/posthtml-loader

[cover]: https://coveralls.io/repos/github/posthtml/posthtml-loader/badge.svg?branch=master
[cover-url]: https://coveralls.io/github/posthtml/posthtml-loader?branch=master
