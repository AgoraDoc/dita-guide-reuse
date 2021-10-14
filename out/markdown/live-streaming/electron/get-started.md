# Get Started with Interactive Live Streaming Premium for Electron {#get-started-with-product-name-for-platform}

The Agora Video SDK enables you to develop rapidly to enhance your social, work, education, and IoT apps with real-time engagements.

This page shows the minimum code you need to add live streaming into your app by using the Agora Video SDK for Electron.

## Understand the tech {#understand-the-tech}

The following figure shows the workflow of a live streaming implemented by the Agora SDK.

![](https://web-cdn.agora.io/docs-files/1625465916613)

To start a live streaming, you implement the following steps in your app:

1.  Set the client role
2.  Retrieve a token
3.  Join a channel
4.  Publish and subscribe to in the channel.

For an app client to join a channel, you need the following information:

-   The App ID: A randomly generated string provided by Agora for identifying your app. You can get the App ID from [Agora Console](https://console.agora.io/).

-   The user ID: The unique identifier of a user. You need to specify the user ID yourself, and ensure that it is unique in the channel.

-   A token: In a test or production environment, your app client retrieves tokens from your server. For rapid testing, you can use a temporary token with a validity period of 24 hours.

-   The channel name: A string that identifies the channel for the live streaming.


## Prerequisites {#prerequisites}

In order to follow the procedure in this page, you must have:

-   A valid Agora account.

-   A valid Agora project with an App ID and a temporary token. For details, see [Get Started with Agora](https://docs.agora.io/en/Agora%20Platform/get_appid_token?platform=All%20Platforms).

-   A Windows or macOS computer with access to the internet. If your network has a firewall, follow the instructions in [Firewall Requirements](https://docs.agora.io/en/Agora%20Platform/firewall?platform=All%20Platforms).

-   [Node.js](https://nodejs.org/en/download/) 6.9.1 or later.


## Project setup {#project-setup}

1.  For new projects, create a directory named `agora_electron_quickstart`. For a basic Electron app, create the following files in the directory:

    -   `package.json`: The configuration of your Electron project, such as project name and dependencies.

    -   `index.html`: The visual interface with the user.

    -   `main.js`: The main process of your Electron app.

    -   `renderer.js`: The renderer process of your Electron app, where the APIs provided by the Agora SDK are implemented.

2.  Integrate the Agora Video SDK into your project.

    1.  Modify the `package.json` file as follows:

        -   `electron_version`：The version number of Electron. You can set it to `12.0.0`, `11.0.0`, `10.2.0`, `9.0.0`, `7.1.2`, `6.1.7`, `5.0.8`, `4.2.8`, `3.0.6`, or `1.8.3`.

        -   `prebuilt`: Set it as `true` to prevent compatibility issues.

        -   `agora-electron-sdk`: The version number of the Agora Video SDK for Electron. See [Release notes](https://docs.agora.io/en/Voice/release_electron_video?platform=Electron) for details.

        -   `electron`: Set it as the same as `electron_version`.

        **macOS**

        ```
        {
         "name": "electron-demo-app",
         "version": "0.1.0",
         "author": "your name",
         "description": "My Electron app",
         "main": "main.js",
         "scripts": {
           "start": "electron ."
         },
         "agora_electron": {
           "electron_version": "10.2.0",
           "platform": "darwin",
           "prebuilt": true
         },
         "devDependencies": {
           "agora-electron-sdk": "3.4.2",
           "electron": "10.2.0"
         }
        }
        ```

        **Windows**

        ```
        { 
         "name": "electron-demo-app",
         "version": "0.1.0",
         "author": "your name",
         "description": "My Electron app",
         "main": "main.js",
         "scripts": {
           "start": "electron ."
         },
         "agora_electron": {
           "electron_version": "10.2.0",
           "platform": "win32",
           "prebuilt": true,
           "arch": "ia32"
         },
         "devDependencies": {
           "agora-electron-sdk": "3.4.2",
           "electron": "10.2.0"
         }
        }
        ```

    2.  In Terminal, go to the root folder of your project:

        **Note:** If you already have a node\_modules folder in the root folder of your project, Agora recommands deleting the `node_modules` folder before running the following commands. Otherwise, an error might occur.

        **macOS**

        Run the following command to install dependencies:

        ```
        npm install
        ```

        **Windows**

        1.  Run the following command to install the 32-bit version of Electron:

            ```
            npm install -D --arch=ia32 electron
            ```

        2.  Run the following command to install the other dependencies:

            ```
            npm install
            ```


## Implement a client for Interactive Live Streaming Premium {#implement-a-client-for-product-name}

This section shows how to use the Agora Video SDK to implement Interactive Live Streaming Premium into your app step by step.

### Create the user interface {#create-the-user-interface}

To create the user interface, copy the following code into `index.html`:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Electron Quickstart</title>
</head>
<body>
  <h1>Electron Quickstart</h1>
  <!--Add a frame for local video-->
  <div id="join-channel-local-video"></div>
  <!--Add a frame for remote video-->
  <div id="join-channel-remote-video"></div>
</body>
</html>
```

### Create the main process {#create-the-main-process}

To set up a basic main process for your Electron app, copy the following code into `main.js`:

```javascript
const { app, BrowserWindow } = require('electron')
const path = require('path')
 
// If you use Electron 9.x or later, be sure to set allowRendererProcessReuse as false
app.allowRendererProcessReuse = false
 
function createWindow() {
  // Create a browser window
  const mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      preload: path.join(__dirname, 'renderer.js'),
      nodeIntegration: true
    }
  })
  
  // Load contents in index.html
  mainWindow.loadFile('./index.html')
  // Enable developer tools in the browser window
  mainWindow.webContents.openDevTools()
}
 
// Manage the lifecycle of the browser window
app.whenReady().then(() => {
  createWindow()
  // Open a window if none are open (macOS)
  app.on('activate', function () {
    if (BrowserWindow.getAllWindows().length === 0) {
      createWindow()
    }
  })
})
 
// Quit the app when all windows are closed (Windows)
app.on('window-all-closed', function () {
  if (process.platform !== 'darwin') app.quit()
})
```

### Implement the Interactive Live Streaming Premium logic {#implement-the-product-name-logic}

To implement the minimum client logic of Video Call, you need to do the following things:

1.  Call `initialize` to create an instance.
2.  Call `setChannelProfile` to set the channel profile as live streaming.
3.  Call `setClientRole` to set the user role as host or audience. The host publishes streams to the channel, and the audience subscribes to the streams.
4.  Call `enableVideo` to enable the video module.
5.  Call `joinChannel` to join a channel.
6.  Call `setupLocalVideo` to set up the local video frame.
7.  When another user joins the channel, call `setupRemoteVideo` to set up the remote video frame.
8.  Leave the channel and release all the resources used by .

The API call sequence is shown in the following diagram:

![start-api-sequence-electron]

Copy the following code into `renderer.js` and fill in `APPID`, `token`, and `ChannelName` with [values from your Agora project](https://docs.agora.io/en/AgoraPlatform/get_appid_token):

```language-javascript

                     window.addEventListener('DOMContentLoaded',() => {
                     const AgoraRtcEngine = require('agora-electron-sdk').default
                     
                     
                     const os = require('os')
                     const path = require('path')
                     
                     // Pass your App ID here.
                     const APPID = "Your App ID"
                     // Pass your token here.
                     const token = "Your token"
                     
                     const localVideoContainer = document.getElementById('join-channel-local-video')
                     const remoteVideoContainer = document.getElementById('join-channel-remote-video')
                     const sdkLogPath = path.resolve(os.homedir(), "./test.log")
                     
                     let rtcEngine = new AgoraRtcEngine()
                     // Initialize AgoraRtcEngine.
                     rtcEngine.initialize(APPID)
                     
                     
                     // Listen for the "joinedChannel" event of the local user.
                     rtcEngine.on('joinedChannel', (channel, uid, elapsed) => {
                     // If the local user joins the channel, set up the local video frame.
                     rtcEngine.setupLocalVideo(localVideoContainer)
                     })
                     
                     rtcEngine.on('error', (err, msg) => {
                     console.log("Error!")
                     })
                     
                     // Listen for the "userJoined" event of the remote user.
                     rtcEngine.on('userJoined', (uid) => {
                     // If the remote user joins the channel, set up the remote video frame.
                     rtcEngine.setupRemoteVideo(uid, remoteVideoContainer)
                     })
                     
                     // For a live streaming scenario, set the channel profile as "1".
                     rtcEngine.setChannelProfile(1)
                     
                     // Set the user role as "1"(host) or "2"(audience).
                     rtcEngine.setClientRole(1)
                     
                     // Enable the video module
                     rtcEngine.enableVideo()
                     
                     rtcEngine.setLogFile(sdkLogPath)
                     
                     // Pass your channel name here, which must be the same as the channel name you filled in to generate the temporary token.
                     // Join the channel.
                     rtcEngine.joinChannel(token, "ChannelName", null, 123456)
                     
                     })
         
```

## Test your app {#test-your-app}

Take the following steps to test your Electron app:

1.  In Terminal, go to the root folder of your project and run the following command:

    ```
    npm start
    ```

    When the app launches, you should be able to see yourself on the local view.

2.  Ask a friend to join the call with you on the [demo app](https://webdemo.agora.io/basicVideoCall/index.html). Enter the same App ID and channel name.

    If your friend joins as a host, you should be able to see and hear each other; if as an audience member, you should only be able to see yourself while your friend can see and hear you.


## Next steps {#next-steps}

Generating a token by hand is not helpful in a production context. [Authenticate Your Users with Tokens](https://docs.agora.io/en/Video/token_server?platform=Android) shows you how to start the live streaming with a token that you retrieve from your server.

## See also {#see-also}

Agora provides an open source sample project  on GitHub for your reference.

