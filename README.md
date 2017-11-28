# require-wrapper

A wrapper module for Node's [require](https://nodejs.org/api/modules.html#modules_require) to use for requiring dynamic
dependencies in Webpack environment.

## Installation

```bash
npm install require-wrapper --save
```

## Usage

In [Webpack configuration file](https://webpack.js.org/configuration/) exclude this module from parsing by
setting [module.noParse](https://webpack.js.org/configuration/module/#module-noparse) option:

```javascript
module.exports = {
    // ...
    module: {
        rules: {
            // ...
        },
        noParse: /require-wrapper/
    }
};
```

or

```javascript
module.exports = {
    // ...
    module: {
        rules: {
            // ...
        },
        noParse: function(content) {
            return /require-wrapper/.test(content);
        }
    }
};
```

In your source code require the dependency module dynamically:

**dynamic/hello.js**

```javascript
module.exports = function sayHello() {
    console.log('hello world!');
};
```

**index.js**

```javascript
var nodeRequire = require('require-wrapper');
var helloModulePath = path.resolve(__dirname, 'dynamic/hello.js');
var sayHello = nodeRequire(helloModulePath);
sayHello(); // => hello world!
```

## Why?

Sometimes in Node applications you still need to require dynamic modules but Webpack parses
[require calls differently](https://webpack.js.org/guides/dependency-management/#require-with-expression). If it's not
the case you need then wrap Node's `require` function as proposed in this module.
