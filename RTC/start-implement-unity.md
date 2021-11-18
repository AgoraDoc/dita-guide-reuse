# Implement a client for [product-name]

This section shows how to use the [sdk-name] to implement [feature] into your app step by step.

## Create the UI

Agora recommends adding the following elements to the UI:
<ul>
<li props="video">The local video view</li>
<li props="video">The remote video view</li>
<li props="video">An end-call button </li>
<li props="video live">The view of the host</li>
<li props="video live">An exit button</li>
</ul>
If you use the **Unity Editor** to build your UI, ensure that you bind **VideoSurface.cs** to the **GameObjects** designated for local and remote videos. In the following example, the **VideoSurface.cs** is applied to the cube for the local video and to the cylinder for the remote video.

![UI]

## Get the device permission (Android only)

If you build for Android, you should add in APIs to check and request the device permission.

Unity versions later than **UNITY_2018_3_OR_NEWER** do not automatically request camera and microphone permissions from your Android device. If you are using one of these versions, call the `CheckPermission` method to request access to the camera and microphone of your Android device.

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

## Implement the [product-name] logic
The following figure shows the API call sequence of implementing the [product-name]:

![start-api-sequence-unity]

To implement this logic, take the following steps:

**Initialize the IRtcEngine**

- Call the `GetEngine` method and pass in the App ID to initialize the `IRtcEngine` object.
- According to your scenarios, you can also listen for callback events, such as when the local user joins the channel, and when the first video frame of a remote user is decoded.
 
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
      
<p props="live"><b>Set the channel profile</b> 

- Call the <code>SetChannelProfile</code> method to set the channel profile as <code>LIVE_BROADCASTING</code>.
- One <code>IRtcEngine</code> object uses one profile only. If you want to switch to another profile, destroy the current <code>IRtcEngine</code> object with the <code>Destroy</code> method and create a new one before calling the <code>SetChannelProfile</code> method.
   
  ```c#
  // Set the channel profile as LIVE_BROADCASTING.
  mRtcEngine.SetChannelProfile(CHANNEL_PROFILE.CHANNEL_PROFILE_LIVE_BROADCASTING);
  ```
</p>

<p props="live"><b>Set the client role</b> 

An interactive streaming channel has two client roles: <code>BROADCASTER</code> and <code>AUDIENCE</code>, and the default role is <code>AUDIENCE</code>. After setting the channel profile to <code>LIVE_BROADCASTING</code>, you can call the <code>SetClientRole</code> method and pass in the client role set by the user.
   
Note that in an interactive live streaming, only the host can be heard and seen. If you want to switch the client role after joining the channel, call the <code>SetClientRole</code> method.
   ```c#
   // Set the client role as the host.
   mRtcEngine.SetClientRole(CLIENT_ROLE.BROADCASTER);
   ```
</p>   

**Set the local video**

After initializing an `IRtcEngine object`, set the local video before joining a channel so that you can see yourself in the call. Follow these steps to configure the local video:

- Call `EnableVideo` to enable the video module.
- Call `EnableVideoObserver` to get the local video.
   
  ```c#
  // Enable the video module.
  mRtcEngine.EnableVideo();
  // Get the local video and pass it on to Unity.
  mRtcEngine.EnableVideoObserver();
  ```
- Choose an object to display the local video, and drag the **VideoSurface.cs** file to **Script**, so that you can bind the **VideoSurface.cs** file to the object and see the local video. Unity renders 3D objects by default, such as Cube, Cylinder and Plane. To render the object in other types, modify the renderer in the **VideoSurface.cs** file.
  ![drag]
  
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

   <ul>
   <li><code>channelKey</code>: The token for identifying the role and privileges of a user. Set it as one of the following values:
   <ul>
   <li>A temporary token generated in Agora Console. A temporary token is valid for 24 hours.</li>
   <li>A token generated at the server. This applies to scenarios with high-security requirements.</li>
   </ul>
   </li>
   <li><code>channelName</code>:The unique name of the channel to join. Users that input the same channel name join the same channel.</li>
   <li><code>uid</code>:Integer. The unique ID of the local user. If you set <code>uid</code> as 0, the SDK automatically assigns one user ID and returns it in the <code>OnJoinChannelSuccessHandler</code> callback.</li>
   </ul>
   
```c#
// Join a channel. 
mRtcEngine.JoinChannelByKey(null, channel, null, 0);
```

**Set the remote video**

In a video call, you should be able to see remote users too. Call `SetForUser` in **VideoSurface.cs** after joining a channel. When a remote user joins the channel, the SDK triggers the `OnUserJoinedHandler` callback, and you can get the user’s uid.

Follow the steps to set the remote video:
   
   - In the **VideoSurface.cs** file, call the `SetForUser` method in the callback, and pass in the `uid` to set the video of the remote user.
   - Drag the **VideoSurface.cs** script to the target object, so that you can bind the **VideoSurface.cs** to the object and see the remote video.
   
   <note>The default value of `uid` in the `SetForUser` method is 0. To see the remote user, ensure that you input the uid of the remote user in `SetForUser`. Otherwise, the SDK uses the default value and you can only see the local video.</note>
     
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
To remove the **VideoSurface.cs** script from the object, see the following sample codes:
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

**Leave the channel and destroy the <code>IRtcEngine</code> object**
   
According to your scenario, such as when the call ends and when you need to close the app, call `LeaveChannel` to leave the current call, and call `DisableVideoObserver` to disable the video. 
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
After leaving the channel, if you want to exit the app or release the memory of `IRtcEngine`, call `Destroy` to destroy the `IRtcEngine` object.
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
   