# See also

This section provides additional information for your reference: 

## Sample project

Agora provides an open source sample project [OpenLive-iOS-Swift](https://github.com/AgoraIO/Basic-Video-Broadcasting/tree/master/OpenLive-iOS) on GitHub that implements interactive live video streaming for your reference.

## Other approaches to integrate the SDK{#othermethods}

In addition to integrating the [sdk-name] for <ph props="ios">iOS through Swift Package</ph><ph props="mac">macOS through CocoaPods</ph>, you can also import the SDK into your project <ph props="ios">through CocoaPods or </ph>by manually copying the SDK files.

<p props="ios">
<b>Automatically integrate the SDK with CocoaPods</b>
<ol>
<li>Install CocoaPods if you have not. See <xref href="https://guides.cocoapods.org/using/getting-started.html#getting-started" scope="external" format="html">Getting Started with CocoaPods</xref>.</li>
<li>In Terminal, navigate to the root of your project folder, and run the <codeph>pod init</codeph> command to create a <codeph>Podfile</codeph> in the project folder.</li>
<li>Open the <codeph>Podfile</codeph>, and replace all contents with the following code. Remember to replace <codeph>Your App</codeph> with the target name of your project.
<codeblock>
# platform :<ph keyref="cocoapods-platform"/>
target 'Your App' do
    pod '<ph keyref="cocoapods-library"/>'
end
</codeblock>
<li>In Terminal, run the <codeph>pod install</codeph> command to install the SDK. When the SDK is installed successfully, you can see <codeph>Pod installation complete!</codeph> in Terminal and an <codeph>xcworkspace</codeph> file in the project folder.</li>
<li>Open the <codeph>xcworkspace</codeph> file for any further steps.</li>
<p>

<b>Manually copy the SDK files</b>

1. Go to [SDK Downloads]([sdk-download]), download the latest version of the [sdk-name], and extract the files from the downloaded SDK package.

2. From the `libs` folder of the downloaded SDK package, copy the files or subfolders you need to the root of your project folder.
   
   <note type="attention">Certain files and subfolders under the <code>libs</code> folder are optional. See <a href="https://docs.agora.io/en/Voice/faq/reduce_app_size_rtc?platform=iOS#extension_libraries">extension libraries</a> for details.</note>
   
3. In Xcode, [link your target to the frameworks or libraries](https://help.apple.com/xcode/mac/current/#/dev51a648b07) you have copied. Be sure to choose **Embed & Sign** from the pop-up menu in the Embed column.

   <note type="attention">
   <ul>
   <li>Apple does not allow an app extension to contain any dynamic library. If you are integrating the Agora SDK to an app extension, choose <b>Do Not Embed</b> in the Embed column.</li>
   <li>The Agora SDK uses libc++ (LLVM) by default. Contact <xref href="mailto:support@agora.io">support@agora.io</xref> if you want to use libstdc++ (GNU). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio and video real devices.</li>
   </ul>
   </note>

## Objective-C code sample

To implement [product-name] in your app using Objective-C:

1. Replace the contents in the  `ViewController.h` file with the following:

   <p props="ios">
   <codeblock>
   #import &lt;UIKit/UIKit.h&gt;
   #import &lt;AgoraRtcKit/AgoraRtcEngineKit.h&gt;
   @interface ViewController : UIViewController &lt;AgoraRtcEngineDelegate&gt;
   @property (strong, nonatomic) AgoraRtcEngineKit *agoraKit;
   @end
   </codeblock>
   </p>
   <p props="mac">
   <codeblock>
   #import &lt;AppKit/AppKit.h&gt;
   #import &ltAgoraRtcKit/AgoraRtcEngineKit.h&gt
   @interface ViewController : NSViewController &ltAgoraRtcEngineDelegate&gt
   @property (strong, nonatomic) AgoraRtcEngineKit *agoraKit;
   @end
   </codeblock>
   </p>

2. Replace the contents in the `ViewController.m` file with the following:

   <codeblock>
   #import "ViewController.h"
   #import <ph keyref="ui-library"/>
   <p/>
   @interface ViewController ()
   @property (nonatomic, strong) <ph keyref="ui-view"/> *localView;
   @property (nonatomic, strong) <ph keyref="ui-view"/> *remoteView;
   @end
   <p/>
   @implementation ViewController
   - (void)viewDidLoad {
       [super viewDidLoad];
       [self initViews];
       [self initializeAndJoinChannel];
   }
   <p/>
   - (void)viewDidLayoutSubviews {
       [super viewDidLayoutSubviews];
       self.remoteView.frame = self.view.bounds;
       self.localView.frame = CGRectMake(self.view.bounds.size.width - 90, 0, 90, 160);
   }
   <p/>
   - (void)initViews {
       self.remoteView = [[<ph keyref="ui-view"/> alloc] init];
       [self.view addSubview:self.remoteView];
       self.localView = [[<ph keyref="ui-view"/> alloc] init];
       [self.view addSubview:self.localView];
   }
   <p/>
   - (void)initializeAndJoinChannel {
       // Pass in your App ID here
       self.agoraKit = [AgoraRtcEngineKit sharedEngineWithAppId:@"Your App ID" delegate:self];
       [self.agoraKit setChannelProfile:AgoraChannelProfileLiveBroadcasting];
       [self.agoraKit setClientRole:AgoraClientRoleBroadcaster];
       [self.agoraKit enableVideo];
       AgoraRtcVideoCanvas *videoCanvas = [[AgoraRtcVideoCanvas alloc] init];
       videoCanvas.uid = 0;
       videoCanvas.renderMode = AgoraVideoRenderModeHidden;
       videoCanvas.view = self.localView;
       [self.agoraKit setupLocalVideo:videoCanvas];
       // Pass in your token and channel name here
       [self.agoraKit joinChannelByToken:@"Your token" channelId:@"Channel name" info:nil uid:0 joinSuccess:^(NSString * _Nonnull channel, NSUInteger uid, NSInteger elapsed) {
       }];
   }
   <p/>
   - (void)rtcEngine:(AgoraRtcEngineKit *)engine didJoinedOfUid:(NSUInteger)uid elapsed:(NSInteger)elapsed {
       AgoraRtcVideoCanvas *videoCanvas = [[AgoraRtcVideoCanvas alloc] init];
       videoCanvas.uid = uid;
       videoCanvas.renderMode = AgoraVideoRenderModeHidden;
       videoCanvas.view = self.remoteView;
       [self.agoraKit setupRemoteVideo:videoCanvas];
   }
   <p/>
   - (void)viewDidDisappear:(BOOL)animated {
     [super viewDidDisappear:animated];
     [self.agoraKit leaveChannel:nil];
     [AgoraRtcEngineKit destroy];
   }
   @end
	</codeblock>

<p props="live lives">
## Listening for audience events

The [sdk-name] does not report events of an audience member in a live streaming channel. Refer to [How can I listen for an audience joining or leaving an interactive live streaming channel](https://docs.agora.io/en/Interactive%20Broadcast/faq/audience_event) if your scenario requires so.
</p>

## Integrate earlier versions of the SDK

Choose one of the following methods to integrate a version of the Agora macOS SDK earlier than v3.2.0.

### Method 1: Through CocoaPods

1. Ensure that you have installed CocoaPods before performing the following steps. See the installation guide in [Getting Started with CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started).

2. In Terminal, navigate to the project path, and run the `pod init` command to create a `Podfile` in the project folder.


3. Open the `Podfile`, delete all contents, and input the following codes. Remember to replace `Your App` with the target name of your project and replace `version` with the version of the SDK that you want to integrate. For information about the SDK version, see [Release Notes]([release-notes]).

   <codeblock>
   # platform :<ph keyref="cocoapods-platform"/>
   target 'Your App' do
       pod '<ph keyref="cocoapods-library"/>', 'version'
   end
   </codeblock>

4. Return to Terminal, and run the `pod install` command to install the Agora SDK. Once you successfully install the SDK, it shows `Pod installation complete!` in Terminal, and you can see an `xcworkspace` file in the project folder.

5. Open the generated `xcworkspace` file.

### Method 2: Through your local storage

You need to use different integration methods to integrate different versions of the SDK. Click the following version categories to expand the corresponding integration steps.

#### From v3.2.0 to v3.2.1

<% if (product == "audio") { %>
1. Copy the `AgoraRtcKit.framework`, `Agorafdkaac.framework`, and `AgoraSoundTouch.framework` dynamic libraries to the `./project_name` folder in your project (`project_name` is an example of your project name).
<% } if (product == "video") { %>
1. Copy the `AgoraRtcKit.framework`, `Agorafdkaac.framework`, `Agoraffmpeg.framework`, and `AgoraSoundTouch.framework` dynamic libraries to the `./project_name` folder in your project (`project_name` is an example of your project name).
<% } %>

2. Open Xcode, and navigate to **TARGETS > Project Name > General > Frameworks, Libraries, and Embedded Content**.

<% if (product == "audio") { %>
3. Click **+** > **Add Other…** > **Add Files** to add the `AgoraRtcKit.framework`, `Agorafdkaac.framework`, and `AgoraSoundTouch.framework` dynamic libraries. Ensure that the **Embed** attribute of these dynamic libraries is **Embed & Sign**.

   Once these dynamic libraries are added, the project automatically links to other system libraries.
<% } if (product == "video") { %>
3. Click **+** > **Add Other…** > **Add Files** to add the `AgoraRtcKit.framework`, `Agorafdkaac.framework`, `Agoraffmpeg.framework`, and `AgoraSoundTouch.framework` dynamic libraries. Ensure that the **Embed** attribute of these dynamic libraries is **Embed & Sign**.

   Once these dynamic libraries are added, the project automatically links to other system libraries.
<% } %>

  <div class="alert note"><ul><li>According to the official requirements of Apple, the Extension of an app cannot contain the dynamic library. If you need to integrate the SDK with the dynamic library in the Extension, change the file status to <b>Do Not Embed</b>.</li><li>The Agora SDK uses libc++ (LLVM) by default. Contact support@agora.io if you want to use libstdc++ (GNU). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.</li></ul></div>

#### From v3.0.1 to v3.1.2

1. Copy the `AgoraRtcKit.framework` dynamic library to the `./project_name` folder in your project (`project_name` is an example of your project name).
2. Open Xcode, and navigate to **TARGETS > Project Name > General > Frameworks, Libraries, and Embedded Content**.
3. Click **+ > Add Other… > Add Files** to add the `AgoraRtcKit.framework` dynamic library. Ensure that the **Embed** attribute of the dynamic library is **Embed & Sign**. Once the dynamic library is added, the project automatically links to other system libraries.
 
 <div class="alert note"><ul><li>According to the official requirements of Apple, the Extension of an app cannot contain the dynamic library. If you need to integrate the SDK with the dynamic library in the Extension, change the file status to <b>Do Not Embed</b>.</li><li>The Agora SDK uses libc++ (LLVM) by default. Contact support@agora.io if you want to use libstdc++ (GNU). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.</li></ul></div>

#### v3.0.0

In v3.0.0, the SDK package contains an `AgoraRtcKit.framework` dynamic library and an `AgoraRtcKit.framework` static library. Choose which of these libraries to add according to your needs.
The paths of the two libraries in the SDK package are as follows:
- The path of the dynamic library: `./Agora_Native_SDK_for_macOS_..._Dynamic/libs`.
- The path of the static library: `./Agora_Native_SDK_for_macOS_.../libs`.

<div class="alert info">If you need to check the type of a library, run the following command: <tt>file /path/xxx.framework/xxx</tt> (<tt>xxx</tt> refers to the library name).<li>When Terminal returns <tt>dynamically linked shared library</tt>, the library is a dynamic library.</li><li>When Terminal returns <tt>current ar archive random library</tt>, the library is a static library.</div>

**Integrate the dynamic library:**
1. Copy the `AgoraRtcKit.framework` dynamic library from the `./libs` path of the SDK package to the `./project_name` folder in your project (`project_name` is an example of your project name).
2. Open Xcode, and navigate to **TARGETS > Project Name > General > Frameworks, Libraries, and Embedded Content**.
3. Click **+ > Add Other… > Add Files** to add the `AgoraRtcKit.framework` dynamic library. Ensure that the **Embed** attribute of the dynamic library is **Embed & Sign**. 
 Once the dynamic library is added, the project automatically links to other system libraries.
 
 <div class="alert note">According to the official requirements of Apple, the Extension of an app cannot contain the dynamic library. If you need to integrate the SDK with the dynamic library in the Extension, change the file status to <b>Do Not Embed</b>.</div>

**Integrate the static library:**
1. Copy the `AgoraRtcKit.framework` static library from the `./libs` path of the SDK package to the `./project_name` folder in your project (`project_name` is an example of your project name).
2. Open **Xcode**, navigate to **TARGETS > Project Name > Build Phases > Link Binary with Libraries**, and click **+** to add the following libraries. To add the `AgoraRtcKit.framework` static library, you need to click **+**, and then click **Add Other...**.

| SDK | Library |
| ---------------- | ---------------- |
| Voice SDK      |<li>`AgoraRtcKit.framework`</li><li>`Accelerate.framework`</li><li>`CoreWLAN.framework`</li><li>`libc++.tbd`</li><li>`libresolv.9.tbd`</li><li>`SystemConfiguration.framework`</li>|
| Video SDK | <li>`AgoraRtcKit.framework`</li><li>`Accelerate.framework`</li><li>`CoreWLAN.framework`</li><li>`libc++.tbd`</li><li>`libresolv.9.tbd`</li><li>`SystemConfiguration.framework`</li><li>`VideoToolbox.framework`</li> |

<div class="alert note">The Agora SDK uses libc++ (LLVM) by default. Contact support@agora.io if you want to use libstdc++ (GNU). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.</div>

#### Earlier than v3.0.0

1. Copy the `AgoraRtcEngineKit.framework` static library from the `./libs` path of the SDK package to the `./project_name` folder in your project (`project_name` is an example of your project name).
2. Open **Xcode**, navigate to **TARGETS > Project Name > Build Phases > Link Binary with Libraries**, and click **+** to add the following libraries. To add the `AgoraRtcEngineKit.framework` static library, you need to click **+**, and then click **Add Other...**.

| SDK | Library |
| ---------------- | ---------------- |
| Voice SDK      |<li>`AgoraRtcEngineKit.framework`</li><li>`Accelerate.framework`</li><li>`CoreWLAN.framework`</li><li>`libc++.tbd`</li><li>`libresolv.9.tbd`</li><li>`SystemConfiguration.framework`</li>|
| Video SDK | <li>`AgoraRtcEngineKit.framework`</li><li>`Accelerate.framework`</li><li>`CoreWLAN.framework`</li><li>`libc++.tbd`</li><li>`libresolv.9.tbd`</li><li>`SystemConfiguration.framework`</li><li>`VideoToolbox.framework`</li> |

<div class="alert note">The Agora SDK uses libc++ (LLVM) by default. Contact support@agora.io if you want to use libstdc++ (GNU). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.</div>