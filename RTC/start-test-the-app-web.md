# Test your app

This quickstart uses [webpack](https://webpack.js.org/) to package the project and `webpack-dev-server` to run your project.

1. In `package.json`, add `webpack`, `webpack-cli`, and `webpack-dev-server` to the `dependencies` field, and the `build` and `start:dev` commands to the `scripts` field.

   ```json
   {
     "name": "agora_web_quickstart",
     "version": "1.0.0",
     "description": "",
     "main": "main.js",
     "scripts": {
       "test": "echo \"Error: no test specified\" && exit 1",
       "build": "webpack --config webpack.config.js",
       "start:dev": "webpack serve --open --config webpack.config.js"
     },
     "dependencies": {
       "agora-rtc-sdk-ng": "latest",
       "webpack": "5.28.0",
       "webpack-dev-server": "3.11.2",
       "webpack-cli": "4.5.0"
     },
     "author": "",
     "license": "ISC"
   }
   ```

2. Create a file named `webpack.config.js` in the project directory to configure webpack, with the following code: 

   ```javascript
   const path = require('path');
   
    module.exports = {
    entry: './main.js',
    output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, './dist'),
    },
    devServer: {
    compress: true,
    port: 9000
    }
    };
   ```

3. To install dependencies, run the following command:

   ```shell
   npm install
   ```

4. To build and run the project using webpack, run the following command:

   ```shell
   # Use webpack to package the project
   npm run build
   ```

5. Use webpack-dev-server to run the project:

   ```shell
   npm run start:dev
   ```

A local web server automatically opens in your browser. You see the following page:

![](https://web-cdn.agora.io/docs-files/1619428543085)

Click **JOIN** to start a [feature]. However, being in a [feature] on your own is no fun. Ask a friend to join the same [feature] with you on the [start-demo-web].

<div class="alert info"><li>Running the web app through a local server (localhost) is for testing purposes only. In production, ensure that you use the HTTPS protocol when deploying your project.</li><li>Due to security limits on HTTP addresses except 127.0.0.1, the Web SDK only supports HTTPS or http://localhost (http://127.0.0.1). If you deploy your project over HTTP, you can only visit your project at http://localhost (http://127.0.0.1).</li></div>
