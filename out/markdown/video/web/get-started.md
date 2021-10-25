# Get Started with Video Call for Web {#get-started-with-product-name-for-platform}

The Agora Video SDK enables you to develop rapidly to enhance your social, work, education, and IoT apps with real-time engagements.

This page shows the minimum code you need to add video call into your app by using the Agora Video SDK for Web.

## Understand the tech {#understand-the-tech}

The following figure shows the workflow of a video call implemented by the Agora SDK.

![Basic workflow](https://web-cdn.agora.io/docs-files/1627550978702)

To start a video call, you implement the following steps in your app:

1.  Retrieve a token
2.  Join a channel
3.  Publish and subscribe to audio and video in the channel. You can use the `LocalTrack` and `RemoteTrack` objects to publish and subscirbe to audio and video tracks in the channel.

For an app client to join a channel, you need the following information:

-   The App ID: A randomly generated string provided by Agora for identifying your app. You can get the App ID from [Agora Console](https://console.agora.io/).

-   The user ID: The unique identifier of a user. You need to specify the user ID yourself, and ensure that it is unique in the channel.

-   A token: In a test or production environment, your app client retrieves tokens from your server. For rapid testing, you can use a temporary token with a validity period of 24 hours.

-   The channel name: A string that identifies the channel for the video call.


## Prerequisites {#prerequisites}

Before proceeding, ensure that your development environment meets the following requirements:

In order to follow the procedure in this page, you must have:

-   A valid Agora account

-   An [Agora project](https://docs.agora.io/en/Agora%20Platform/get_appid_token?platform=Web) with an App ID and a temp token

-   A Windows or macOS computer meets the following requirements:

    -   Access to the Internet. If your network has a firewall, follow the instructions in [Firewall Requirements](https://docs.agora.io/en/Agora%20Platform/firewall?platform=Web) to access Agora services.

    -   A browser that matches the [supported browser list](https://docs.agora.io/en/Video/web_sdk_compatibility?platform=Web). Agora highly recommends using the [latest version](https://www.google.com/chrome/) of Google Chrome.

    -   Physical audio and video input devices

    -   An Intel 2.2GHz Core i3/i5/i7 processor \(2nd generation\) or equivalent

-   [Node.js and npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)


## Project setup {#project-setup}

Create a new directory named `agora_web_quickstart`. For a minimal Web app client, create the following files in the directory :

-   `index.html`: The visual interface with the user.

-   `main.js`: The programmable interface with `AgoraRTCClient` to implement the client logic.

-   `package.json`: Install and manage the dependencies of your project. To create the `package.json` file, you can navigate to the `agora_web_quickstart` directory on the command line and run `npm init`.


## Implement a client for video call {#implement-a-client-for-feature}

The following section shows how to use the Agora Video SDK for Web to add video call into your Web app step by step.

### Integrate the SDK {#integrate-the-sdk}

Integrate the the Agora Video SDK for Web into your project through npm, as follows:

1.  In `package.json`, add `agora-rtc-sdk-ng` and its version number to the `dependencies` field:

    ```json
    {
      "name": "agora_web_quickstart",
      "version": "1.0.0",
      "description": "",
      "main": "main.js",
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
      },
      "dependencies": {
        "agora-rtc-sdk-ng": "latest",
      },
      "author": "",
      "license": "ISC"
    }
    ```

2.  To import the `AgoraRTC` module in your project, copy the following code into `main.js`.

    ```javascript
    import AgoraRTC from "agora-rtc-sdk-ng"
    ```


### Implement the user interface {#implement-the-user-interface}

To implement the user interface, copy the following code into `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Agora Quickstart</title>
    <!--
      This line is used to refer to the bundle.js file packaged by webpack. A sample webpack configuration is shown in the later step of running your app.
    -->
    <script src="./dist/bundle.js"></script>
</head>
<body>
    <h2 class="left-align">Agora Quickstart</h2>
        <div class="row">
            <div>
                <button type="button" id="join">Join</button>
                <button type="button" id="leave">Leave</button>
            </div>
        </div>
</body>
</html>
```

### Implement the client logic {#implement-the-client-logic}

To implement the client logic of video call, we need to do the following things:

1.  Call [`createClient`](https://docs.agora.io/en/Video/API%20Reference/web_ng/interfaces/iagorartc.html#createclient) to create an `AgoraRTCClient` object.

2.  Call [`join`](https://docs.agora.io/en/Video/API%20Reference/web_ng/interfaces/iagorartcclient.html#join) to join an RTC channel with the App ID, user ID, token, and channel name.

3.  Call [`createMicrophoneAudioTrack`](https://docs.agora.io/en/Video/API%20Reference/web_ng/interfaces/iagorartc.html#createmicrophoneaudiotrack) to create a local audio track from the audio sampled by a microphone, call [`createCameraVideoTrack`](https://docs.agora.io/en/Video/API%20Reference/web_ng/interfaces/iagorartc.html#createcameravideotrack) to create a local video track from the video captured by a camera, and then call `publish` to publish the local audio and video tracks to the channel.

4.  When a remote user joins the channel and publishes tracks:

    1.  Listen for the [`client.on("user-published")`](https://docs.agora.io/en/Video/API%20Reference/web_ng/interfaces/iagorartcclient.html#event_user_published) event. When the SDK triggers this event, get an `AgoraRTCRemoteUser` object from this event.

    2.  Call [`subscribe`](https://docs.agora.io/en/Video/API%20Reference/web_ng/interfaces/iagorartcclient.html#subscribe) to subscribe to the `AgoraRTCRemoteUser` object and get the `RemoteAudioTrack` and `RemoteVideoTrack` objects of the remote user.

    3.  Call [`play`](https://docs.agora.io/en/Video/API%20Reference/web_ng/interfaces/itrack.html#play) to play the remote tracks.


The following figure shows the API call sequence of the basic one-to-one video call.

![API seqeunce of video call](https://web-cdn.agora.io/docs-files/1617877136130)

Copy the following code into `main.js` and then replace `appID` and `token` with [values from your Agora project](get-started.md#).

```javascript
import AgoraRTC from "agora-rtc-sdk-ng"

let rtc = {
    localAudioTrack: null,
    localVideoTrack: null,
    client: null
};

let options = {
    // Pass your App ID here.
    appId: "Your App ID",
    // Set the channel name.
    channel: "test",
    // Pass your temp token here.
    token: "Your temp token",
    // Set the user ID.
    uid: 123456
};

async function startBasicCall() {
    // Create an AgoraRTCClient object.
    rtc.client = AgoraRTC.createClient({ mode: "rtc", codec: "vp8" });
    
    // Listen for the "user-published" event, from which you can get an AgoraRTCRemoteUser object.
    rtc.client.on("user-published", async (user, mediaType) => {
        // Subscribe to the remote user when the SDK triggers the "user-published" event
        await rtc.client.subscribe(user, mediaType);
        console.log("subscribe success");

        // If the remote user publishes a video track.
        if (mediaType === "video") {
            // Get the RemoteVideoTrack object in the AgoraRTCRemoteUser object.
            const remoteVideoTrack = user.videoTrack;
            // Dynamically create a container in the form of a DIV element for playing the remote video track.
            const remotePlayerContainer = document.createElement("div");
            // Specify the ID of the DIV container. You can use the uid of the remote user.
            remotePlayerContainer.id = user.uid.toString();
            remotePlayerContainer.textContent = "Remote user " + user.uid.toString();
            remotePlayerContainer.style.width = "640px";
            remotePlayerContainer.style.height = "480px";
            document.body.append(remotePlayerContainer);

            // Play the remote video track.
            // Pass the DIV container and the SDK dynamically creates a player in the container for playing the remote video track.
            remoteVideoTrack.play(remotePlayerContainer);

            // Or just pass the ID of the DIV container.
            // remoteVideoTrack.play(playerContainer.id);
        }

        // If the remote user publishes an audio track.
        if (mediaType === "audio") {
            // Get the RemoteAudioTrack object in the AgoraRTCRemoteUser object.
            const remoteAudioTrack = user.audioTrack;
            // Play the remote audio track. No need to pass any DOM element.
            remoteAudioTrack.play();
        }

        // Listen for the "user-unpublished" event
        rtc.client.on("user-unpublished", user => {
            // Get the dynamically created DIV container.
            const remotePlayerContainer = document.getElementById(user.uid);
            // Destroy the container.
            remotePlayerContainer.remove();
        });

    });

    window.onload = function () {

        document.getElementById("join").onclick = async function () {
            // Join an RTC channel.
            await rtc.client.join(options.appId, options.channel, options.token, options.uid);
            // Create a local audio track from the audio sampled by a microphone.
            rtc.localAudioTrack = await AgoraRTC.createMicrophoneAudioTrack();
            // Create a local video track from the video captured by a camera.
            rtc.localVideoTrack = await AgoraRTC.createCameraVideoTrack();
            // Publish the local audio and video tracks to the RTC channel.
            await rtc.client.publish([rtc.localAudioTrack, rtc.localVideoTrack]);
            // Dynamically create a container in the form of a DIV element for playing the local video track.
            const localPlayerContainer = document.createElement("div");
            // Specify the ID of the DIV container. You can use the uid of the local user.
            localPlayerContainer.id = options.uid;
            localPlayerContainer.textContent = "Local user " + options.uid;
            localPlayerContainer.style.width = "640px";
            localPlayerContainer.style.height = "480px";
            document.body.append(localPlayerContainer);

            // Play the local video track.
            // Pass the DIV container and the SDK dynamically creates a player in the container for playing the local video track.
            rtc.localVideoTrack.play(localPlayerContainer);
            console.log("publish success!");
        }

        document.getElementById("leave").onclick = async function () {
            // Destroy the local audio and video tracks.
            rtc.localAudioTrack.close();
            rtc.localVideoTrack.close();

            // Traverse all remote users.
            rtc.client.remoteUsers.forEach(user => {
                // Destroy the dynamically created DIV containers.
                const playerContainer = document.getElementById(user.uid);
                playerContainer && playerContainer.remove();
            });

            // Leave the channel.
            await rtc.client.leave();
        }
    }
}

startBasicCall()
```

## Test your app {#test-your-app}

This quickstart uses [webpack](https://webpack.js.org/) to package the project and `webpack-dev-server` to run your project.

1.  In `package.json`, add `webpack`, `webpack-cli`, and `webpack-dev-server` to the `dependencies` field, and the `build` and `start:dev` commands to the `scripts` field.

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

2.  Create a file named `webpack.config.js` in the project directory to configure webpack, with the following code:

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

3.  To install dependencies, run the following command:

    ```shell
    npm install
    ```

4.  To build and run the project using webpack, run the following command:

    ```shell
    # Use webpack to package the project
    npm run build
    ```

5.  Use webpack-dev-server to run the project:

    ```shell
    npm run start:dev
    ```


A local web server automatically opens in your browser. You see the following page:

![](https://web-cdn.agora.io/docs-files/1619428543085)

Click **JOIN** to start a video call. However, being in a video call on your own is no fun. Ask a friend to join the same video call with you on the [Basic-Video-Call](https://webdemo.agora.io/basicVideoCall/index.html).

**Note:** 3.  Running the web app through a local server \(localhost\) is for testing purposes only. In production, ensure that you use the HTTPS protocol when deploying your project.
4.  Due to security limits on HTTP addresses except 127.0.0.1, the Web SDK only supports HTTPS or http://localhost \(http://127.0.0.1\). If you deploy your project over HTTP, you can only visit your project at http://localhost \(http://127.0.0.1\).


## Next steps {#next-steps}

Generating a token by hand is not helpful in a production context. [Authenticate Your Users with Tokens](https://docs.agora.io/en/Video/token_server?platform=Android) shows you how to start the video call with a token that you retrieve from your server.

## See also {#see-also}

-   Agora also provides an open-source [sample project](https://github.com/AgoraIO/API-Examples-Web) on GitHub for your reference.

-   In addition to integrating the Agora Web SDK into your project through npm, you can also choose either of the following methods to integrate the Agora Web SDK into your project:

    -   Through the CDN: Add the following code to the line before `<style>` in the `index.html` file of your project.

        ```html
        <script src="https://download.agora.io/sdk/release/AgoraRTC_N-4.7.1.js"></script>
        ```

    -   Manually download the latest [Agora Web SDK 4.x](https://docs.agora.io/en/Video/downloads?platform=Web), copy the `.js` file to the directory of your project files, and add the following code to the line before the `<style>` tag in your project.

        ```html
        <script src="./AgoraRTC_N-4.7.1.js"></script>
        ```

        **Note:** If you use the above methods, the SDK fully exports an `AgoraRTC` object. You can visit the `AgoraRTC` object to operate the Web SDK.


