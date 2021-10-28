# Initialize the Agora engine

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