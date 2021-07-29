# React, Webpack 5 and Babel 7(?) Starter

Setting up a React project from scratch without CRA. 👷‍♀️

Gleaned from Robin Wieruch's [Webpack 5](https://www.robinwieruch.de/webpack-setup-tutorial/) [React Tutorials](https://www.robinwieruch.de/minimal-react-webpack-babel-setup)

## Actions

### 1. Setup Project

- `mkdir -p %PROJECT_NAME%/src`
- `git init`
- `npx gitignore node`
- `echo $(node -v) >.nvmrc`
- `npm init -y`

### 2. Setup Babel

- `npm add -D @babel/core @babel/preset-env @babel/preset-react babel-loader`
- create `.babelrc`

```json
{
  "presets": ["@babel/preset-env", "@babel/preset-react"]
}
```

### 3. Setup up Webpack

- `npm add -D webpack webpack-dev-server webpack-cli`
- add to `package.json` scripts

  ```json
  "scripts": {
  	"start": "webpack serve --config ./webpack.config.js --mode development",
    "build": "webpack --config ./webpack.config.js --mode production"
  }
  ```

### 4. Setup ReactJS

- `npm add react react-dom`
- `npm add -D react-hot-loader`

## Essential Files

- `webpack.config.js`

  ```jsx
  const webpack = require("webpack");
  const path = require("path");
  const HtmlWebpackPlugin = require("html-webpack-plugin");

  module.exports = {
    entry: path.resolve(__dirname, "./src/index.js"),
    module: {
      rules: [
        {
          test: /\.(js|jsx)$/,
          exclude: /node_modules/,
          use: ["babel-loader"]
        }
      ]
    },
    // WHAT FILES WE ARE BUNDLING
    resolve: {
      extensions: ["*", ".js", ".jsx"]
    },
    // WHERE WE PUT THE BUNDLED CODE
    output: {
      path: path.resolve(__dirname, "./dist"),
      filename: "bundle.js"
    },
    plugins: [
      // HOT RELOAD FOR DEV
      new webpack.HotModuleReplacementPlugin(),
      // GENERATE DIST HTML
      new HtmlWebpackPlugin({
        title: "What's this? A Webpack generated React project",
        // WHERE IS OUR HTML TEMPLATE
        template: path.resolve(__dirname, "./src/index.html")
      })
    ],
    // LIKE IT SAYS: DEV SERVER
    devServer: {
      contentBase: path.resolve(__dirname, "./dist"),
      hot: true
    }
  };
  ```

- `src/index.js`

```jsx
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

// Renders the App.jsx - so we'd better add one
ReactDOM.render(<App />, document.getElementById("app"));

// HOT RELOAD
module.hot.accept();
```

- `src/index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>React, Webpack & Babel Starter</title>
  </head>
  <body>
    <!-- OUR STARTER DIV  -->
    <div id="app"></div>
    <!-- OUR BUNDLED JS -->
    <script src="./bundle.js"></script>
  </body>
</html>
```

`src/index.html`

```jsx
import React from "react";

const title = "It's all in the hips";

// REALLY SIMPLE APP COMPONENT
const App = () => {
  return <div>{title}</div>;
};

export default App;
```

## The Magic

- `npm start` - will start the dev server
- `npm run build` - builds the production code in `./dist`
