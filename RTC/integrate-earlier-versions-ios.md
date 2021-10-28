
# Integrate earlier versions of the SDK

Choose one of the following methods to integrate a version of the [platform] SDK earlier than v3.2.0.

## Method 1: Through CocoaPods

<ol>
<li>Ensure that you have installed CocoaPods before performing the following steps. See the installation guide in <xref href="https://guides.cocoapods.org/using/getting-started.html#getting-started" scope="external" format="html">Getting Started with CocoaPods</xref>.</li>
<li>In Terminal, navigate to the root of your project folder, and run the <codeph>pod init</codeph> command to create a <codeph>Podfile</codeph> in the project folder.</li>
<li>Open the <codeph>Podfile</codeph>, and replace all contents with the following code. Remember to replace <codeph>Your App</codeph> with the target name of your project and replace <codeph>version</codeph> with the version of the SDK that you want to integrate. For information about the SDK version, see <xref keyref="release-notes-ios" props="ios"></xref>.
<p props="video live" conref="conref/get-started-sample-code-apple.dita#get-started-sample-code/video-cocoapods-ios"></p>
<p props="audio" conref="conref/get-started-sample-code-apple.dita#get-started-sample-code/audio-cocoapods-ios"></p>
</li>
<li>In Terminal, run the <codeph>pod install</codeph> command to install the SDK. When the SDK is installed successfully, you can see <codeph>Pod installation complete!</codeph> in Terminal and an <codeph>xcworkspace</codeph> file in the project folder.</li>
<li>Open the <codeph>xcworkspace</codeph> file for any further steps.</li>
</ol>

## Method 2: Through your local storage

You need to use different integration methods to integrate different versions of the SDK.

### From v3.2.0 to v3.2.1

1. According to your requirements, choose one of the following methods to copy the `AgoraRtcKit.framework`, `Agorafdkaac.framework`, <ph props="video live lives">`Agoraffmpeg.framework`, </ph>and `AgoraSoundTouch.framework` dynamic libraries to the `./project_name` folder in your project (`project_name` is an example of your project name):

   <ol>
   <li>If you do not need to use a simulator to run the project, copy the above dynamic libraries under the path of <codeph>./libs</codeph> in the SDK package.</li>
   <li>If you need to use a simulator to run the project, copy the above dynamic libraries under the path of <codeph>./libs/ALL_ARCHITECTURE</codeph> in the SDK package. The dynamic libraries under this path contains the x86-64 architecture, you need to remove the x86-64 architecture in the libraries before uploading the app to the App Store.
   In Terminal, run the following command to remove the x86-64 architecture. Remember to replace <codeph>ALL_ARCHITECTURE/AgoraRtcKit.framework/AgoraRtcKit</codeph> with the path of the dynamic library in your project.
   <codeblock>lipo -remove x86-64 ALL_ARCHITECTURE/AgoraRtcKit.framework/AgoraRtcKit -output ALL_ARCHITECTURE/AgoraRtcKit.framework/AgoraRtcKit</codeblock>
   </li>
   </ol>
   
2. Open Xcode, and navigate to **TARGETS &gt; Project Name &gt; General &gt; Frameworks, Libraries, and Embedded Content**.

3. Click **+** &gt; **Add Other…** &gt; **Add Files** to add the `AgoraRtcKit.framework`, `Agorafdkaac.framework`, <ph props="video live lives">`Agoraffmpeg.framework`, </ph>and `AgoraSoundTouch.framework` dynamic libraries. Ensure that the **Embed** attribute of these dynamic libraries is **Embed & Sign**.

   Once these dynamic libraries are added, the project automatically links to other system libraries.

   <note type="attention">
   <ul>
   <li>According to the official requirements of Apple, the Extension of an app cannot contain the dynamic library. If you need to integrate the SDK with the dynamic library in the Extension, change the file status to <b>Do Not Embed</b>.</li>
   <li>The Agora SDK uses libc++ (LLVM) by default. Contact support@agora.io if you want to use libstdc++ (GNU). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.</li>
   </ul>
   </note>

### From v3.0.1 to v3.1.2

1. According to your requirements, choose one of the following methods to copy the `AgoraRtcKit.framework` dynamic library to the `./project_name` folder in your project (`project_name` is an example of your project name):

   <ol>
   <li>If you do not need to use a simulator to run the project, copy the above dynamic library under the path of <codeph>./libs</codeph> in the SDK package.</li>
   <li>If you need to use a simulator to run the project, copy the above dynamic library under the path of <codeph>./libs/ALL_ARCHITECTURE</codeph> in the SDK package. The dynamic library under this path contains the x86-64 architecture, you need to remove the x86-64 architecture in the library before uploading the app to the App Store.
   In Terminal, run the following command to remove the x86-64 architecture. Remember to replace <codeph>ALL_ARCHITECTURE/AgoraRtcKit.framework/AgoraRtcKit</codeph> with the path of the dynamic library in your project.
   <codeblock>lipo -remove x86-64 ALL_ARCHITECTURE/AgoraRtcKit.framework/AgoraRtcKit -output ALL_ARCHITECTURE/AgoraRtcKit.framework/AgoraRtcKit</codeblock>
   </li>
   </ol>
   
