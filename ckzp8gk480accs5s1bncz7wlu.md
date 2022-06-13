## Create a react application without create-react-app

**create-react-app (CRA)** is facebook's officially-endorsed way to create a react application. It is a very good choice to start with, especially for those who are new to the React world or who just don't want to spend time setting up everything.

But, there are some disadvantages to this method. It abstracts everything. Due to that beginners may think CRA is the only way to start the react apps and might not know that transpiler(babel), bundler(webpack) are the key dependencies that are used under the hood by CRA. In addition to that, It will install extra dependencies such as scss-loader which may not be required in projects developed using plain CSS. 

The alternative to CRA is setting up your own boilerplate. It is difficult to configure at the beginning but it allows us to easily customise as per our requirements. Let's see how we can configure that in this article.

**Step-1:  Initialize your project with npm** 

I assume that you have already installed and configured the node.

```
npm init -y
```

By running this command, a new package.json file will be created with default values.

**Step-2: Install react and react-dom**

These are the only two runtime dependencies needed for react application.

```
npm install react react-dom --save

```

**Step-3:  Install and configure Transpiler(Babel)**

Old browsers cannot understand JSX and ES6 code. We need to install a transpiler to convert it into a backwards-compatible javascript code. 

```
npm install @babel/core @babel/preset-env @babel/cli @babel/preset-react --save-dev
```

- **babel/core **is a module that contains the main functionality of Babel,
- **babel/cli** is a module that allows us to use babel from the terminal,
- **babel/preset-env** is preset that handles the transformation of ES6 syntax into common javascript,
- **babel/preset-react** is preset that deals with JSX and converts it into vanilla javascript.
- **--save-dev** means save all above-installed modules in devDependencies in package.json file


To configure Babel, create a file in your root directory with the name **.babelrc**, and include the following:

```
 {
     "presets": [ "@babel/preset-env", "@babel/preset-react" ]
 }
```

**Step-4: Install bundlers and loaders **

Bundlers bundle all the JavaScript files in your project and prepare all the necessary resources for usage in the browser.

```
npm install webpack webpack-cli webpack-dev-server babel-loader css-loader style-loader html-webpack-plugin --save-dev
``` 
- **webpack:** the bundler.
- **webpack-cli:** CLI for Webpack.
- **babel-loader:** loader for Babel.
- **style-loader:** loader that injects styles into the DOM.
- **css-loader:** loader for CSS.
- **html-webpack-plugin:** this plugin is used to create HTML files that will serve bundles.

**Step-5: Create HTML template and Components to render**

Create index.html file inside the src folder of your project. It will be used by HTMLWebpackPlugin as a template to generate the new one.

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>React Boiler Plate</title>
</head>
<body>
    <div id="root"></div>
</body>
</html>
```

We need to create our App react component in src folder.

```
import React from "react";

export default function App() {
  return <div>React Boiler Plate</div>;
}
```

Next, we need to create index.js file which serves as the entry point for your react application.

```
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(<App />, document.querySelector("#root"));

```

**Step-6: Configure webpack and build scripts**

Create a file called webpack.config.js inside your root directory and add configuration as below.

```
const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: path.resolve(__dirname, "src", "index.js"),
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "bundle.js",
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
        },
      },
      {
        test: /\.css$/,
        use: [
          {
            loader: "style-loader",
          },
          {
            loader: "css-loader",
          },
        ],
      },
    ],
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./src/index.html",
    }),
  ],
};

```

Next, add the below scripts inside your package.json file

```
 "scripts": {
     "start": "webpack-dev-server --mode=development --open --hot",
     "build": "rm -rf dist && webpack --mode=production"
  }
```

**Step-7:Building and running our app**

Finally, run the below command in the terminal to start the application in development mode.

```
npm start
```

Our app is now available at:** localhost:3000**

And... that's it! Now we have our React application ready to start working with it 🙌🏼

If you've reached this point, thank you very much. I hope that this tutorial has been helpful for you and I'll see you all in the next.
