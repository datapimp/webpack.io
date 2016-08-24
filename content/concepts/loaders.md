---
title: Loaders
---

webpack supports modules written in a variety of languages and preprocessors, via _loaders_. _Loaders_ describe to webpack **how** to process non-JavaScript _modules_ and include these _dependencies_ into your _bundles_.

In this section we will look at how we can use loaders to include any type of file we can point webpack to as javascript code. Loaders allow us to simply `require` any file in our application.  The webpack compiler can be instructed to use any combination of loaders for any type of file, through either the configuration file or by using loader syntax in your require statements, e.g.

```javascript
const jQuery = require('expose?jQuery!jquery')
const Settings = require('yaml!./settings.yml')
const Styles = require('css!less!./styles.less')
```

In this section we will look at the basic configuration pattern that webpack uses, and the syntax for specifying loader options for a file in the require statements. We will include links at the end with more advanced topics, but if you are interested in writing your own loaders see `TODO`

## module.loaders Configuration

webpack loader configuration is specified as an array of objects. These objects are used to test every path to every file webpack comes across once it has analyzed all of the require statements. The objects are sets of rules used to assign responsibility for certain files to the desired webpack loader.  The order of the objects matter, because the first rule to match a file wins.

Let's look at what a loader configuration is saying. Starting with the project.

**In the context of the current project directory, I have files which will be loaded by `babelLoader` and by `cssLoader`**

```javascript
const Config = {
  context: process.cwd(),

  module: {
    loaders: [
      babelLoader,
      cssLoader,
    ]
  }
}
```

What is in a loader configuration?

```javascript
const babelLoader = {

  // Take any file which matches this pattern
  test: /\.js$/,

  // IF the full path of the file matches all of include rules
  include: [
    // it is in the current working directory
    process.cwd(),

    // if it is in a sub folder called components, containers, or stores
    /\/(components|containers|stores)\//,

    // if the following function returns true when passed the path in question
    function (path) {
      return !path.match(/\.raw/)
    }
  ],

  // AND!
  // the full path of the file doesn't match any of the exclude rules
  exclude: [
    /vendor\//
  ],

  // THEN!
  // I am your loader.
  loader: 'babel'

  // Please, use this configuration
  query: {
    babelRc: false,
    presets: ['react','es-2015']
  }
}
```