2. Open Xcode, and navigate to **TARGETS &gt; Project Name &gt; General &gt; Frameworks, Libraries, and Embedded Content**.
3. Click **+ &gt; Add Other… &gt; Add Files** to add the `AgoraRtcKit.framework` dynamic library. Ensure that the **Embed** attribute of the dynamic library is **Embed & Sign**. Once the dynamic library is added, the project automatically links to other system libraries.
 
   <note type="attention">
   <ul>
   <li>According to the official requirements of Apple, the Extension of an app cannot contain the dynamic library. If you need to integrate the SDK with the dynamic library in the Extension, change the file status to <b>Do Not Embed</b>.</li>
   <li>The Agora SDK uses libc++ (LLVM) by default. Contact support@agora.io if you want to use libstdc++ (GNU). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.</li>
   </ul>
   </note>

### v3.0.0

In v3.0.0, the SDK package contains an `AgoraRtcKit.framework` dynamic library and an `AgoraRtcKit.framework` static library. Choose which of these libraries to add according to your needs.
The paths of the two libraries in the SDK package are as follows:

- The path of the dynamic library: `./Agora_Native_SDK_for_iOS_..._Dynamic/libs`.
- The path of the static library: `./Agora_Native_SDK_for_iOS_.../libs`.

<note type="attention">If you need to check the type of a library, run the following command: <codeph>file /path/xxx.framework/xxx</codeph> (<codeph>xxx</codeph> refers to the library name).
<ul>
<li>When Terminal returns <codeph>dynamically linked shared library</codeph>, the library is a dynamic library.</li>
<li>When Terminal returns <codeph>current ar archive random library</codeph>, the library is a static library.</li>
</ul>
</note>

#### Integrate the dynamic library

1. Copy the `AgoraRtcKit.framework` dynamic library from the `./libs` path of the SDK package to the `./project_name` folder in your project (`project_name` is an example of your project name).
2. Open Xcode, and navigate to **TARGETS &gt; Project Name &gt; General &gt; Frameworks, Libraries, and Embedded Content**.
3. Click **+ &gt; Add Other… &gt; Add Files** to add the `AgoraRtcKit.framework` dynamic library. Ensure that the **Embed** attribute of the dynamic library is **Embed & Sign**. 
 Once the dynamic library is added, the project automatically links to other system libraries.
 
   <note type="attention">
   <ul>
   <li>According to the official requirements of Apple, the Extension of an app cannot contain the dynamic library. If you need to integrate the SDK with the dynamic library in the Extension, change the file status to <b>Do Not Embed</b>.</li>
   <li>The Agora SDK uses libc++ (LLVM) by default. Contact support@agora.io if you want to use libstdc++ (GNU). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.</li>
   </ul>
   </note>

#### Integrate the static library

1. Copy the `AgoraRtcKit.framework` static library from the `./libs` path of the SDK package to the `./project_name` folder in your project (`project_name` is an example of your project name).
2. Open **Xcode**, navigate to **TARGETS &gt; Project Name &gt; Build Phases &gt; Link Binary with Libraries**, and click **+** to add the following libraries. To add the `AgoraRtcKit.framework` static library, you need to click **+**, and then click **Add Other...**.

   | SDK | Library |
   | ---------------- | ---------------- |
   | Voice SDK      |<ul><li>`AgoraRtcKit.framework`</li><li>`Accelerate.framework`</li><li>`AudioToolbox.framework`</li><li>`AVFoundation.framework`</li><li>`CoreMedia.framework`</li><li>`libc++.tbd`</li><li>`libresolv.tbd`</li><li>`SystemConfiguration.framework`</li><li>`CoreTelephony.framework`</li></ul>|
   | Video SDK | <ul><li>`AgoraRtcKit.framework`</li><li>`Accelerate.framework`</li><li>`AudioToolbox.framework`</li><li>`AVFoundation.framework`</li><li>`CoreMedia.framework`</li><li>`libc++.tbd`</li><li>`libresolv.tbd`</li><li>`SystemConfiguration.framework`</li><li>`CoreML.framework`</li><li>`VideoToolbox.framework`</li></ul> |

   <note type="attention">The Agora SDK uses libc++ (LLVM) by default. Contact support@agora.io if you want to use libstdc++ (GNU). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.</note>


### Earlier than v3.0.0

1. Copy the `AgoraRtcEngineKit.framework` static library from the `./libs` path of the SDK package to the `./project_name` folder in your project (`project_name` is an example of your project name).
2. Open **Xcode**, navigate to **TARGETS &gt; Project Name &gt; Build Phases &gt; Link Binary with Libraries**, and click **+** to add the following libraries. To add the `AgoraRtcEngineKit.framework` static library, you need to click **+**, and then click **Add Other...**.

   | SDK | Library |
   | ---------------- | ---------------- |
   | Voice SDK      |<ul><li>`AgoraRtcEngineKit.framework`</li><li>`Accelerate.framework`</li><li>`AudioToolbox.framework`</li><li>`AVFoundation.framework`</li><li>`CoreMedia.framework`</li><li>`libc++.tbd`</li><li>`libresolv.tbd`</li><li>`SystemConfiguration.framework`</li><li>`CoreTelephony.framework`</li></ul>|
   | Video SDK | <ul><li>`AgoraRtcEngineKit.framework`</li><li>`Accelerate.framework`</li><li>`AudioToolbox.framework`</li><li>`AVFoundation.framework`</li><li>`CoreMedia.framework`</li><li>`libc++.tbd`</li><li>`libresolv.tbd`</li><li>`SystemConfiguration.framework`</li><li>`CoreML.framework`</li><li>`VideoToolbox.framework`</li></ul> |

   <note type="attention">The Agora SDK uses libc++ (LLVM) by default. Contact support@agora.io if you want to use libstdc++ (GNU). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.</note>