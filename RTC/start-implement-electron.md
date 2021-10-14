# Implement a client for [product-name]

This section shows how to use the [sdk-name] to implement [product-name] into your app step by step.

## Create the user interface

To create the user interface, copy the following code into  `index.html`:

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

## Create the main process

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

## Implement the [product-name] logic
To implement the minimum client logic of Video Call, you need to do the following things:

<ol>
<li>Call <code>initialize</code> to create an <ph keyref="IRtcEngine"/> instance.</li>
<li props="live">Call <code>setChannelProfile</code> to set the channel profile as live streaming.</li>
<li props="live">Call <code>setClientRole</code> to set the user role as host or audience. The host publishes streams to the channel, and the audience subscribes to the streams.</li>
<li>Call <code>enableVideo</code> to enable the video module.</li>
<li>Call <code>joinChannel</code> to join a channel.</li>
<li>Call <code>setupLocalVideo</code> to set up the local video frame.</li>
<li>When another user joins the channel, call <code>setupRemoteVideo</code> to set up the remote video frame.</li>
<li>Leave the channel and release all the resources used by <ph keyref="IRtcEngine"/>.</li>
</ol>

The API call sequence is shown in the following diagram:

![start-api-sequence-electron]

Copy the following code into `renderer.js` and fill in `APPID`, `token`, and `ChannelName` with [values from your Agora project](https://docs.agora.io/en/AgoraPlatform/get_appid_token):

<p props="video" conref="conref/get-started-sample-code-electron.dita#get-started-sample-code/impl-video"/>
<p props="live" conref="conref/get-started-sample-code-electron.dita#get-started-sample-code/impl-live"/>