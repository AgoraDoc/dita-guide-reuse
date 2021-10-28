# Get Started with Interactive Live Streaming Premium for Cocos Creator {#get-started-with-product-name-for-platform}

The Agora Video SDK enables you to develop rapidly to enhance your social, work, education, and IoT apps with real-time engagements.

This page shows the minimum code you need to add live streaming into your app by using the Agora Video SDK for Cocos Creator.

## Understand the tech {#understand-the-tech}

The following figure shows the workflow of a live streaming implemented by the Agora SDK.

![Basic workflow](https://web-cdn.agora.io/docs-files/1625465916613)

To start a live streaming, you implement the following steps in your app:

1.  Set the client role
2.  Retrieve a token
3.  Join a channel
4.  Publish and subscribe to audio and video in the channel.

For an app client to join a channel, you need the following information:

-   The App ID: A randomly generated string provided by Agora for identifying your app. You can get the App ID from [Agora Console](https://console.agora.io/).

-   The user ID: The unique identifier of a user. You need to specify the user ID yourself, and ensure that it is unique in the channel.

-   A token: In a test or production environment, your app client retrieves tokens from your server. For rapid testing, you can use a temporary token with a validity period of 24 hours.

-   The channel name: A string that identifies the channel for the live streaming.


## Project setup {#project-setup}

Use the following steps to build a project from scratch:

1.  Open Cocos Creator, and navigate to New Project.

2.  Choose an example \(**Hello World** shown here\), choose a storage path, and enter a project name.

3.  Click **Create**.

4.  Integrate the Agora Video SDK.

    1.  Open a Cocos Creator project, and click **Service** on the right to open the **Service** panel.

    2.  Click the ![](https://web-cdn.agora.io/docs-files/1603983326448) button following **AppID**, and click **Set Cocos AppID**.

        ![](https://web-cdn.agora.io/docs-files/1603983352672)

    3.  Click the drop-down box, select a game, and click **Association** to associate your project with your game.

    4.  On the **Service** panel, scroll down to find and click **Agora RTC**.

    5.  Click the ![](https://web-cdn.agora.io/docs-files/1603983397604) button following **Agora RTC**, read the prompts carefully, and click **Open Service**.

    6.  On the bottom of the **Agora RTC** panel, select **Video** as the SDK type, and click **Save** to integrate the Agora Cocos Creator SDK. Then, in **Node Library** \> **Cloud Nodes**, you can see **AgoraVideoRender**.


After integrating the Agora Cocos Creator SDK, Cocos Creator automatically adds codes in your project for obtaining device permissions, so you do not need to write these codes yourself.

Cocos Creator also uses your Cocos Creator account to automatically register an Agora account and create an Agora project. You can click **Dashboard** on the **Agora RTC** panel to visit Agora Console.

## Implement a client for Interactive Live Streaming Premium {#implement-a-client-for-product-name}

Once the project is set up, create a component script and a scene based on your requirements, and add the script into the scene node. See [Creating and using component script](https://docs.cocos.com/creator/manual/en/scripting/use-component.html).

You can use core APIs of the Agora Cocos Creator SDK in the `Script/HelloWorld.js` component script to implement the basic live streaming function. The following figure shows the API call sequence:

![](https://web-cdn.agora.io/docs-files/1606314149803)

### Initialize the Agora engine {#initialize-the-agora-engine}

Initialize the Agora engine before calling any other Agora APIs. You need to pass in the Agora App ID obtained from the Agora Console.

Call `init` and pass in the App ID to initialize the Agora engine.

According to your requirements, you can also listen for callback events in this step, such as when the local user joins or leaves the channel.

```typescript
cc.Class({

    ...

    onLoad: function () {
        // Initialize the Agora engine. Replace YOUR_APPID with the App ID of your Agora project.
        agora.init('YOUR_APPID');
        agora.on('joinChannelSuccess', this.onJoinChannelSuccess, this);
        agora.on('leaveChannel', this.onLeaveChannel, this);
        agora.on('userJoined', this.onUserJoined, this);
        agora.on('userOffline', this.onUserOffline, this);
    },

    ...
});

```

### Set the channel profile {#set-the-channel-profile}

After initializing the Agora engine, call `setChannelProfile` to set the channel profile as `LIVE_BROADCASTING`. If you want to switch to another profile, destroy the current Agora engine with the **release** method, and reinitialize the Agora engine before calling `setChannelProfile`.

```
    cc.Class({
    ...

    onLoad: function () {

    ...

    // Set the channel profile as interactive live streaming.
    agora.setChannelProfile(agora.CHANNEL_PROFILE_TYPE.CHANNEL_PROFILE_LIVE_BROADCASTING);
    },
    ...
    });
```

### Set the client role {#set-the-client-role}

An interactive streaming channel has two client roles: BROADCASTER and AUDIENCE; the default role is AUDIENCE.

After setting the channel profile to `LIVE_BROADCASTING`, your app may use the following steps to set the client role:

1.  In Cocos Creator, navigate to **Node Library** \> **Builtin Nodes**, and add **Toggle Group** to the **Scene** panel. You can adjust the display position of toggle components as needed.

2.  In the **Node Tree** panel, you can see that the **toggleContainer** contains three toggle components. You need to delete one redundant toggle, for example, **toggle3**.

    > In this section, designate **toggle1** as the option for audience, and **toggle2** as the option for host.

3.  In `HelloWorld.js`, call `setClientRole`, and pass in the client role set by the user.

    ```
    cc.Class({
    
         ...
    
         setClientRole: function (toggle, customEventData) {
             // Set the client role. A user chooses a client role and the toggle component passes it through customEventData.
             // If the customEventData value is 1, the client role is set to audience, and the corresponding enumerator is agora
             CLIENT_ROLE_TYPE.CLIENT_ROLE_AUDIENCE.
             // If the customEventData value is 2, the client role is set to host, and the corresponding enumerator is agora.CLIENT_ROLE_TYPE
             CLIENT_ROLE_BROADCASTER.
             agora.setClientRole(customEventData);
         },
    
         ...
     });
    ```

4.  Return to Cocos Creator, and click **toggle1**. In the **Properties** panel, find **Toggle** \> **Check Events**, and add a check event.

5.  On the **Node Tree** panel, find and add the **Canvas** to the **cc.Node** property box of the check event. In the two drop-down boxes following the **cc.Node** property box, select the scene \(**HelloWorld**\) and the check event \(**setClientRole**\) in turn, and enter **1** in **CustomEventData**.

6.  Click **Add Component** \> **Renderer Component** \> **Label** to add a label for **toggle1**. navigate to **Label** \> **String**, and enter **Audience**.

7.  Repeat step 3 step 4, and step 5 to configure the **toggle2**. Note that you need to enter **2** in **CustomEventData** and **Host** in **Label** \> **String**.


During the live streaming, only the host can be seen. If you want to switch the client role after joining the channel, call the `setClientRole` method.

### Set the local video view {#set-the-local-video-view}

To make the user see the local video before starting a video call, you need to set the local video view before joining a channel. Use the following steps:

1.  In `HelloWorld.js`, call `enableVideo` to enable the video module and `startPreview` to start the local video preview.

    ```typescript
    cc.Class({
    
        ...
    
        onLoad: function () {
    
            ...
    
            // Enable the video module.
            agora.enableVideo();
            // Start the local video preview.
            agora.startPreview();
        },
    
        ...
    });
    ```

2.  In Cocos Creator, navigate to **Node Library** \> **Cloud Nodes**, and add **AgoraVideoRender** to the **Scene**. Once added successfully, in the **assets** folder of the **Assets** panel, you can see **cloud-component** folder which contains `AgoraVideoRender.js` and `AgoraVideoRender.prefab` files.


### Join a channel {#join-a-channel}

After setting the local video view, do the following to join a channel:

1.  In Cocos Creator, add a button for joining or leaving a channel. navigate to **Node Library** \> **Builtin Nodes**, and add a **Button** component to the **Scene**.

2.  On the **Node Tree** panel, click **button** \> **Background** \> **Label**. On the **Properties** panel, find **Label** \> **String**, and modify the string to **Join a channel**.

3.  In `HelloWorld.js`, define a `clickButton` method to set the events when clicking the button.

    ```typescript
    cc.Class({
    
        ...
    
        // Set the events when clicking the button.
        // If the user joins a channel, the joined variable is true, and the SDK calls leaveChannel when the user clicks the button.
        // If the user leaves a channel, the joined variable is false, and the SDK calls joinChannel when the user clicks the button.
        clickButton: function () {
            if (this.joined) {
                this.leaveChannel();
            } else {
                this.joinChannel();
                }
        },
    
        ...
    });
    ```

4.  Call `joinChannel`, and enter a token, a channel name, and a user ID.

    ```typescript
    cc.Class({
    
        ...
    
        joinChannel: function () {
            // Join a channel.
            // Replace YOUR_TOKEN with the token you generated.
            // Replace CHANNEL_NAME with the channel name that you use to generate the token.
            // Enter a user ID in uid.
            agora.joinChannel('YOUR_TOKEN', 'CHANNEL_NAME', '', uid);
        },
    
        ...
    });
    ```

5.  In the `onJoinChannelSuccess` callback, add the logic to determine whether the local user joins the channel to dynamically change the label of the **button** component. Note that you need to remove the original definition of the label in `HelloWorld.js` as follows:

    -   In `Properties`, remove `text: 'Hello, World!'`.

    -   In `onLoad`, remove `this.label.string = this.text;`.

    ```typescript
    cc.Class({
    
       ...
    
       // Occur when a local user joins a channel.
       onJoinChannelSuccess: function (channel, uid, elapsed) {
           // After the user leaves the channel, the label of the button changes to Leave the channel.
           this.joined = true;
           this.label.string = 'Leave the channel';
       },
    
       ...
    });
    ```

6.  Return to **Cocos Creator**, and click **Canvas** on the **Node Tree** panel. On the **Properties** panel, find the **HelloWorld** component script, and add the **Label** of the **button** component to the script to dynamically modify the label of the button.

    ![](https://web-cdn.agora.io/docs-files/1634869842263)

7.  On the **Node Tree** panel, click **button**, find **Button** \> **Check Events** on the **Properties** panel, and add a check event.

8.  Add the **Canvas** to the **cc.Node** property box of the check event. In the two drop-down boxes following the **cc.Node** property box, select the scene \(**HelloWorld**\) and the check event \(**clickButton**\) in turn.


### Set the remote video view {#set-the-remote-video-view}

During the interactive video streaming, you should be able to see all the hosts, regardless of your role.

Shortly after a remote user joins the channel, the SDK gets the user's user ID in the `onUserJoined` callback. You can set the remote video view as follows:

1.  In `HelloWorld.js`, import the `AgoraVideoRender` cloud component to render the remote video view. In the `onUserJoined` and `onUserOffline` callbacks, add logic to determine whether Cocos Creator renders the remote video.

    ```typescript
    // Import the AgoraVideoRender cloud component.
    const AgoraVideoRender = require('AgoraVideoRender');
    
    cc.Class({
    
        ...
    
        properties: {
    
            ...
    
            // Add a remote video view.
            target: {
                default: null,
                type: cc.Prefab
            }
        },
    
        ...
    
        // Occur when a remote user joins the channel. This callback gets the user ID of the remote user.
        onUserJoined: function (uid, elapsed) {
            if (this.remoteUid === undefined) {
                this.remoteUid = uid;
                // Add a Prefab.
                const render = cc.instantiate(this.target);
                // Set the uid of the AgoraVideoRender component to the uid of the remote user.
                render.getComponent(AgoraVideoRender).uid = this.remoteUid;
                // Add AgoraVideoRender component to the Scene.
                cc.director.getScene().addChild(render, 0, `uid:${this.remoteUid}`);
                // Set the size of the remote video view.
                render.setContentSize(160, 120);
                // Set the position of the remote video view.
                render.setPosition(300, 120);
            }
        },
    
        // Occur when a remote user leaves the channel.
        onUserOffline: function (uid, reason) {
            if (this.remoteUid === uid) {
                // When a remote user leaves the channel, set the uid to undefined.
                cc.director.getScene().getChildByName(`uid:${this.remoteUid}`).destroy();
                this.remoteUid = undefined;
            }
        },
    
        ...
    });
    ```

2.  In Cocos Creator, click **Canvas** on the **Node Tree** panel. On the **Assets** panel, find **assets** \> **cloud-component** \> **AgoraVideoRender** \> **AgoraVideoRender.prefab**, and add it to **HelloWorld** \> **Target** of **Canvas**.

    ![](https://web-cdn.agora.io/docs-files/1634869889573)


### Leave the channel {#leave-the-channel}

Call the `leaveChannel` method to leave the current channel according to your scenario, for example, when the Interactive Live Streaming Premium ends, when you need to close the app, or when your app runs in the background.

After leaving the channel, if you want to exit the app or release the memory of the Agora engine, call `release` to destroy the Agora engine.

```typescript
cc.Class({

...

leaveChannel: function () {
    // Leaves the channel.
    agora.leaveChannel();
    if (this.remoteUid !== undefined) {
        // Remove the remote video view.
        cc.director.getScene().getChildByName(`uid:${this.remoteUid}`).destroy();
        this.remoteUid = undefined;
    }
    // After the local user leaves the channel, the label of the button changes to Join a channel.
    this.joined = false;
    this.label.string = 'Join a channel';
    // Destroy the Agora engine.
    agora.release();
},

// Occur when the local user leaves the channel.
onLeaveChannel: function (stats) {}

...
});
```

## Test your app {#test-your-app}

Agora recommends running and debugging your project through a physical Android or iOS device. See [Publish to native](https://docs.cocos.com/creator/manual/en/publish/publish-native.html).

For example, to build and run an iOS project, you can do the following:

1.  In Cocos Creator, click **Project** \> **Build**.

2.  Enter the title \(such as hello\_world\), the platform, and the build path, and click **Build**.

3.  Once the build is completed, click the **Open** button at the right of the Build Path to open the build folder.

4.  Under the `./jsb-link/frameworks/runtime-src/proj.ios_mac` path, find and open your iOS project \(such as `hello_world.xcworkspace`\).

5.  In Xcode, select your iOS device, and click the button to build and run your project. After the project runs successfully run the project, you can see your video.

6.  If you want to experience a Interactive Live Streaming Premium, you can also use the Agora Web sample app to interact with your iOS device. Ensure that you enter the same App ID, channel name, and temporary token in the Agora Web sample app as in your iOS project.




When the interactive live video streaming starts successfully, hosts can see their own videos, and audience members can see the video of hosts in the app.

## Next steps {#next-steps}

Generating a token by hand is not helpful in a production context. [Authenticate Your Users with Tokens](https://docs.agora.io/en/Video/token_server?platform=Android) shows you how to start the live streaming with a token that you retrieve from your server.

## See also {#see-also}

Agora provides an open-source [Agora-Cocos-Quickstart/CocosCreator](https://github.com/AgoraIO-Community/Agora-Cocos-Quickstart/tree/master/CocosCreator) sample project on GitHub that implements a basic one-to-one video call. You can download the demo project as a code source reference.

