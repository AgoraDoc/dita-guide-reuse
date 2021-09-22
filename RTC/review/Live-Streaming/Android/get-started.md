# Get Started with Interactive Live Streaming Premium for Android {#get-started-with-product-name-for-platform}

The Agora Video SDK enables you to develop rapidly to enhance your social, work, education, and IoT apps with real-time engagements.

This page shows the minimum code you need to add live streaming into your app by using the Agora Video SDK for Android.

## Understand the tech {#understand-the-tech}

The following figure shows the workflow of a live streaming implemented by the Agora SDK.

![](https://web-cdn.agora.io/docs-files/1625465916613)

To start a live streaming, you implement the following steps in your app:

1.  Set the client role
2.  Retrieve a token
3.  Join a channel
4.  Publish and subscribe to in the channel

For an app client to join a channel, you need the following information:

-   The App ID: A randomly generated string provided by Agora for identifying your app. You can get the App ID from [Agora Console](https://console.agora.io/).

-   The user ID: The unique identifier of a user. You need to specify the user ID yourself, and ensure that it is unique in the channel.

-   A token: In a test or production environment, your app client retrieves tokens from your server. For rapid testing, you can use a temporary token with a validity period of 24 hours.

-   The channel name: A string that identifies the channel for the live streaming.


## Prerequisites {#prerequisites}

Before proceeding, ensure that your development environment meets the following requirements:

-   Android Studio 3.0 or later.
-   Android SDK API Level 16 or higher.
-   A mobile device that runs Android 4.1 or later.
-   A valid [Agora account](https://console.agora.io/).
-   An active Agora project with an App ID and a temporary token. For details, see [Get Started with Agora](https://docs.agora.io/en/Agora%20Platform/get_appid_token).
-   A computer with access to the internet. If your network has a firewall, follow the instructions in [Firewall Requirements](https://docs.agora.io/en/Agora%20Platform/firewall).

## Project setup {#project-setup}

1.  For new projects, in **Android Studio**, create a **Phone and Tablet** [Android project](https://developer.android.com/studio/projects/create-project) with an **Empty Activity**.

    After creating the project, **Android Studio** automatically starts gradle sync. Ensure that the sync succeeds before you continue.

2.  Integrate the Agora Video SDK into your project. For Agora SDK v3.5.0 or later, you can intergrate the SDK with mavenCentral with the following steps. For how to integrate SDKs earlier than v3.5.0, see [Other approches to intergrate the SDK](get-started.md#).

    a. In `/Gradle Scripts/build.gradle(Project: <projectname>)`, add the following lines to add the JitPack dependency.

    ```java
     all projects {
          repositories {
          ...
          maven { url 'https://www.jitpack.io' }
          }
     }
    ```

    b. In `/Gradle Scripts/build.gradle(Module: <projectname>.app)`, add the following lines to integrate the Agora Video SDK into your Android project.

    ```java
     ...
     dependencies {
         ...
         // For x.y.z, fill in a specific SDK version number. For example, 3.4.0
         implementation 'com.github.agorabuilder:native-full-sdk:x.y.z'
     }
    ```

3.  Add permissions for network and device access.

    In `/app/Manifests/AndroidManifest.xml`, add the following permissions after `</application>`:

    ```xml
     <uses-permission android:name="android.permission.INTERNET" />
     <uses-permission android:name="android.permission.CAMERA" />
     <uses-permission android:name="android.permission.RECORD_AUDIO" />
     <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
     <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
     <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
     <uses-permission android:name="android.permission.BLUETOOTH" />
    ```

4.  Prevent code obfuscation.

    In `/Gradle Scripts/proguard-rules.pro`, add the following line to prevent obfuscating the code of the SDK:

    ```
     -keep class io.agora.**{*;}
    ```


## Implement a client for Interactive Live Streaming Premium {#implement-a-client-for-product-name}

This section shows how to use the Agora Video SDK to implement Interactive Live Streaming Premium into your app step by step.

### Create the UI {#create-the-ui}

Create the user interface \(UI\) for the in the layout file of your project.

In the interface, you have one frame for local video and another for remote video.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <FrameLayout
        android:id="@+id/local_video_view_container"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="@android:color/white" />

    <FrameLayout
        android:id="@+id/remote_video_view_container"
        android:layout_width="160dp"
        android:layout_height="160dp"
        android:layout_alignParentEnd="true"
        android:layout_alignParentRight="true"
        android:layout_alignParentTop="true"
        android:layout_marginEnd="16dp"
        android:layout_marginRight="16dp"
        android:layout_marginTop="16dp"
        android:background="@android:color/darker_gray" />

</RelativeLayout>
```

### Handle the Android system logic {#handle-the-android-system-logic}

In this section, we import the necessary Android classes and handle the Android permissions.

1.  Import Android classes

    In `/app/java/com.example.<projectname>/MainActivity`, add the following lines after `package com.example.<projectname>`:

    ```java
    import androidx.core.app.ActivityCompat;
    import androidx.core.content.ContextCompat;
    
    import android.Manifest;
    import android.content.pm.PackageManager;
    import android.view.SurfaceView;
    import android.widget.FrameLayout;
    ```

2.  Handle the Android permissions

    When your app launches, check if the permissions necessary to insert video calling functionality into the app are granted. If the permissions are not granted, use the built-in Android functionality to request them; if they are, return `true`.

    In `/app/java/com.example.<projectname>/MainActivity`, add the following lines before the `onCreate` function:

    ```java
    // Java
    private static final int PERMISSION_REQ_ID = 22;
    
    private static final String[] REQUESTED_PERMISSIONS = {
         Manifest.permission.RECORD_AUDIO,
         Manifest.permission.CAMERA
    };
    
    private boolean checkSelfPermission(String permission, int requestCode) {
        if (ContextCompat.checkSelfPermission(this, permission) !=
                PackageManager.PERMISSION_GRANTED) {
            ActivityCompat.requestPermissions(this, REQUESTED_PERMISSIONS, requestCode);
            return false;
        }
        return true;
    }
    ```

    ```kotlin
    // Kotlin
    private val PERMISSION_REQ_ID_RECORD_AUDIO = 22
    private val PERMISSION_REQ_ID_CAMERA = PERMISSION_REQ_ID_RECORD_AUDIO + 1
    
    private fun checkSelfPermission(permission: String, requestCode: Int): Boolean {
      if (ContextCompat.checkSelfPermission(this, permission) != 
          PackageManager.PERMISSION_GRANTED) {
        ActivityCompat.requestPermissions(this,
                                         arrayOf(permission),
                                         requestCode)
        return false
      }
      return true
    }
    ```


### Implement the Interactive Live Streaming Premium logic {#implement-the-product-name-logic}

When your app opens, you create an RtcEngine instance, enable the video, join a channel, and publish the local video to the lower frame layout in the UI. When another user joins the channel, you app catches the join event and adds the remote video to the top frame layout in the UI.

The following figure shows the API call sequence of implementing Interactive Live Streaming Premium.

![](https://web-cdn.agora.io/docs-files/1625208380588)

To implement this logic, take the following steps:

1.  Import the Agora classes.

    In `/app/java/com.example.<projectname>/MainActivity`, add the following lines after `import android.os.Bundle;`:

    ```java
     import io.agora.rtc.IRtcEngineEventHandler;
     import io.agora.rtc.RtcEngine;
     import io.agora.rtc.video.VideoCanvas;
    ```

2.  Create the variables that you use to create and join a video call channel.

    In `/app/java/com.example.<projectname>/MainActivity`, add the following lines after `AppCompatActivity {`:

    ```java
     // Java
     // Fill the App ID of your project generated on Agora Console.
     private String appId = "";
     // Fill the channel name.
     private String channelName = "";  
     // Fill the temp token generated on Agora Console.
     private String token = "";
     private RtcEngine mRtcEngine;
    
     private final IRtcEngineEventHandler mRtcEventHandler = new IRtcEngineEventHandler() {
         @Override
         // Listen for the remote user joining the channel to get the uid of the user.
         public void onUserJoined(int uid, int elapsed) {
             runOnUiThread(new Runnable() {
                 @Override
                 public void run() {
                     // Call setupRemoteVideo to set the remote video view after getting uid from the onUserJoined callback.
                     setupRemoteVideo(uid);
                 }
             });
         }
     };
    ```

    ```kotlin
     // Kotlin
     // Fill the App ID of your project generated on Agora Console.
     private val APP_ID = ""
     // Fill the channel name.
     private val CHANNEL = ""
     // Fill the temp token generated on Agora Console.
     private val TOKEN = ""
    
     private var mRtcEngine: RtcEngine ?= null
    
     private val mRtcEventHandler = object : IRtcEngineEventHandler() {
       // Listen for the remote user joining the channel to get the uid of the user.
       override fun onUserJoined(uid: Int, elapsed: Int) {
         runOnUiThread {
             // Call setupRemoteVideo to set the remote video view after getting uid from the onUserJoined callback. 
             setupRemoteVideo(uid) 
         }
       }
     }
    ```

3.  Initialize the app and join the channel.

    Call the following core methods to join a channel in the `MainActivity` class. In the following sample code, the `initializeAndJoinChannel` function encapsulates these core methods.

    In `/app/java/com.example.<projectname>/MainActivity`, add the following lines after the `onCreate` function:

    ```language-java
    // Java
    private void initializeAndJoinChannel() {
     try {
         mRtcEngine = RtcEngine.create(getBaseContext(), appId, mRtcEventHandler);
     } catch (Exception e) {
         throw new RuntimeException("Check the error.");
     }
    
     // For a live streaming scenario, set the channel profile as BROADCASTING.
     mRtcEngine.setChannelProfile(Constants.CHANNEL_PROFILE_LIVE_BROADCASTING);
     // Set the client role as BORADCASTER or AUDIENCE according to the scenario.
     mRtcEngine.setClientRole(Constants.CLIENT_ROLE_BROADCASTER);
    
     // By default, video is disabled, and you need to call enableVideo to start a video stream.
     mRtcEngine.enableVideo();
    
     FrameLayout container = findViewById(R.id.local_video_view_container);
     // Call CreateRendererView to create a SurfaceView object and add it as a child to the FrameLayout.
     SurfaceView surfaceView = RtcEngine.CreateRendererView(getBaseContext());
     container.addView(surfaceView);
     // Pass the SurfaceView object to Agora so that it renders the local video.
     mRtcEngine.setupLocalVideo(new VideoCanvas(surfaceView, VideoCanvas.RENDER_MODE_FIT, 0));
    
     // Join the channel with a token.
     mRtcEngine.joinChannel(token, channelName, "", 0);
    }    
       
    ```

4.  Add the remote interface when a remote host joins the channel.

    In `/app/java/com.example.<projectname>/MainActivity`, add the following lines after the `initializeAndJoinChannel` function:

    ```java
     // Java
     private void setupRemoteVideo(int uid) {
         FrameLayout container = findViewById(R.id.remote_video_view_container);
         SurfaceView surfaceView = RtcEngine.CreateRendererView(getBaseContext());
         surfaceView.setZOrderMediaOverlay(true);
         container.addView(surfaceView);
         mRtcEngine.setupRemoteVideo(new VideoCanvas(surfaceView, VideoCanvas.RENDER_MODE_FIT, uid));
     }
    ```

    ```kotlin
     // Kotlin
     private fun setupRemoteVideo(uid: Int) {
         val remoteContainer = findViewById(R.id.remote_video_view_container) as FrameLayout
    
         val remoteFrame = RtcEngine.CreateRendererView(baseContext)
         remoteFrame.setZOrderMediaOverlay(true)
         remoteContainer.addView(remoteFrame)
         mRtcEngine!!.setupRemoteVideo(VideoCanvas(remoteFrame, VideoCanvas.RENDER_MODE_FIT, uid))
     }
    ```


### Start and stop your app {#start-and-stop-your-app}

Now you have created the Interactive Live Streaming Premium functionality, start and stop the app. In this implementation, a live streaming starts when the user opens your app. The live streaming ends when the user closes your app.

1.  Check that the app has the correct permissions. If permissions are granted, call `initializeAndJoinChannel` to join a video call channel.

    In `/app/java/com.example.<projectname>/MainActivity`, replace `onCreate` with the following code in the `MainActivity` class.

    ```java
     // Java
     @Override
     protected void onCreate(Bundle savedInstanceState) {
         super.onCreate(savedInstanceState);
         setContentView(R.layout.activity_main);
    
         // If all the permissions are granted, initialize the RtcEngine object and join a channel.
         if (checkSelfPermission(REQUESTED_PERMISSIONS[0], PERMISSION_REQ_ID) undefined checkSelfPermission(Manifest.permission.CAMERA, PERMISSION_REQ_ID_CAMERA)) {
           initializeAndJoinChannel()
         }
     }
    ```

    ```kotlin
     // Kotlin
     override fun onCreate(savedInstanceState: Bundle?) {
         super.onCreate(savedInstanceState)
         setContentView(R.layout.activity_main)
    
         // If all the permissions are granted, initialize the RtcEngine object and join a channel.
         if (checkSelfPermission(Manifest.permission.RECORD_AUDIO, PERMISSION_REQ_ID_RECORD_AUDIO) && checkSelfPermission(Manifest.permission.CAMERA, PERMISSION_REQ_ID_CAMERA)) {
           initializeAndJoinChannel()
         }
     }
    ```

2.  When the user closes this app, clean up all the resources you created in `initializeAndJoinChannel`.

    In `/app/java/com.example.<projectname>/MainActivity`, add `onDestroy` after the `onCreate`function.

    ```java
     // Java
     protected void onDestroy() {
         super.onDestroy();
    
         mRtcEngine.leaveChannel();
         mRtcEngine.destroy();
     }
    ```

    ```kotlin
     // Kotlin
     override fun onDestroy() {
         super.onDestroy()
    
         mRtcEngine?.leaveChannel()
         RtcEngine.destroy()
     }
    ```


## Test your app {#test-your-app}

Connect an Android device to your computer, and click on your Android Studio. A moment later you will see the project installed on your device.

Take the following steps to test the live streaming app:

1.  Grant microphone and camera access to your app.

2.  When the app launches, you should be able to see yourself on the local view.

3.  Ask a friend to join the video call with you on the [demo app](https://webdemo.agora.io/basicVideoCall/index.html). Enter the same App ID and channel name.

4.  After your friend joins the channel, you should be able to see and hear each other.


## Next steps {#next-steps}

Generating a token by hand is not helpful in a production context. [Authenticate Your Users with Tokens](https://docs.agora.io/en/Video/token_server?platform=Android) shows you how to start video calling with a token that you retrieve from your server.

## See also {#see-also}

This section provides additional information for your reference.

### Sample project {#sample-project}

Agora provides an open source sample project [JoinChannelVideo](https://github.com/AgoraIO/API-Examples/blob/master/Android/APIExample/app/src/main/java/io/agora/api/example/examples/basic/JoinChannelVideo.java) on GitHub that implements one-to-one video call and group video call for your reference.

### Other approches to integrate the SDK {#other}

In addition to integrating the Agora Video SDK for Android through JitPack, you can also import the SDK into your project through mavenCentral or by manually copying the SDK files.

**Automatically integrate the SDK with mavenCentral**

1.  In `/Gradle Scripts/build.gradle(Project: <projectname>)`, add the following lines to add the mavenCentral dependency:

    ```java
    buildscript {
        repositories {
            ...
            mavenCentral()
        }
        ...
    }
    
    allprojects {
        repositories {
            ...
            mavenCentral()
        }
    }
    ```

2.  In `/Gradle Scripts/build.gradle(Module: <projectname>.app)`, add the following lines to integrate the Agora Video SDK into your Android project:

    ```java
    ...
    dependencies {
     ...
     // For x.y.z, fill in a specific SDK version number. For example, 3.5.0.
     // Get the latest version number through the release notes.
     implementation 'io.agora.rtc:full-sdk:x.y.z'
    }
    ```


This method only applies to v3.5.0 or later.

**Manually copy the SDK files**

1.  Go to [SDK Downloads](https://docs.agora.io/en/Video/downloads?platform=Android), download the latest version of the Agora Video SDK, and extract the files from the downloaded SDK package.

2.  Copy the following files or subfolders from the libs folder of the downloaded SDK package to the path of your project.

    |File or subfolder|Path of your project|
    |-----------------|--------------------|
    |`agora-rtc-sdk.jar` file|`/app/libs/`|
    |`arm-v8a` folder|`/app/src/main/jniLibs/`|
    |`armeabi-v7a` folder|`/app/src/main/jniLibs/`|
    |`x86` folder|`/app/src/main/jniLibs/`|
    |`x86_64` folder|`/app/src/main/jniLibs/`|
    |`include` folder|`/app/src/main/jniLibs/`|


-   If you use the armeabi architecture, copy files from the `armeabi-v7a` folder to the `armeabi` file of your project. Contact [support@agora.io](mailto:support@agora.io) if you encounter any incompability issue.

-   Not all libraries in the SDK package are necessary. Refer to [How can I reduce the app size after integrating the RTC Native SDK](https://docs.agora.io/en/Video/faq/reduce_app_size_rtc) for details.


