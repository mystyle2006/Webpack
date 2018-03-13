Output Management
=================
In this Tutorial, we're going to learn how to manage our output in our apps.

Preparation
-------------
##### project

```
webpack-demo
  |- package.json
  |- webpack.config.js
  |- /dist
  |- /src
    |- index.js
+   |- print.js
  |- /node_modules
```

Let's make our <b>package.json</b>. Please type the bellow command at command window.

```
npm init -y
npm install --save-dev webpack@3.6.0
```

You can see our package.json like below

```json
{
  "name": "Output-Management",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^3.6.0"
  }
}
```

##### src/index.js
```js
  import _ from 'lodash';
  import printMe from './print.js';

  function component() {
    var element = document.createElement('div');
    var btn = document.createElement('button');

    element.innerHTML = _.join(['Hello', 'webpack'], ' ');

    btn.innerHTML = 'Click me and check the console!';
    btn.onclick = printMe;

    element.appendChild(btn);

    return element;
  }

  document.body.appendChild(component());
```

##### src/print.js
```js
  export default function printMe() {
    console.log('I get called from print.js!');
  }
```

##### dist/index.html
```html
<!doctype html>
  <html>
    <head>
      <title>Output Management</title>
      <script src="./print.bundle.js"></script>
    </head>
    <body>
      <script src="./bundle.js"></script>
      <script src="./app.bundle.js"></script>
    </body>
  </html>
```

##### webpack.config.js

```js
const path = require('path');

  module.exports = {
    entry: {
      app: './src/index.js',
      print: './src/print.js'
    },
    output: {
      filename: '[name].bundle.js',
      path: path.resolve(__dirname, 'dist')
    }
  };
```

Let's run <b>npm run build</b>

```
Hash: aa305b0f3373c63c9051
Version: webpack 3.0.0
Time: 536ms
          Asset     Size  Chunks                    Chunk Names
  app.bundle.js   545 kB    0, 1  [emitted]  [big]  app
  print.bundle.js  2.74 kB       1  [emitted]         print
   [0] ./src/print.js 84 bytes {0} {1} [built]
   [1] ./src/index.js 403 bytes {0} [built]
   [3] (webpack)/buildin/global.js 509 bytes {0} [built]
   [4] (webpack)/buildin/module.js 517 bytes {0} [built]
    + 1 hidden module
```

After build, we cann see the files ```app.bundle.js``` and ```print.bundle.js``` in our dist folder. 

but what would happen if we need other .js scripts or change the name of entry points's name? we might change our output options as well.
So let's fix with this plugin [HtmlWebpackPlugin](https://webpack.js.org/plugins/html-webpack-plugin/)

## Setting up HtmlWebpackPlugin
First install the plugin and adjust the ```webpack.config.js```

```
npm install --save-dev html-webpack-plugin
```

##### webpack.config.js
```js
const path = require('path');
+ const HtmlWebpackPlugin = require('html-webpack-plugin');

  module.exports = {
    entry: {
      app: './src/index.js',
      print: './src/print.js'
    },
+   plugins: [
+     new HtmlWebpackPlugin({
+       title: 'Output Management'
+     })
+   ],
    output: {
      filename: '[name].bundle.js',
      path: path.resolve(__dirname, 'dist')
    }
  };
```

We had to change output options when we change our entry points. but this plugin will help us not to do it because this will create ```index.html``` with the bundle files automatically.

Now it's time to run ```npm run build``` and check out the ```/index``` folder.

## Cleaning up the ```/dist``` folder
As you might have noticed after the past build, out ```/dist``` folder has become quite cluttered. However, ```clean-webpack-plugin``` will create ```/dist``` folder automatically with only used files.

and now install the plugins and give it a edit like below.

```
npm install clean-webpack-plugin --save-dev
```

```
  const path = require('path');
  const HtmlWebpackPlugin = require('html-webpack-plugin');
+ const CleanWebpackPlugin = require('clean-webpack-plugin');

  module.exports = {
    entry: {
      app: './src/index.js',
      print: './src/print.js'
    },
    plugins: [
+     new CleanWebpackPlugin(['dist']),
      new HtmlWebpackPlugin({
        title: 'Output Management'
      })
    ],
    output: {
      filename: '[name].bundle.js',
      path: path.resolve(__dirname, 'dist')
    }
  };
```

now you can see the newly created ```/dist``` folder.
