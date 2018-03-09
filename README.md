`html-webpack-scripts-plugin` improves control over scripts generated by [Webpack](https://webpack.js.org/).

[![npm version](https://badge.fury.io/js/html-webpack-scripts-plugin.svg)](http://badge.fury.io/js/html-webpack-scripts-plugin)

[![Dependency Status](https://david-dm.org/hypotenuse/html-webpack-scripts-plugin.svg)](https://david-dm.org/hypotenuse-packs/html-webpack-scripts-plugin)

[![NPM](https://nodei.co/npm/html-webpack-scripts-plugin.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/html-webpack-scripts-plugin/)

## Introduction
[`html-webpack-plugin`](https://www.npmjs.com/package/html-webpack-plugin) will add scripts generated by [Webpack](https://webpack.js.org/) into generated HTML like `<script type="text/javascript" src="/bundle/vendor.0a78e31b5c440.js"></script>`  without need to include them manually. However it won't give you additional control. For example you can't set `defer` attribute or make them inline.

This plugin is similar to [`script-ext-html-webpack-plugin`](https://www.npmjs.com/package/script-ext-html-webpack-plugin), however it won't work in conjunction with [`html-webpack-template`](https://www.npmjs.com/package/html-webpack-template) meanwhile `html-webpack-scripts-plugin` will.

## Installation
```shell
npm install html-webpack-plugin --save-dev (Must be installed)
npm install html-webpack-scripts-plugin --save-dev
```

Usage
----------------------

Suppose you have two scripts generated by webpack: 
`vendor.0a78e31b5c440.js` 
`app.4234fe71c300ea.js`
Plugin gives you ability to:

#### Add specific attributes like `async` `defer` `id` `charset`:
```js

// webpack.config.js

const HtmlWebpackScriptsPlugin = require('html-webpack-scripts-plugin')

const HtmlWebpackScriptsPluginInstance = new HtmlWebpackScriptsPlugin({
	'defer charset=utf8': /vendor/,
	'async id=appscript': /app/
})


module.exports = {
	...
	plugins: [..., HtmlWebpackScriptsPluginInstance]
	...
} 

```
Output:
```html
<script defer charset="utf8" type="text/javascript" src="vendor.0a78e31b5c440.js"></script>
<script async id="appscript" type="text/javascript" src="app.4234fe71c300ea.js"></script>
```

#### Add custom attributes like `data-*`
```js

// webpack.config.js

const HtmlWebpackScriptsPlugin = require('html-webpack-scripts-plugin')

const HtmlWebpackScriptsPluginInstance = new HtmlWebpackScriptsPlugin({
	'defer data-script-defer=true': /vendor/, 
	'charset=utf8 id=appscript inline data-script-inline=true': /app/
})

module.exports = {
	...
	plugins: [..., HtmlWebpackScriptsPluginInstance]
	...
}

```
Output:
```html
<script defer data-script-defer="true" type="text/javascript" src="vendor.0a78e31b5c440.js"></script>
<script charset="utf8" id="appscript" data-script-inline="true" type="text/javascript"> /* Content of app.4234fe71c300ea.js */ </script>
```

#### Make scripts inline:
```js
// vendor.0a78e31b5c440.js
console.log('Hi')

// app.4234fe71c300ea.js
console.log('Webpack')
```

```js

// webpack.config.js

const HtmlWebpackScriptsPlugin = require('html-webpack-scripts-plugin')

const HtmlWebpackScriptsPluginInstance = new HtmlWebpackScriptsPlugin({ inline: /vendor|app/ })

module.exports = {
	...
	plugins: [..., HtmlWebpackScriptsPluginInstance]
	...
}

```
Output:
```html
<script type="text/javascript">console.log('Hi')</script>
<script type="text/javascript">console.log('Webpack')</script>
```
