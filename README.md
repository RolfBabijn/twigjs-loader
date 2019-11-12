# twigjs-loader
[![Build Status](https://travis-ci.org/megahertz/twigjs-loader.svg?branch=master)](https://travis-ci.org/megahertz/twigjs-loader)
[![npm version](https://badge.fury.io/js/twigjs-loader.svg)](https://badge.fury.io/js/twigjs-loader)
[![Dependencies status](https://david-dm.org/megahertz/twigjs-loader/status.svg)](https://david-dm.org/megahertz/twigjs-loader)

## Description

twig.js loader for Webpack


## Installation

This package requires node.js 8 at least.

Install with [npm](https://npmjs.org/package/twigjs-loader):

    $ npm install -D twigjs-loader

## Usage

```js
const indexView = require('./index.twig');
console.log(indexView({ variable1: 'value' }));
```

**webpack.config.js**

```js
module.exports = {
  //...
  module: {
    rules: [
      {
        test: /\.twig$/,
        use: 'twigjs-loader',
      },
      //...
    ],
  },
  //...
}

```

### With Express

 - [example](examples/express)
 - [typescript example](examples/typescript)

`$ npm install twigjs-loader`

**index.js:**
```js
import * as express from 'express';
import { ExpressView } from 'twigjs-loader';
import indexView from './views/index.twig';

const app = express();
app.set('view', ExpressView);

app.get('/', (req, res) => {
  res.render(indexView, {
    url: req.originalUrl,
  })
});

app.listen(8080);
```

### On frontend

 - [example](examples/frontend)

```js
import indexView from './views/index.twig';

document.body.innerHTML = indexView({
  url: location.href,
})
```

## Configure

You can configure how a template is compiled by webpack using the
`renderTemplate` option. For example:

**webpack.config.js**

```js
module.exports = {
  //...
  module: {
    rules: [
      {
        test: /\.twig$/,
        use:  {
          loader: 'twigjs-loader',
          options: {
            renderTemplate(twigData, dependencies) {
              return `
                ${dependencies}
                var twig = require("twig").twig;
                var tpl = twig(${JSON.stringify(twigData)});
                module.exports = function(context) { return tpl.render(context); };
              `;
            },
          },
        },
      },
      //...
    ],
  },
  //...
}

```

## Credits

Based on [zimmo-be/twig-loader](https://github.com/zimmo-be/twig-loader)
