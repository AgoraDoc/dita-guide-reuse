# Get Started with Video Call for React Native {#get-started-with-product-name-for-platform}

The Agora Video SDK enables you to develop rapidly to enhance your social, work, education, and IoT apps with real-time engagements.

This page shows the minimum code you need to add video call into your app by using the Agora Video SDK for React Native.

## Understand the tech {#understand-the-tech}

The following figure shows the workflow of a video call implemented by the Agora SDK.

![Basic workflow](https://web-cdn.agora.io/docs-files/1627550978702)

To start a video call, you implement the following steps in your app:

1.  Retrieve a token
2.  Join a channel
3.  Publish and subscribe to in the channel

For an app client to join a channel, you need the following information:

-   The App ID: A randomly generated string provided by Agora for identifying your app. You can get the App ID from [Agora Console](https://console.agora.io/).

-   The user ID: The unique identifier of a user. You need to specify the user ID yourself, and ensure that it is unique in the channel.

-   A token: In a test or production environment, your app client retrieves tokens from your server. For rapid testing, you can use a temporary token with a validity period of 24 hours.

-   The channel name: A string that identifies the channel for the video call.


## Prerequisites {#prerequisites}

Before proceeding, ensure that your development environment meets all the requirements necessary.

### Development environment {#development-environment}

If your target platform is iOS, your development environment must meet the following requirements:

-   React Native 0.59.10 or later

-   macOS

-   Node 10 or later

-   Xcode 9.4 or later

-   CocoaPods

-   A physical or virtual mobile device running iOS 8.0 or later


If your target platform is Android, your development environment must meet the following requirements:

-   React Native 0.59.10 or later

-   macOS, Windows, or Linux

-   Node 10 or later

-   Java Development Kit \(JDK\) 8 or later

-   Android Studio \(latest version recommended\)

-   A physical or virtual mobile device running Android 5.0 or later


For more information, see [Setting up the development environment](https://reactnative.dev/docs/environment-setup).

### Other prerequisites {#other-prerequisites}

-   A valid [Agora account](https://console.agora.io/).
-   An active Agora project with an App ID and a temporary token. For details, see [Get Started with Agora](https://docs.agora.io/en/Agora%20Platform/get_appid_token).
-   A computer with access to the internet. If your network has a firewall, follow the instructions in [Firewall Requirements](https://docs.agora.io/en/Agora%20Platform/firewall).

## Project setup {#project-setup}

Follow the steps to create the environment necessary to add video call into your app.

### Create a React Native project {#create-a-react-native-project}

1.  Make sure you have set up the development environment based on your operating system and target platform.

2.  Run the following command and fill in your project name in `ProjectName` to create and initialize a new React Native project:

    ```shell
    npx react-native init ProjectName
    ```

    A successful execution of this command generates a simple sample project in the directory that you run the command.

3.  Launch your Android or iOS simulator and run your project by executing the following command:

    a. Run `npx react-native start` in the root of your project to start Metro.

    b. Open another terminal in the root of your project and run `npx react-native run-android` to start the Android app, or run `npx react-native run-ios` to start the iOS app.

    If everything is set up correctly, you should see your new app running in your Android or iOS simulator shortly.

    You can also run your project on a physical Android or iOS device. For detailed instructions, see [Running on device](https://reactnative.dev/docs/running-on-device).


Now that you have your project running successfully, you can start trying to integrate the Agora React Native SDK and modify your project.

### Integrate the SDK {#integrate}

This section describes how to integrate the SDK on React Native 0.60.0 or later.

If you use React Native 0.59.x, [Installing \(React Native == 0.59.x](https://github.com/AgoraIO-Community/react-native-agora/blob/master/README.md#installing-react-native--059x) to integrate the Agora Video SDK for React Native.

1.  Install the latest version of the Agora Video SDK for React Native using npm:

    ```shell
    npm i --save react-native-agora
    ```

    Do not link native modules manually, as React Native 0.60.0 and later support automatically linking native modules. For details, see [Autolinking](https://github.com/react-native-community/cli/blob/master/docs/autolinking.md).

2.  If your target platform is iOS, you also need to run the following command:

    ```shell
    npx pod-install
    ```

3.  The Agora Video SDK for React Native uses Swift in native modules. Therefore, your project must support compiling Swift; Otherwise, you get errors when building the iOS app. Do the following:

    a. Open `ios/ProjectName.xcworkspace` with Xcode.

    b. Click **File \> New \> File**, select **iOS \> Swift File**, and then click **Next \> Create** to create a new `File.swift` file.


### Add TypeScript {#add-typescript}

The sample code in this article is written in TypeScript. If you want to use this sample code directly, you need to add support for TypeScript to your project.

1.  Run one of the following commands in the root of your project to add TypeScript dependencies:

    **Method one**: Using npm

    ```shell
    npm install --save-dev typescript @types/jest @types/react @types/react-native @types/react-test-renderer
    ```

    **Method two**: Using yarn

    ```shell
    yarn add --dev typescript @types/jest @types/react @types/react-native @types/react-test-renderer
    ```

2.  Create a `tsconfig.json` file in the root of your project, and copy the following code to the file:

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

3.  Create a `jest.config.js` file in the root of your project, and copy the following code to the file:

    ```javascript
    module.exports = {
         preset: 'react-native',
         moduleFileExtensions: ['ts', 'tsx', 'js', 'jsx', 'json', 'node']
    };
    ```

4.  Rename `App.js` to `App.tsx`.


For more information, see [Using TypeScript](https://reactnative.dev/docs/typescript).

## Implement a client for Video Call {#implement-a-client-for-product-name}

This section shows how to use the Agora Video SDK to implement the video call into your app step by step.

### 1. Create the UI {#1-create-the-ui}

Create the user interface \(UI\) for Video Call in the layout file of your project.

For a video call, we recommend adding the following elements into the UI:

-   The start-call button

-   The end-call button

-   The local video view

-   The remote video view


The `components/Style.ts` file in the [Agora-RN-Quickstart](https://github.com/AgoraIO-Community/Agora-RN-Quickstart) sample project defines the styles of the UI elements. You can create a `components` folder for your project, add a `Style.ts` file under that folder, and then copy the following code to the file:

```typescript
import {Dimensions, StyleSheet} from 'react-native'

const dimensions = {
    width: Dimensions.get('window').width,
    height: Dimensions.get('window').height,
}

export default StyleSheet.create({
    max: {
        flex: 1,
    },
    buttonHolder: {
        height: 100,
        alignItems: 'center',
        flex: 1,
        flexDirection: 'row',
        justifyContent: 'space-evenly',
    },
    button: {
        paddingHorizontal: 20,
        paddingVertical: 10,
        backgroundColor: '#0093E9',
        borderRadius: 25,
    },
    buttonText: {
        color: '#fff',
    },
    fullView: {
        width: dimensions.width,
        height: dimensions.height - 100,
    },
    remoteContainer: {
        width: '100%',
        height: 150,
        position: 'absolute',
        top: 5
    },
    remote: {
        width: 150,
        height: 150,
        marginHorizontal: 2.5
    },
    noUserText: {
        paddingHorizontal: 10,
        paddingVertical: 5,
        color: '#0093E9',
    },
})
```

### 2. Import classes {#2-import-classes}

Open the `App.tsx` file, and delete all code. Add the following code to the beginning of the `App.tsx` file:

```typescript
import React, {Component} from 'react'
import {Platform, ScrollView, Text, TouchableOpacity, View, PermissionsAndroid} from 'react-native'
// Import the RtcEngine class and view rendering components into your project.
import RtcEngine, {RtcLocalView, RtcRemoteView, VideoRenderMode} from 'react-native-agora'
// Import the UI styles.
import styles from './components/Style'
```

### 3. Add project permissions {#3-add-project-permissions}

**Android**

Add the following code to the `App.tsx` file to set a prompt box for getting permissions to use the microphone and camera on an Android device:

```typescript
const requestCameraAndAudioPermission = async () =>{
    try {
        const granted = await PermissionsAndroid.requestMultiple([
            PermissionsAndroid.PERMISSIONS.CAMERA,
            PermissionsAndroid.PERMISSIONS.RECORD_AUDIO,
        ])
        if (
            granted['android.permission.RECORD_AUDIO'] === PermissionsAndroid.RESULTS.GRANTED
            && granted['android.permission.CAMERA'] === PermissionsAndroid.RESULTS.GRANTED
        ) {
            console.log('You can use the cameras & mic')
        } else {
            console.log('Permission denied')
        }
    } catch (err) {
        console.warn(err)
    }
}
```

**iOS**

In Xcode, open the `info.plist` file. Add the following contents to add permissions for your device:

|Key|Type|Value|
|---|----|-----|
|Privacy - Microphone Usage Description|String|The purpose for accessing the microphone, such as for a call or interactive live streaming.|
|Privacy - Camera Usage Description|String|The purpose for accessing the camera, such as for a call or interactive live streaming.|

### 4. Create an App component {#4-create-an-app-component}

When creating an App component, you need to define your App ID, token, and channel name, which will be used for initializing the `RtcEngine` object and joining the channel. Ensure the App ID and channel name are the same as the App ID and channel name used for generating the token.

```typescript
// Define a Props interface.
interface Props {
}

// Define a State interface.
interface State {
    appId: string,
    channelName: string,
    token: string,
    joinSucceed: boolean,
    peerIds: number[],
}

// Create an App component, which extends the properties of the Pros and State interfaces.
export default class App extends Component<Props, State> {
    _engine?: RtcEngine
    // Add a constructor，and initialize this.state. You need:
    // Replace yourAppId with the App ID of your Agora project.
    // Replace yourChannel with the channel name that you want to join.
    // Replace yourToken with the token that you generated using the App ID and channel name above.
    constructor(props) {
        super(props)
        this.state = {
            appId: 'yourAppId',
            channelName: 'yourChannel',
            token: 'yourToken',
            joinSucceed: false,
            peerIds: [],
        }
        if (Platform.OS === 'android') {
            requestCameraAndAudioPermission().then(() => {
                console.log('requested!')
            })
        }
    }
    // Other code. See step 5 to step 10.
    ...
}
```

### 5. Initialize RtcEngine {#5-initialize-rtcengine}

Create and initialize the `RtcEngine` object before calling any other Agora APIs.

You can also listen for callback events, such as when the local user joins the channel, when a remote user joins the channel, and when a remote user leaves the channel.

1.  Allow the user to set the role as `Broadcaster` or `Audience`.
2.  Call `setClientRole` and pass in the client role set by the user.

Add the following code after `// Other code` in `App.tsx`：

```language-typescript
// Typescript
// Mount the App component into the DOM.
componentDidMount() {
    this.init()
}
// Pass in your App ID through this.state, create and initialize an RtcEngine object.
init = async () => {
    const {appId} = this.state
    this._engine = await RtcEngine.create(appId)
    // Enable the video module.
    await this._engine.enableVideo()

    // Listen for the UserJoined callback.
    // This callback occurs when the remote user successfully joins the channel.
    this._engine.addListener('UserJoined', (uid, elapsed) => {
        console.log('UserJoined', uid, elapsed)
        const {peerIds} = this.state
        if (peerIds.indexOf(uid) === -1) {
            this.setState({
                peerIds: [...peerIds, uid]
            })
        }
    })

    // Listen for the UserOffline callback.
    // This callback occurs when the remote user leaves the channel or drops offline.
    this._engine.addListener('UserOffline', (uid, reason) => {
        console.log('UserOffline', uid, reason)
        const {peerIds} = this.state
        this.setState({
            // Remove peer ID from state array
            peerIds: peerIds.filter(id => id !== uid)
        })
    })

    // Listen for the JoinChannelSuccess callback.
    // This callback occurs when the local user successfully joins the channel.
    this._engine.addListener('JoinChannelSuccess', (channel, uid, elapsed) => {
        console.log('JoinChannelSuccess', channel, uid, elapsed)
        this.setState({
            joinSucceed: true
        })
    })
}
   
```

### 6. Join a channel {#6-join-a-channel}

After initializing the `RtcEngine` object, you can call `joinChannel` to join a channel.

```typescript
// Pass in your token and channel name through this.state.token and this.state.channelName.
// Set the ID of the local user, which is an integer and should be unique. If you set uid as 0,
// the SDK assigns a user ID for the local user and returns it in the JoinChannelSuccess callback.
startCall = async () => {
        await this._engine?.joinChannel(this.state.token, this.state.channelName, null, 0)
    }
```

### 7. Render UI elements {#7-render-ui-elements}

Call the `render()` method in the App component to render the UI elements and handle the button click event.

```typescript
render() {
        return (
            <View style={styles.max}>
                <View style={styles.max}>
                    <View style={styles.buttonHolder}>
                        <TouchableOpacity
                            onPress={this.startCall}
                            style={styles.button}>
                            <Text style={styles.buttonText}> Start Call </Text>
                        </TouchableOpacity>
                        <TouchableOpacity
                            onPress={this.endCall}
                            style={styles.button}>
                            <Text style={styles.buttonText}> End Call </Text>
                        </TouchableOpacity>
                    </View>
                    {this._renderVideos()}
                </View>
            </View>
        )
    }
```

### 8. Render the local video view {#8-render-the-local-video-view}

Configure the video view of the local user. After joining the channel, the user can see themselves. You can also call `startPreview` to start the local video preview before joining the channel.

```typescript
// Set the rendering mode of the video view as Hidden,
// which uniformly scales the video until it fills the visible boundaries.
_renderVideos = () => {
        const {joinSucceed} = this.state
        return joinSucceed ? (
            <View style={styles.fullView}>
                <RtcLocalView.SurfaceView
                    style={styles.max}
                    channelId={this.state.channelName}
                    renderMode={VideoRenderMode.Hidden}/>
                {this._renderRemoteVideos()}
            </View>
        ) : null
    }
```

### 9. Render the remote video view {#9-render-the-remote-video-view}

In a video call, you should be able to see other users. After joining the channel, pass in the `uid` of the remote user sending the video, and set the video view of the remote user.

```typescript
// Set the rendering mode of the video view as Hidden,
// which uniformly scales the video until it fills the visible boundaries.
_renderRemoteVideos = () => {
        const {peerIds} = this.state
        return (
            <ScrollView
                style={styles.remoteContainer}
                contentContainerStyle={{paddingHorizontal: 2.5}}
                horizontal={true}>
                {peerIds.map((value, index, array) => {
                    return (
                        <RtcRemoteView.SurfaceView
                            style={styles.remote}
                            uid={value}
                            channelId={this.state.channelName}
                            renderMode={VideoRenderMode.Hidden}
                            zOrderMediaOverlay={true}/>
                    )
                })}
            </ScrollView>
        )
    }
```

### 10. Leave the channel {#10-leave-the-channel}

Call `leaveChannel` to leave the current channel according to your scenario. For example, when the video call ends, when you need to close the app, or when your app runs in the background.

```typescript
endCall = async () => {
        await this._engine?.leaveChannel()
        this.setState({peerIds: [], joinSucceed: false})
    }
```

## Test your app {#test-your-app}

Before testing your app, you need to install and run your project on a device first.Agora recommends you run this project on a physical mobile device, as some simulators may not support the full features of this project.

**To run the Android app on a physical device:**

1.  Enable **Developer options** on your Android device, and then connect it to your computer using a USB cable.

2.  Run `npx react-native run-android` in the project root directory.


**To run the iOS app on a physical device:**

1.  Open the `ProjectName/ios/ProjectName.xcworkspace` folder with Xcode.

2.  Connect your iOS device to your computer using a USB cable.

3.  Click the **Build and run** button in Xcode.


A moment later you will see the project installed on your device. Then, take the following steps to test the video call app:

1.  Grant microphone and camera access to your app.

2.  When the app launches, you should be able to see yourself on the local view.

3.  Ask a friend to join the video call with you on the [demo app](https://webdemo.agora.io/basicVideoCall/index.html). Enter the same App ID and channel name.

4.  After your friend joins the channel, you should be able to see and hear each other.


## Next steps {#next-steps}

Generating a token by hand is not helpful in a production context. [Authenticate Your Users with Tokens](https://docs.agora.io/en/Video/token_server?platform=Android) shows you how to start video calling with a token that you retrieve from your server.

## See also {#see-also}

This section provides additional information for your reference.

### Sample project {#sample-project}

Agora provides an open source sample project [Agora-RN-Quickstart](https://github.com/AgoraIO-Community/Agora-RN-Quickstart) on GitHub that implements one-to-one video call and group video call for your reference.

### Integrate the SDK using yarn {#integrate-the-sdk-using-yarn}

In addition to integrating the Agora Video SDK for React Native using npm, you can also import the SDK into your project using yarn.

1.  Install yarn.

    ```
    npm install -g yarn
    ```

2.  Download the SDK using yarn.

    ```
    yarn add react-native-agora
    ```


If your target platform is iOS, you need to do two more steps as described in [Integrate the SDK](get-started.md#) to install the SDK and add support for Swift.

