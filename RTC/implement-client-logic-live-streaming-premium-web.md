# Implement the client logic

To implement the client logic of [feature], we need to do the following things:

1. Call [`createClient`](https://docs.agora.io/en/live-streaming/API%20Reference/web_ng/interfaces/iagorartc.html#createclient) to create an `AgoraRTCClient` object.
2. Call [`setClientRole`](https://docs.agora.io/en/live-streaming/API%20Reference/web_ng/interfaces/iagorartcclient.html#setclientrole) to set the client role.
3. Call [`join`](https://docs.agora.io/en/live-streaming/API%20Reference/web_ng/interfaces/iagorartcclient.html#join) to join an RTC channel with the App ID, user ID, token, and channel name. 
4. Call [`createMicrophoneAudioTrack`](https://docs.agora.io/en/Video/API%20Reference/web_ng/interfaces/iagorartc.html#createmicrophoneaudiotrack) to create a local audio track from the audio sampled by a microphone, call [`createCameraVideoTrack`](https://docs.agora.io/en/Video/API%20Reference/web_ng/interfaces/iagorartc.html#createcameravideotrack) to create a local video track from the video captured by a camera, and then call `publish` to publish the local audio and video tracks to the channel.
5. When a remote user joins the channel and publishes tracks:
   1. Listen for the [`client.on("user-published")`](https://docs.agora.io/en/live-streaming/API%20Reference/web_ng/interfaces/iagorartcclient.html#event_user_published) event. When the SDK triggers this event, get an `AgoraRTCRemoteUser` object from this event.
   2. Call [`subscribe`](https://docs.agora.io/en/live-streaming/API%20Reference/web_ng/interfaces/iagorartcclient.html#subscribe) to subscribe to the `AgoraRTCRemoteUser` object and get the `RemoteAudioTrack` and `RemoteVideoTrack` objects of the remote user.
   3. Call [`play`](https://docs.agora.io/en/live-streaming/API%20Reference/web_ng/interfaces/itrack.html#play) to play the remote tracks.

The following figure shows the API call sequence of the basic one-to-one [feature].

![start-api-sequence-web]

Copy the following code into `main.js` and then replace `appID` and `token` with [values from your Agora project](start-prerequisites-web.md).

```js
import AgoraRTC from "agora-rtc-sdk-ng"
 
let rtc = {
    // For the local audio and video tracks.
    localAudioTrack: null,
    localVideoTrack: null,
    client: null
};
 
let options = {
    // Pass your app ID here.
    appId: "your_app_id",
    // Set the channel name.
    channel: "test",
    // Use a temp token
    token: "your_temp_token",
    // Uid
    uid: 123456
    // Set the user role as "host" or "audience".
    role: "audience"    
};
 
async function startBasicLiveStreaming() {
 
    rtc.client = AgoraRTC.createClient({ mode: "live", codec: "vp8" });
 
    window.onload = function () {
 
        document.getElementById("join").onclick = async function () {
            rtc.client.setClientRole(options.role, clientRoleOptions);
            await rtc.client.join(options.appId, options.channel, options.token, options.uid);
        }
 
        document.getElementById("leave").onclick = async function () {
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
 
    rtc.client.on("user-published", async (user, mediaType) => {
        // Subscribe to a remote user.
        await rtc.client.subscribe(user, mediaType);
        console.log("subscribe success");
 
        // If the subscribed track is video.
        if (mediaType === "video") {
            // Get `RemoteVideoTrack` in the `user` object.
            const remoteVideoTrack = user.videoTrack;
            // Dynamically create a container in the form of a DIV element for playing the remote video track.
            const remotePlayerContainer = document.createElement("div");
            // Specify the ID of the DIV container. You can use the `uid` of the remote user.
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
 
        // If the subscribed track is audio.
        if (mediaType === "audio") {
            // Get `RemoteAudioTrack` in the `user` object.
            const remoteAudioTrack = user.audioTrack;
            // Play the audio track. No need to pass any DOM element.
            remoteAudioTrack.play();
        }
    });
 
    rtc.client.on("user-unpublished", user => {
        // Get the dynamically created DIV container.
        const remotePlayerContainer = document.getElementById(user.uid);
        // Destroy the container.
        remotePlayerContainer.remove();
    });
}
 
startBasicLiveStreaming()
```