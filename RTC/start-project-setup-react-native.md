# Project setup

Follow the steps to create the environment necessary to add video call into your app.

## Create a React Native project

1. Make sure you have set up the development environment based on your operating system and target platform.

2. Run the following command and fill in your project name in `ProjectName` to create and initialize a new React Native project:

   ```shell
   npx react-native init ProjectName
   ```

   A successful execution of this command generates a simple sample project in the directory that you run the command.

3. Launch your Android or iOS simulator and run your project by executing the following command:

   a. Run `npx react-native start` in the root of your project to start Metro.

   b. Open another terminal in the root of your project and run `npx react-native run-android` to start the Android app, or run `npx react-native run-ios` to start the iOS app.

   If everything is set up correctly, you should see your new app running in your Android or iOS simulator shortly.

   You can also run your project on a physical Android or iOS device. For detailed instructions, see [Running on device](https://reactnative.dev/docs/running-on-device).

Now that you have your project running successfully, you can start trying to integrate the Agora React Native SDK and modify your project.


## Integrate the SDK {#integrate}

This section describes how to integrate the SDK on React Native 0.60.0 or later.

If you use React Native 0.59.x, [Installing (React Native == 0.59.x](https://github.com/AgoraIO-Community/react-native-agora/blob/master/README.md#installing-react-native--059x) to integrate the [sdk-name] for [platform].

1. Install the latest version of the [sdk-name] for [platform] using npm:

   ```shell
   npm i --save react-native-agora
   ```
   
   Do not link native modules manually, as React Native 0.60.0 and later support automatically linking native modules. For details, see [Autolinking](https://github.com/react-native-community/cli/blob/master/docs/autolinking.md).
   
2. If your target platform is iOS, you also need to run the following command:

   ```shell
   npx pod-install
   ```
   
3. The [sdk-name] for [platform] uses Swift in native modules. Therefore, your project must support compiling Swift; Otherwise, you get errors when building the iOS app. Do the following:

   a. Open `ios/ProjectName.xcworkspace` with Xcode.

   b. Click **File > New > File**, select **iOS > Swift File**, and then click **Next > Create** to create a new `File.swift` file.


## Add TypeScript

The sample code in this article is written in TypeScript. If you want to use this sample code directly, you need to add support for TypeScript to your project.

1. Run one of the following commands in the root of your project to add TypeScript dependencies:

   **Method one**: Using npm

   ```shell
   npm install --save-dev typescript @types/jest @types/react @types/react-native @types/react-test-renderer
   ```

   **Method two**: Using yarn

   ```shell
   yarn add --dev typescript @types/jest @types/react @types/react-native @types/react-test-renderer
   ```

2. Create a `tsconfig.json` file in the root of your project, and copy the following code to the file:

   ```json
   {
     "compilerOptions": {
       "allowJs": true,
       "allowSyntheticDefaultImports": true,
       "esModuleInterop": true,
       "isolatedModules": true,
       "jsx": "react",
       "lib": ["es6"],
       "moduleResolution": "node",
       "noEmit": true,
       "strict": true,
       "target": "esnext"
     },
     "exclude": [
       "node_modules",
       "babel.config.js",
       "metro.config.js",
       "jest.config.js"
     ]
   }
   ```

3. Create a `jest.config.js` file in the root of your project, and copy the following code to the file:

   ```javascript
   module.exports = {
        preset: 'react-native',
        moduleFileExtensions: ['ts', 'tsx', 'js', 'jsx', 'json', 'node']
   };
   ```

4. Rename `App.js` to `App.tsx`.

For more information, see [Using TypeScript](https://reactnative.dev/docs/typescript).