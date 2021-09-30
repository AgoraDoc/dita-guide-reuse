# Get Started with Interactive Live Streaming Premium for macOS {#get-started-with-product-name-for-platform}

The Agora Video SDK enables you to develop rapidly to enhance your social, work, education, and IoT apps with real-time engagements.

This page shows the minimum code you need to add live streaming into your app by using the Agora Video SDK for macOS.

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

-   Xcode 9.0 or later.
-   A valid [Agora account](https://console.agora.io/).
-   An active Agora project with an App ID and a temporary token. For details, see [Get Started with Agora](https://docs.agora.io/en/Agora%20Platform/get_appid_token).
-   A computer with access to the internet. If your network has a firewall, follow the instructions in [Firewall Requirements](https://docs.agora.io/en/Agora%20Platform/firewall).

## Project setup {#project-setup}

For new projects, in **Xcode**, follow the steps to create the environment necessary to add live streaming into your app.

1.  Create a new macOS app and configure the following settings:

    -   **Product Name**: Any name you like.

    -   **Team**: If you have added a team, choose it from the pop-up menu. If not, you can see the **Add account** button. Click it, input your Apple ID, and click **Next** to add your team.

    -   **Organization Identifier**: The identifier of your organization. If you do not belong to an organization, use any identifier you like.

    -   **Interface**: Choose **Storyboard**.

    -   **Language**: Choose **Swift**.

2.  Integrate the Video SDK into your project:

    ```http
    	 https://github.com/AgoraIO/AgoraRtcEngine_iOS
    ```

    In **Choose Package Options**, specify the Video SDK version you want to use. You need to fill in x.y.z for version x.y.z and x.y.z-r.a for version x.y.z.a. For example, fill in 3.5.0 for version 3.5.0 and 3.5.0-r.2 for version 3.5.0.2.

    **Attention:**

    -   For the , Agora provides Swift Packages for 3.4.3 or later versions.
    -   If you have issues installing this Swift Package, try going to **File** \> **Swift Packages** \> **Reset Package Caches**.
    -   For more integration methods, see [Other approaches to integrate the SDK](get-started.md#).


    1.  Install CocoaPods if you have not. See [Getting Started with CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started).
    2.  In Terminal, navigate to the root of your project folder, and run the \[/dita/topic/topic/body/ol/li/ol/li/codeph \{"- topic/codeph "\}\)pod init\(codeph\] command to create a \[/dita/topic/topic/body/ol/li/ol/li/codeph \{"- topic/codeph "\}\)Podfile\(codeph\] in the project folder.
    3.  Open the \[/dita/topic/topic/body/ol/li/ol/li/codeph \{"- topic/codeph "\}\)Podfile\(codeph\], and replace all contents with the following code. Remember to replace \[/dita/topic/topic/body/ol/li/ol/li/codeph \{"- topic/codeph "\}\)Your App\(codeph\] with the target name of your project. \[/dita/topic/topic/body/ol/li/ol/li/codeblock \{"- topic/codeblock "\}\) \# platform :macos, '10.11' target 'Your App' do pod 'AgoraRtcEngine\_macOS' end \(codeblock\]

        **Attention:** For more integration methods, see [Other approaches to integrate the SDK](get-started.md#).

3.  In Terminal, run the \[/dita/topic/topic/body/ol/li/p/codeph \{"- topic/codeph "\}\)pod install\(codeph\] command to install the SDK. When the SDK is installed successfully, you can see \[/dita/topic/topic/body/ol/li/p/codeph \{"- topic/codeph "\}\)Pod installation complete!\(codeph\] in Terminal and an \[/dita/topic/topic/body/ol/li/p/codeph \{"- topic/codeph "\}\)xcworkspace\(codeph\] file in the project folder.

4.  Open the \[/dita/topic/topic/body/ol/li/p/codeph \{"- topic/codeph "\}\)xcworkspace\(codeph\] file for any further steps.

5.  [Enable automatic signing](https://help.apple.com/xcode/mac/current/#/dev23aab79b4) for your project.

6.  Set the deployment target for your app:

    1.  In the [project editor](https://help.apple.com/xcode/mac/current/#/devdab46c612), choose your target and click **General**.

    2.  In the **Deployment Info** settings, choose the operating system version for your  app from the pop-up menu.

7.  Add permissions for microphone and camera usage. In the [Project navigator](https://help.apple.com/xcode/mac/current/#/dev7d7220fbb), open the `info.plist` file of your project and [add the following properties](https://help.apple.com/xcode/mac/current/#/dev3f399a2a6):

    -   Privacy - Microphone Usage Description

    -   Privacy - Camera Usage Description


## Implement a client for Interactive Live Streaming Premium {#implement-a-client-for-product-name}

This section shows how to use the Agora Video SDK to implement live streaming in your app step by step.

### Create the UI {#create-the-ui}

In the interface, you should have one frame for local video and another for remote video. In `ViewController.swift`, replace any existing content with the following:

\[/dita/topic/topic/topic/body/p/codeblock \{"- topic/codeblock "\}\) import AppKit class ViewController: NSViewController \{ var localView: NSView! var remoteView: NSView! override func viewDidLoad\(\) \{ super.viewDidLoad\(\) initView\(\) \} override func viewDidLayout\(\) \{ super.viewDidLayout\(\) remoteView.frame = self.view.bounds localView.frame = CGRect\(x: self.view.bounds.width - 90, y: 0, width: 90, height: 160\) \} func initView\(\) \{ remoteView = NSView\(\) self.view.addSubview\(remoteView\) localView = NSView\(\) self.view.addSubview\(localView\) \} \} \(codeblock\]

### Implement the Interactive Live Streaming Premium logic {#implement-the-product-name-logic}

When your app opens, you create an `AgoraRtcEngineKit` instance, enable the video,join a channel, and if the local user is a host, publish the local video to the lower frame layout in the UI. When another host joins the channel, your app catches the join event and adds the remote video to the top frame layout in the UI.

The following figure shows the API call sequence of implementing Interactive Live Streaming Premium.

![](https://web-cdn.agora.io/docs-files/1627888336100)

To implement this logic, take the following steps:

1.  Import the Agora kit and add the `agoraKit` variable. Modify your `ViewController.swift` as follows:

    ```swift
    import UIKit
    // Add this line to import the Agora kit 
    import AgoraRtcKit
    class ViewController: UIViewController {
        var localView: UIView!
        var remoteView: UIView!
        // Add this linke to add the agoraKit variable
        var agoraKit: AgoraRtcEngineKit?
    }
    
    override func viewDidLoad() {
               super.viewDidLoad()
               initView()
            }
    ```

2.  Initialize the app and join the channel.

    Call the core methods to initialize the app and join a channel. In this example app, this functionality is encapsulated in the `initializeAndJoinChannel` function.

    In `ViewController.swift`, add the following lines after the `initView` function, and fill in your App ID, temporary token, and channel name:

    ```swift
     func initializeAndJoinChannel() {
       // Pass in your App ID here
       agoraKit = AgoraRtcEngineKit.sharedEngine(withAppId: "Your App ID", delegate: self)
       // For a live streaming scenario, set the channel profile as liveBroadcasting.
       agoraKit?.setChannelProfile(.liveBroadcasting)
       // Set the client role as broadcaster or audience.
       agoraKit?.setClientRole(.broadcaster)
       // Video is disabled by default. You need to call enableVideo to start a video stream.
       agoraKit?.enableVideo()
            // Create a videoCanvas to render the local video
            let videoCanvas = AgoraRtcVideoCanvas()
            videoCanvas.uid = 0
            videoCanvas.renderMode = .hidden
            videoCanvas.view = localView
            agoraKit?.setupLocalVideo(videoCanvas)
    
       // Join the channel with a token. Pass in your token and channel name here
       agoraKit?.joinChannel(byToken: "Your token", channelId: "Channel name", info: nil, uid: 0, joinSuccess: { (channel, uid, elapsed) in
            })
        }
    ```

3.  Add the remote interface when a remote host joins the channel.

    In `ViewController.swift`, add the following lines after the `ViewController` class:

    ```swift
    extension ViewController: AgoraRtcEngineDelegate {
        // This callback is triggered when a remote host joins the channel
        func rtcEngine(_ engine: AgoraRtcEngineKit, didJoinedOfUid uid: UInt, elapsed: Int) {
            let videoCanvas = AgoraRtcVideoCanvas()
            videoCanvas.uid = uid
            videoCanvas.renderMode = .hidden
            videoCanvas.view = remoteView
            agoraKit?.setupRemoteVideo(videoCanvas)
        }
    }
    ```


### Start and stop your app {#start-and-stop-your-app}

Now you have created the live streaming functionality, add the functionality of starting and stopping the app. In this implementation, a live stream starts when the user opens your app and ends when the user closes your app.

To implement the functionality of starting and stopping the app:

1.  When the view is loaded, call `initializeAndJoinChannel` to join a live streaming channel.

    In `ViewController.swift`, add the `initializeAndJoinChannel` function inside the `viewDidLoad` function:

    ```swift
    override func viewDidLoad() {
           super.viewDidLoad()
           initView()
           // Add this line
           initializeAndJoinChannel()
        }
    ```

2.  When the user closes this app, clean up all the resources used by your app.

    In `ViewController.swift`, add `viewDidDisappear` after the `initializeAndJoinChannel` function.

    ```swift
    override func viewDidDisappear(_ animated: Bool) {
        super.viewDidDisappear(animated)
        agoraKit?.leaveChannel(nil)
        AgoraRtcEngineKit.destroy()
    	}
    ```


## Test your app {#test-your-app}

To test your app, follow the steps:

1.  Fill in the `appId` and `token` parameters with the App ID and temporary token that you retrieve from Agora Console. Fill `channelName` with the channel name that you use to generate the temporary token.

2.  Connect an macOS device to your computer, and click Run on your Xcode. A moment later you will see the project installed on your device.

3.  When the app launches, you should be able to see the user interface we created.

4.  Ask a friend to join the live streaming with you on the [demo app](https://webdemo.agora.io/basicVideoCall/index.html). Enter the same App ID and channel name.

    If your friend joins as a host, you should be able to see and hear each other; if as an audience member, you should only be able to see yourself while your friend can see and hear you.


## Next steps {#next-steps}

Generating a token by hand is not helpful in a production context. [Authenticate Your Users with Tokens](https://docs.agora.io/en/Video/token_server?platform=Android) shows you how to start the live streaming with a token that you retrieve from your server.

## See also {#see-also}

This section provides additional information for your reference:

### Sample project {#sample-project}

Agora provides an open source sample project [OpenLive-iOS-Swift](https://github.com/AgoraIO/Basic-Video-Broadcasting/tree/master/OpenLive-iOS) on GitHub that implements interactive live video streaming for your reference.

### Other approaches to integrate the SDK {#othermethods}

In addition to integrating the Agora Video SDK for macOS through CocoaPods, you can also import the SDK into your project by manually copying the SDK files.

1.  Install CocoaPods if you have not. See [Getting Started with CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started).
2.  In Terminal, navigate to the root of your project folder, and run the \[/dita/topic/topic/topic/body/ol/li/codeph \{"- topic/codeph "\}\)pod init\(codeph\] command to create a \[/dita/topic/topic/topic/body/ol/li/codeph \{"- topic/codeph "\}\)Podfile\(codeph\] in the project folder.
3.  Open the \[/dita/topic/topic/topic/body/ol/li/codeph \{"- topic/codeph "\}\)Podfile\(codeph\], and replace all contents with the following code. Remember to replace \[/dita/topic/topic/topic/body/ol/li/codeph \{"- topic/codeph "\}\)Your App\(codeph\] with the target name of your project. \[/dita/topic/topic/topic/body/ol/li/codeblock \{"- topic/codeblock "\}\) \# platform :target 'Your App' do pod '' end\(codeblock\]
4.  In Terminal, run the \[/dita/topic/topic/topic/body/ol/li/codeph \{"- topic/codeph "\}\)pod install\(codeph\] command to install the SDK. When the SDK is installed successfully, you can see \[/dita/topic/topic/topic/body/ol/li/codeph \{"- topic/codeph "\}\)Pod installation complete!\(codeph\] in Terminal and an \[/dita/topic/topic/topic/body/ol/li/codeph \{"- topic/codeph "\}\)xcworkspace\(codeph\] file in the project folder.
5.  Open the \[/dita/topic/topic/topic/body/ol/li/codeph \{"- topic/codeph "\}\)xcworkspace\(codeph\] file for any further steps.

**Manually copy the SDK files**

1.  Go to [SDK Downloads](%5Bsdk-download%5D), download the latest version of the Agora Video SDK, and extract the files from the downloaded SDK package.

2.  From the `libs` folder of the downloaded SDK package, copy the files or subfolders you need to the root of your project folder.

    **Attention:** Certain files and subfolders under the `libs` folder are optional. See [extension libraries](https://docs.agora.io/en/Voice/faq/reduce_app_size_rtc?platform=iOS#extension_libraries) for details.

3.  In Xcode, [link your target to the frameworks or libraries](https://help.apple.com/xcode/mac/current/#/dev51a648b07) you have copied. Be sure to choose **Embed & Sign** from the pop-up menu in the Embed column.

    **Attention:**

    -   Apple does not allow an app extension to contain any dynamic library. If you are integrating the Agora SDK to an app extension, choose **Do Not Embed** in the Embed column.
    -   The Agora SDK uses libc++ \(LLVM\) by default. Contact [support@agora.io](mailto:support@agora.io) if you want to use libstdc++ \(GNU\). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio and video real devices.

### Objective-C code sample {#objective-c-code-sample}

To implement Interactive Live Streaming Premium in your app using Objective-C:

1.  Replace the contents in the `ViewController.h` file with the following:

    \[/dita/topic/topic/topic/body/ol/li/p/codeblock \{"- topic/codeblock "\}\) \#import <AppKit/AppKit.h\> \#import <AgoraRtcKit/AgoraRtcEngineKit.h\> @interface ViewController : NSViewController <AgoraRtcEngineDelegate\> @property \(strong, nonatomic\) AgoraRtcEngineKit \*agoraKit; @end \(codeblock\]

2.  Replace the contents in the `ViewController.m` file with the following:

    \[/dita/topic/topic/topic/body/ol/li/codeblock \{"- topic/codeblock "\}\) \#import "ViewController.h" \#import     @interface ViewController \(\) @property \(nonatomic, strong\) \*localView; @property \(nonatomic, strong\) \*remoteView; @end
    
        @implementation ViewController - \(void\)viewDidLoad \{ \[super viewDidLoad\]; \[self initViews\]; \[self initializeAndJoinChannel\]; \}
    
        - \(void\)viewDidLayoutSubviews \{ \[super viewDidLayoutSubviews\]; self.remoteView.frame = self.view.bounds; self.localView.frame = CGRectMake\(self.view.bounds.size.width - 90, 0, 90, 160\); \}
    
        - \(void\)initViews \{ self.remoteView = \[\[alloc\] init\]; \[self.view addSubview:self.remoteView\]; self.localView = \[\[alloc\] init\]; \[self.view addSubview:self.localView\]; \}
    
        - \(void\)initializeAndJoinChannel \{ // Pass in your App ID here self.agoraKit = \[AgoraRtcEngineKit sharedEngineWithAppId:@"Your App ID" delegate:self\]; \[self.agoraKit setChannelProfile:AgoraChannelProfileLiveBroadcasting\]; \[self.agoraKit setClientRole:AgoraClientRoleBroadcaster\]; \[self.agoraKit enableVideo\]; AgoraRtcVideoCanvas \*videoCanvas = \[\[AgoraRtcVideoCanvas alloc\] init\]; videoCanvas.uid = 0; videoCanvas.renderMode = AgoraVideoRenderModeHidden; videoCanvas.view = self.localView; \[self.agoraKit setupLocalVideo:videoCanvas\]; // Pass in your token and channel name here \[self.agoraKit joinChannelByToken:@"Your token" channelId:@"Channel name" info:nil uid:0 joinSuccess:^\(NSString \* \_Nonnull channel, NSUInteger uid, NSInteger elapsed\) \{ \}\]; \}
    
        - \(void\)rtcEngine:\(AgoraRtcEngineKit \*\)engine didJoinedOfUid:\(NSUInteger\)uid elapsed:\(NSInteger\)elapsed \{ AgoraRtcVideoCanvas \*videoCanvas = \[\[AgoraRtcVideoCanvas alloc\] init\]; videoCanvas.uid = uid; videoCanvas.renderMode = AgoraVideoRenderModeHidden; videoCanvas.view = self.remoteView; \[self.agoraKit setupRemoteVideo:videoCanvas\]; \}
    
        - \(void\)viewDidDisappear:\(BOOL\)animated \{ \[super viewDidDisappear:animated\]; \[self.agoraKit leaveChannel:nil\]; \[AgoraRtcEngineKit destroy\]; \} @end
    
    \(codeblock\]


\#\# Listening for audience events

The Agora Video SDK does not report events of an audience member in a live streaming channel. Refer to [How can I listen for an audience joining or leaving an interactive live streaming channel](https://docs.agora.io/en/Interactive%20Broadcast/faq/audience_event) if your scenario requires so.

### Integrate earlier versions of the SDK {#integrate-earlier-versions-of-the-sdk}

Choose one of the following methods to integrate a version of the Agora macOS SDK earlier than v3.2.0.

#### Method 1: Through CocoaPods {#method-1-through-cocoapods}

1.  Ensure that you have installed CocoaPods before performing the following steps. See the installation guide in [Getting Started with CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started).

2.  In Terminal, navigate to the project path, and run the `pod init` command to create a `Podfile` in the project folder.

3.  Open the `Podfile`, delete all contents, and input the following codes. Remember to replace `Your App` with the target name of your project and replace `version` with the version of the SDK that you want to integrate. For information about the SDK version, see [Release Notes](%5Brelease-notes%5D).

    \[/dita/topic/topic/topic/topic/body/ol/li/codeblock \{"- topic/codeblock "\}\) \# platform :target 'Your App' do pod '', 'version' end\(codeblock\]

4.  Return to Terminal, and run the `pod install` command to install the Agora SDK. Once you successfully install the SDK, it shows `Pod installation complete!` in Terminal, and you can see an `xcworkspace` file in the project folder.

5.  Open the generated `xcworkspace` file.


#### Method 2: Through your local storage {#method-2-through-your-local-storage}

You need to use different integration methods to integrate different versions of the SDK. Click the following version categories to expand the corresponding integration steps.

##### From v3.2.0 to v3.2.1 {#from-v320-to-v321}

<% if \(product == "audio"\) \{ %\>

1.  Copy the `AgoraRtcKit.framework`, `Agorafdkaac.framework`, and `AgoraSoundTouch.framework` dynamic libraries to the `./project_name` folder in your project \(`project_name` is an example of your project name\). <% \} if \(product == "video"\) \{ %\>

2.  Copy the `AgoraRtcKit.framework`, `Agorafdkaac.framework`, `Agoraffmpeg.framework`, and `AgoraSoundTouch.framework` dynamic libraries to the `./project_name` folder in your project \(`project_name` is an example of your project name\). <% \} %\>

3.  Open Xcode, and navigate to **TARGETS \> Project Name \> General \> Frameworks, Libraries, and Embedded Content**.


<% if \(product == "audio"\) \{ %\> 3. Click **+** \> **Add Other…** \> **Add Files** to add the `AgoraRtcKit.framework`, `Agorafdkaac.framework`, and `AgoraSoundTouch.framework` dynamic libraries. Ensure that the **Embed** attribute of these dynamic libraries is **Embed & Sign**.

Once these dynamic libraries are added, the project automatically links to other system libraries. <% \} if \(product == "video"\) \{ %\> 3. Click **+** \> **Add Other…** \> **Add Files** to add the `AgoraRtcKit.framework`, `Agorafdkaac.framework`, `Agoraffmpeg.framework`, and `AgoraSoundTouch.framework` dynamic libraries. Ensure that the **Embed** attribute of these dynamic libraries is **Embed & Sign**.

Once these dynamic libraries are added, the project automatically links to other system libraries. <% \} %\>

-   According to the official requirements of Apple, the Extension of an app cannot contain the dynamic library. If you need to integrate the SDK with the dynamic library in the Extension, change the file status to **Do Not Embed**.
-   The Agora SDK uses libc++ \(LLVM\) by default. Contact support@agora.io if you want to use libstdc++ \(GNU\). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.

##### From v3.0.1 to v3.1.2 {#from-v301-to-v312}

1.  Copy the `AgoraRtcKit.framework` dynamic library to the `./project_name` folder in your project \(`project_name` is an example of your project name\).

2.  Open Xcode, and navigate to **TARGETS \> Project Name \> General \> Frameworks, Libraries, and Embedded Content**.

3.  Click **+ \> Add Other… \> Add Files** to add the `AgoraRtcKit.framework` dynamic library. Ensure that the **Embed** attribute of the dynamic library is **Embed & Sign**. Once the dynamic library is added, the project automatically links to other system libraries.


-   According to the official requirements of Apple, the Extension of an app cannot contain the dynamic library. If you need to integrate the SDK with the dynamic library in the Extension, change the file status to **Do Not Embed**.
-   The Agora SDK uses libc++ \(LLVM\) by default. Contact support@agora.io if you want to use libstdc++ \(GNU\). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.

##### v3.0.0 {#v300}

In v3.0.0, the SDK package contains an `AgoraRtcKit.framework` dynamic library and an `AgoraRtcKit.framework` static library. Choose which of these libraries to add according to your needs. The paths of the two libraries in the SDK package are as follows:

-   The path of the dynamic library: `./Agora_Native_SDK_for_macOS_..._Dynamic/libs`.

-   The path of the static library: `./Agora_Native_SDK_for_macOS_.../libs`.


If you need to check the type of a library, run the following command: \[/dita/topic/topic/topic/topic/topic/body/div/tt \{"- topic/tt "\}\)file /path/xxx.framework/xxx\(tt\] \(\[/dita/topic/topic/topic/topic/topic/body/div/tt \{"- topic/tt "\}\)xxx\(tt\] refers to the library name\).6.  When Terminal returns \[/dita/topic/topic/topic/topic/topic/body/div/li/tt \{"- topic/tt "\}\)dynamically linked shared library\(tt\], the library is a dynamic library.
7.  When Terminal returns \[/dita/topic/topic/topic/topic/topic/body/div/li/tt \{"- topic/tt "\}\)current ar archive random library\(tt\], the library is a static library.


**Integrate the dynamic library:**

1.  Copy the `AgoraRtcKit.framework` dynamic library from the `./libs` path of the SDK package to the `./project_name` folder in your project \(`project_name` is an example of your project name\).

2.  Open Xcode, and navigate to **TARGETS \> Project Name \> General \> Frameworks, Libraries, and Embedded Content**.

3.  Click **+ \> Add Other… \> Add Files** to add the `AgoraRtcKit.framework` dynamic library. Ensure that the **Embed** attribute of the dynamic library is **Embed & Sign**. Once the dynamic library is added, the project automatically links to other system libraries.


According to the official requirements of Apple, the Extension of an app cannot contain the dynamic library. If you need to integrate the SDK with the dynamic library in the Extension, change the file status to **Do Not Embed**.

**Integrate the static library:**

1.  Copy the `AgoraRtcKit.framework` static library from the `./libs` path of the SDK package to the `./project_name` folder in your project \(`project_name` is an example of your project name\).

2.  Open **Xcode**, navigate to **TARGETS \> Project Name \> Build Phases \> Link Binary with Libraries**, and click **+** to add the following libraries. To add the `AgoraRtcKit.framework` static library, you need to click **+**, and then click **Add Other...**.


|SDK|Library|
|---|-------|
|Voice SDK|1.  `AgoraRtcKit.framework`
2.  `Accelerate.framework`
3.  `CoreWLAN.framework`
4.  `libc++.tbd`
5.  `libresolv.9.tbd`
6.  `SystemConfiguration.framework`
|
|Video SDK|1.  `AgoraRtcKit.framework`
2.  `Accelerate.framework`
3.  `CoreWLAN.framework`
4.  `libc++.tbd`
5.  `libresolv.9.tbd`
6.  `SystemConfiguration.framework`
7.  `VideoToolbox.framework`
|

The Agora SDK uses libc++ \(LLVM\) by default. Contact support@agora.io if you want to use libstdc++ \(GNU\). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.

##### Earlier than v3.0.0 {#earlier-than-v300}

1.  Copy the `AgoraRtcEngineKit.framework` static library from the `./libs` path of the SDK package to the `./project_name` folder in your project \(`project_name` is an example of your project name\).

2.  Open **Xcode**, navigate to **TARGETS \> Project Name \> Build Phases \> Link Binary with Libraries**, and click **+** to add the following libraries. To add the `AgoraRtcEngineKit.framework` static library, you need to click **+**, and then click **Add Other...**.


|SDK|Library|
|---|-------|
|Voice SDK|1.  `AgoraRtcEngineKit.framework`
2.  `Accelerate.framework`
3.  `CoreWLAN.framework`
4.  `libc++.tbd`
5.  `libresolv.9.tbd`
6.  `SystemConfiguration.framework`
|
|Video SDK|1.  `AgoraRtcEngineKit.framework`
2.  `Accelerate.framework`
3.  `CoreWLAN.framework`
4.  `libc++.tbd`
5.  `libresolv.9.tbd`
6.  `SystemConfiguration.framework`
7.  `VideoToolbox.framework`
|

The Agora SDK uses libc++ \(LLVM\) by default. Contact support@agora.io if you want to use libstdc++ \(GNU\). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.

