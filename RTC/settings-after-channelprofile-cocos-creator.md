# Set the local video view

To make the user see the local video before starting a video call, you need to set the local video view before joining a channel. Use the following steps:

1. In `HelloWorld.js`, call `enableVideo` to enable the video module and `startPreview` to start the local video preview.

   ``` typescript
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

2. In Cocos Creator, navigate to **Node Library** > **Cloud Nodes**, and add **AgoraVideoRender** to the **Scene**.
   Once added successfully, in the **assets** folder of the **Assets** panel, you can see **cloud-component** folder which contains `AgoraVideoRender.js` and `AgoraVideoRender.prefab` files.

# Join a channel

After setting the local video view, do the following to join a channel:

1. In Cocos Creator, add a button for joining or leaving a channel. navigate to **Node Library** > **Builtin Nodes**, and add a **Button** component to the **Scene**.

2. On the **Node Tree** panel, click **button** > **Background** > **Label**. On the **Properties** panel, find **Label** > **String**, and modify the string to **Join a channel**.

3. In `HelloWorld.js`, define a `clickButton` method to set the events when clicking the button.

   ``` typescript
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

4. Call `joinChannel`, and enter a token, a channel name, and a user ID.

   ``` typescript
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

5. In the `onJoinChannelSuccess` callback, add the logic to determine whether the local user joins the channel to dynamically change the label of the **button** component. Note that you need to remove the original definition of the label in `HelloWorld.js` as follows:

   * In `Properties`, remove `text: 'Hello, World!'`.
   * In `onLoad`, remove `this.label.string = this.text;`.

    ``` typescript
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

6. Return to **Cocos Creator**, and click **Canvas** on the **Node Tree** panel. On the **Properties** panel, find the **HelloWorld** component script, and add the **Label** of the **button** component to the script to dynamically modify the label of the button.
    
   <image href="https://web-cdn.agora.io/docs-files/1634869842263" format="png" scope="external"></image>

7. On the **Node Tree** panel, click **button**, find **Button** > **Check Events** on the **Properties** panel, and add a check event.

8. Add the **Canvas** to the **cc.Node** property box of the check event. In the two drop-down boxes following the **cc.Node** property box, select the scene (**HelloWorld**) and the check event (**clickButton**) in turn.

# Set the remote video view


<p props="video">During the video call, you should be able to see all users.</p>

<p props="live">During the interactive video streaming, you should be able to see all the hosts, regardless of your role.</p>


Shortly after a remote user joins the channel, the SDK gets the user's user ID in the `onUserJoined` callback. You can set the remote video view as follows:

1. In `HelloWorld.js`, import the `AgoraVideoRender` cloud component to render the remote video view. In the `onUserJoined` and `onUserOffline` callbacks, add logic to determine whether Cocos Creator renders the remote video.

   ``` typescript
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

2. In Cocos Creator, click **Canvas** on the **Node Tree** panel. On the **Assets** panel, find **assets** > **cloud-component** > **AgoraVideoRender** > **AgoraVideoRender.prefab**, and add it to **HelloWorld** > **Target** of **Canvas**.

   <image href="https://web-cdn.agora.io/docs-files/1634869889573" format="png" scope="external"></image>

# Leave the channel

Call the `leaveChannel` method to leave the current channel according to your scenario, for example, when the [product-name] ends, when you need to close the app, or when your app runs in the background.

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