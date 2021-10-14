# See also

This section provides additional information for your reference:

## Sample project

Agora provides an open source sample project <ph props="ios" keyref="start-sample-project-ios"/><ph props="mac" keyref="start-sample-project-mac"/> on GitHub that implements [feature] for your reference.

## Other approaches to integrate the SDK {#othermethods}

In addition to integrating the [sdk-name] for <ph props="ios">iOS through Swift Package</ph>
<ph props="mac">macOS through CocoaPods</ph>, you can also import the SDK into your project 
<ph props="ios">through CocoaPods or </ph>by manually copying the SDK files.

<p props="ios">
<b>Automatically integrate the SDK with CocoaPods</b>
<ol props="ios">
<li>Install CocoaPods if you have not. See <xref href="https://guides.cocoapods.org/using/getting-started.html#getting-started" scope="external" format="html">Getting Started with CocoaPods</xref>.</li>
<li>In Terminal, navigate to the root of your project folder, and run the <codeph>pod init</codeph> command to create a <codeph>Podfile</codeph> in the project folder.</li>
<li>Open the <codeph>Podfile</codeph>, and replace all contents with the following code. Remember to replace <codeph>Your App</codeph> with the target name of your project.
<codeblock props="ios">
# platform :<ph keyref="cocoapods-platform"/>
target 'Your App' do
    pod '<ph keyref="cocoapods-library"/>'
end
</codeblock>
<li>In Terminal, run the <codeph>pod install</codeph> command to install the SDK. When the SDK is installed successfully, you can see <codeph>Pod installation complete!</codeph> in Terminal and an <codeph>xcworkspace</codeph> file in the project folder.</li>
<li>Open the <codeph>xcworkspace</codeph> file for any further steps.</li>
<ol>
<p>

<b>Manually copy the SDK files</b>

1. Go to <xref href="https://docs.agora.io/en/All/downloads?platform=iOS" scope="external" format="html" props="ios">SDK Downloads</xref>
   <xref href="https://docs.agora.io/en/All/downloads?platform=macOS" scope="external" format="html" props="mac">SDK Downloads</xref>, 
   download the latest version of the [sdk-name], and extract the files from the downloaded SDK package.

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
       <ph props="live">[self.agoraKit setChannelProfile:AgoraChannelProfileLiveBroadcasting];
       [self.agoraKit setClientRole:AgoraClientRoleBroadcaster];</ph>
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