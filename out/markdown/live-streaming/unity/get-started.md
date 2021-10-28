# Get Started with Interactive Live Streaming Premium for  {#get-started-with-product-name-for-platform}

The Agora Video SDK enables you to develop rapidly to enhance your social, work, education, and IoT apps with real-time engagements.

This page shows the minimum code you need to add live streaming into your app by using the Agora Video SDK for .

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


## Prerequisites {#prerequisites}

Before proceeding, ensure that your development environment meets the following requirements:

-   Unity 2017 or later


**Note:** Unity 2020.3 or later has compatibility issues on Mac devices. For macOS app development, Agora recommends not using Unity 2020.3 or later.

-   Operating system and IDE requirements:

    |Target Platform|Operating system version|IDE version|
    |---------------|------------------------|-----------|
    |Android|Android 4.1 or later|Android Studio 3.0 or later|
    |iOS|iOS 8.0 or later|Xcode 9.0 or later|
    |macOS|macOS 10.0 or later|Xcode 9.0 or later|
    |Windows|Windows 7 or later|Microsoft Visual Studio 2017 or later|

-   A valid [Agora account](https://docs.agora.io/en/Agora%20Platform/sign_in_and_sign_up) and an [App ID](https://docs.agora.io/en/Agora%20Platform/token?platform=All%20Platforms#getappid)


**Note:** Open the specified ports in [Firewall Requirements](https://docs.agora.io/en/Agora%20Platform/firewall?platform=All%20Platforms#agora-rtc-sdk) if your network has a firewall.

## Project setup {#project-setup}

Follow the steps to create the environment necessary to add live streaming into your app.

1.  Create a new Unity project. Ensure that you have downloaded and installed Unity before you follow the steps below. If not, click here to [download unity](https://store.unity.com/?_ga=2.210473373.969150697.1571977051-1808388573.1570601452#plans-individual).

2.  Open Unity and click **New**；fill in and select the options in the following fields, and click **Create**.

    -   Project name

    -   Location

    -   Template: Choose 3D

3.  Integrate the Agora Video SDK into your project. Go to Unity Asset Store, search for Agora Video SDK for Unity and click it. Then, click **Add to My Assets** and select **Open in Unity**. ![](https://web-cdn.agora.io/docs-files/1635315879257) ![](https://web-cdn.agora.io/docs-files/1635322487734)

4.  Click **Download** in the Package Manager that pops up to download the SDK. When the download completes, the button becomes **Import**. Click **Import** to show the packages of the downloaded SDK.


## Implement a client for Interactive Live Streaming Premium {#implement-a-client-for-product-name}

This section shows how to use the Agora Video SDK to implement live streaming into your app step by step.

### Create the UI {#create-the-ui}

Agora recommends adding the following elements to the UI:

1.  The view of the host
2.  An exit button


If you use the **Unity Editor** to build your UI, ensure that you bind **VideoSurface.cs** to the **GameObjects** designated for local and remote videos. In the following example, the **VideoSurface.cs** is applied to the cube for the local video and to the cylinder for the remote video.

![](https://web-cdn.agora.io/docs-files/1576216681040)

### Get the device permission \(Android only\) {#get-the-device-permission-android-only}

If you build for Android, you should add in APIs to check and request the device permission.

Unity versions later than **UNITY\_2018\_3\_OR\_NEWER** do not automatically request camera and microphone permissions from your Android device. If you are using one of these versions, call the `CheckPermission` method to request access to the camera and microphone of your Android device.

```c#
#if(UNITY_2018_3_OR_NEWER) 
using UnityEngine.Android; 
#endif 
void Start () 
{  
#if(UNITY_2018_3_OR_NEWER) 
permissionList.Add(Permission.Microphone);  
permissionList.Add(Permission.Camera);  
#endif  
} 
private void CheckPermission()
{ 
#if(UNITY_2018_3_OR_NEWER) 
foreach(string permission in permissionList) 
{ 
if (Permission.HasUserAuthorizedPermission(permission)) 
{ 
} 
else 
{  
Permission.RequestUserPermission(permission); 
} 
} 
#endif 
} 
void Update () 
{  
#if(UNITY_2018_3_O
R_NEWER) 
// Ask for your Android device's permissions.
CheckPermission(); 
#endif  
}
```

### Implement the Interactive Live Streaming Premium logic {#implement-the-product-name-logic}

The following figure shows the API call sequence of implementing the Interactive Live Streaming Premium:

![API seqeunce of live streaming](https://web-cdn.agora.io/docs-files/1576220788626)

To implement this logic, take the following steps:

**Initialize the IRtcEngine**

-   Call the `GetEngine` method and pass in the App ID to initialize the `IRtcEngine` object.

-   According to your scenarios, you can also listen for callback events, such as when the local user joins the channel, and when the first video frame of a remote user is decoded.

    ```c#
     // Pass an App ID to create and initialize an IRtcEngine object.
     mRtcEngine = IRtcEngine.GetEngine (appId); 
     // Listen for the OnJoinChannelSuccessHandler callback.
     // This callback occurs when the local user successfully joins the channel.
     mRtcEngine.OnJoinChannelSuccessHandler = OnJoinChannelSuccessHandler; 
     // Listen for the OnUserJoinedHandler callback.
     // This callback occurs when the first video frame of a remote user is received and decoded after the remote user successfully joins the channel.
     // You can call the SetForUser method in this callback to set the remote video.
     mRtcEngine.OnUserJoinedHandler = OnUserJoinedHandler; 
     // Listen for the OnUserOfflineHandler callback.
     // This callback occurs when the remote user leaves the channel or drops offline.
     mRtcEngine.OnUserOfflineHandler = OnUserOfflineHandler;   
    ```


**Set the channnel profile**

-   Call the `SetChannelProfile` method to set the channel profile as `LIVE_BROADCASTING`.

-   One `IRtcEngine` object uses one profile only. If you want to switch to another profile, destroy the current `IRtcEngine` object with the `Destroy` method and create a new one before calling the `SetChannelProfile` method.

    ```c#
    // Set the channel profile as LIVE_BROADCASTING.
    mRtcEngine.SetChannelProfile(CHANNEL_PROFILE.CHANNEL_PROFILE_LIVE_BROADCASTING);
    ```


**Set the client role**

An interactive streaming channel has two client roles: `BROADCASTER` and `AUDIENCE`, and the default role is `AUDIENCE`. After setting the channel profile to `LIVE_BROADCASTING`, you can call the `SetClientRole` method and pass in the client role set by the user.

Note that in an interactive live streaming, only the host can be heard and seen. If you want to switch the client role after joining the channel, call the `SetClientRole` method.

```c#
// Set the client role as the host.
mRtcEngine.SetClientRole(CLIENT_ROLE.BROADCASTER);
```

**Set the local video**

After initializing an `IRtcEngine object`, set the local video before joining a channel so that you can see yourself in the call. Follow these steps to configure the local video:

-   Call `EnableVideo` to enable the video module.

-   Call `EnableVideoObserver` to get the local video.

    ```
    ```c#
    // Enable the video module.
    mRtcEngine.EnableVideo();
    // Get the local video and pass it on to Unity.
    mRtcEngine.EnableVideoObserver();
    ```
    ```

    -   Choose an object to display the local video, and drag the **VideoSurface.cs** file to **Script**, so that you can bind the **VideoSurface.cs** file to the object and see the local video. Unity renders 3D objects by default, such as Cube, Cylinder and Plane. To render the object in other types, modify the renderer in the **VideoSurface.cs** file.

        ![](https://web-cdn.agora.io/docs-files/1576208681884)

        ```c#
        // The default renderer is Renderer.
        Renderer rend = GetComponent(); 
        rend.material.mainTexture = nativeTexture; 
        // Change the renderer to RawImage.
        RawImage rend = GetComponent(); 
        rend.texture = nativeTexture; 
        ```


**Join a channel**

Call `JoinChannelByKey` to join a channel. Set the following parameters when calling this method:

-   `channelKey`: The token for identifying the role and privileges of a user. Set it as one of the following values:
    -   A temporary token generated in Agora Console. A temporary token is valid for 24 hours.
    -   A token generated at the server. This applies to scenarios with high-security requirements.
-   `channelName`:The unique name of the channel to join. Users that input the same channel name join the same channel.
-   `uid`:Integer. The unique ID of the local user. If you set `uid` as 0, the SDK automatically assigns one user ID and returns it in the `OnJoinChannelSuccessHandler` callback.

```c#
// Join a channel. 
mRtcEngine.JoinChannelByKey(null, channel, null, 0);
```

**Set the remote video**

In a video call, you should be able to see remote users too. Call `SetForUser` in **VideoSurface.cs** after joining a channel. When a remote user joins the channel, the SDK triggers the `OnUserJoinedHandler` callback, and you can get the user’s uid.

Follow the steps to set the remote video:

-   In the **VideoSurface.cs** file, call the `SetForUser` method in the callback, and pass in the `uid` to set the video of the remote user.

-   Drag the **VideoSurface.cs** script to the target object, so that you can bind the **VideoSurface.cs** to the object and see the remote video.


**Note:** The default value of `uid` in the `SetForUser` method is 0. To see the remote user, ensure that you input the uid of the remote user in `SetForUser`. Otherwise, the SDK uses the default value and you can only see the local video.

```c#
// This callback occurs when the first video frame of a remote user is received and decoded after the remote user successfully joins the channel.
// You can call the SetForUser method in this callback to set up the remote video view.
private void OnUserJoinedHandler(uint uid, int elapsed)
    {
        Debug.Log ("OnUserJoinedHandler: uid = " + uid)
        GameObject go = GameObject.Find (uid.ToString ());
        if (!ReferenceEquals (go, null)) {
            return; 
        }
        go = GameObject.CreatePrimitive (PrimitiveType.Plane);
        if (!ReferenceEquals (go, null)) {
            go.name = uid.ToString ();
            VideoSurface remoteVideoSurface = go.AddComponent<VideoSurface> ();
          // Set the remote video.
          remoteVideoSurface.SetForUser (uid);
          // Adjust the video refreshing frame rate. The video capture and render frame rate of Agora Unity SDK is 15 fps by default.
          // For example, in the game scenario, the refreshing frame rate of the game is 60 fps, which causes 3 times redundancy compared to the video render frame rate. Call the SetGameFps to adjust the refreshing frame rate of the game to 15 fps.
          remoteVideoSurface.SetGameFps (60);
          remoteVideoSurface.SetEnable (true);
        }
        mRemotePeer = uid;
    }
```

To remove the VideoSurface.cs script from the object, see the following sample codes:

```c#
private void OnUserOfflineHandler(uint uid, USER_OFFLINE_REASON reason)
 {
     // Remove video stream.
     // Call this in the main thread.
     GameObject go = GameObject.Find (uid.ToString());
     if (!ReferenceEquals (go, null)) {
         Destroy (go);
     }
 }
```

**Leave the channel and destroy the `IRtcEngine` object**

-   According to your scenario, such as when the call ends and when you need to close the app, call `LeaveChannel` to leave the current call, and call `DisableVideoObserver` to disable the video.

    ```c#
    public void leave()
     {
         Debug.Log ("calling leave");
         if (mRtcEngine == null)
             return;
         // Leave the channel.
         mRtcEngine.LeaveChannel();
         // Disable the video.
         mRtcEngine.DisableVideoObserver();
     }
    ```

    -   After leaving the channel, if you want to exit the app or release the memory of `IRtcEngine`, call `Destroy` to destroy the `IRtcEngine` object.

        ```c#
        void OnApplicationQuit()
        {
            if (mRtcEngine != null)
            {
            // Destroy the IRtcEngine object.
            IRtcEngine.Destroy();
            mRtcEngine = null;
            }
        }
        ```


## Test your app {#test-your-app}

To test your app, take the following steps:

1.  In Unity, click **File** \> **Build Settings**.

2.  Select the platform that you want to build and run your app. This quickstart takes iOS as an example to build and run the app. You can go to **Player Settings** to set various options for your app built by Unity.

3.  Click **Build** and Unity prompts you for a location to save the build. After Unity finishes building the iOS app, the Finder window appear containing the **Unity-iphone.xcodeproj** file.

4.  Open the the **Unity-iphone.xcodeproj** file in Xcode, select your project, enable Automatic Signing, and select your team profile.

5.  Click the ![](https://web-cdn.agora.io/docs-files/1635392130945) button to run your project. When the app launches, you should be able to see the user interface we created.

6.  Ask a friend to join the live streaming with you on the [demo app](https://webdemo.agora.io/basicVideoCall/index.html). Enter the same App ID and channel name.

    If your friend joins as a host, you should be able to see and hear each other; if as an audience member, you should only be able to see yourself while your friend can see and hear you.


## Next steps {#next-steps}

Generating a token by hand is not helpful in a production context. [Authenticate Your Users with Tokens](https://docs.agora.io/en/Video/token_server?platform=Android) shows you how to start the live streaming with a token that you retrieve from your server.

## See also {#see-also}

This section provides additional information for your reference.

-   Agora provides an open source sample project  on GitHub for your reference.

-   In addition to integrating the Agora Video SDK through Unity Asset Store, you can also manually download the Agora Video SDK:

    -   Go to [SDK Downloads](https://docs.agora.io/en/All/downloads?platform=Unity), download the Agora Video SDK. When the download completes, extract the files from the downloaded SDK package.

    -   Copy the `Plugins` subfolder from the `samples/Hello-Video-Unity-Agora/Assets/AgoraEngine` directory of the downloaded SDK to the `Assets` subfolder of your project.

    **Note:**

    -   Android or iOS developers using Unity Editor for macOS or Windows must also copy the macOS or the x86/X86\_64 subfolder to the specified directory.
    -   iOS developers also need to copy the `BL_BuildPostProcess.cs` file from the `samples/Hello-Video-Unity-Agora/Assets/AgoraEngine/Editor` directory.

