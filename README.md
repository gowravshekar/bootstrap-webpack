bootstrap-webpack
=================

bootstrap package for webpack.

This is the `less` version. If you are looking for the `sass` version, refer to [bootstrap-sass-loader](https://github.com/justin808/bootstrap-sass-loader).


Usage
-----

Bootstrap use some fonts. You need to configure the correct loaders in your `webpack.config.js`. Example:

``` javascript
module.exports = {
  module: {
    loaders: [
      // the url-loader uses DataUrls.
      // the file-loader emits files.
      { test: /\.woff$/,   loader: "url-loader?limit=10000&mimetype=application/font-woff" },
      { test: /\.ttf$/,    loader: "file-loader" },
      { test: /\.eot$/,    loader: "file-loader" },
      { test: /\.svg$/,    loader: "file-loader" }
    ]
  }
};
```

### Complete Bootstrap

To use the complete bootstrap package including styles and scripts with the default settings:

``` javascript
require("bootstrap-webpack");
```

### Custom configuration

You can configurate bootstrap-webpack with two configuration files:

* `bootstrap.config.js`
* `bootstrap.config.less`

Add both files next to each other to your project. And:

``` javascript
require("bootstrap-webpack!./bootstrap.config.js");
```

Or simple add it as entry point to your `webpack.config.js`:

``` javascript
module.exports = {
  entry: [
    "bootstrap-webpack!./bootstrap.config.js",
    "your-existing-entry-point"
  ]
};
```

#### `bootstrap.config.js`

Example:

``` javascript
module.exports = {
  scripts: {
    // add every bootstrap script you need
    'transition': true
  },
  styles: {
    // add every bootstrap style you need
    "mixins": true,

    "normalize": true,
    "print": true,

    "scaffolding": true,
    "type": true,
  }
};
```

#### `bootstrap.config.less`

Write less code. I. e. overwrite the default colors or sizes.

Example:

``` less
@font-size-base:          24px;

@btn-default-color:              #444;
@btn-default-bg:                 #eee;
```

### extract-text-webpack-plugin

Configure post style loaders in `bootstrap.config.js`.

Example:

``` javascript
module.exports = {
  postStyleLoaders: [
    require.resolve('extract-text-webpack-plugin/loader.js') + '?{"omit":1,"extract":true,"remove":true}'
  ],
  scripts: {
    ...
  },
  styles: {
    ...
  }
};
```

Install `extract-text-webpack-plugin` before using this configuration.

### Extracting bootstrap and your css together
If you would like bootstrap and your own styles in a single extracted css file, the above will not work. Instead use:

```
var ExtractTextPlugin = require('extract-text-webpack-plugin');
var webpack = require('webpack');

module.exports = {
  entry: [
    ExtractTextPlugin.extract('style', 'css!less!bootstrap-webpack/bootstrap-styles!./bootstrap.config.js'),
    'bootstrap-webpack/bootstrap-scripts!./bootstrap.config.js',
    <your app entry point>
  ],
  loader: [
    {
      test: /\.(css|less)$/,
      loader: ExtractTextPlugin.extract('style', 'css!less')
    }
  ],
  output: {
    <your output configuration>
  },
  plugins: [
    new webpack.ProvidePlugin({
        jQuery: 'jquery'
    }),
    new ExtractTextPlugin('styles.css')
  ]
```

You need to use the provide plugin to make sure jquery is available for bootstrap scripts
