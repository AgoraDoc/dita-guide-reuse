
# Integrate earlier versions of the SDK

Choose one of the following methods to integrate a version of the [platform] SDK earlier than v3.2.0.

## Method 1: Through CocoaPods

1. Ensure that you have installed CocoaPods before performing the following steps. See the installation guide in [Getting Started with CocoaPods](https://guides.cocoapods.org/using/getting-started.html#getting-started).

2. In Terminal, navigate to the project path, and run the `pod init` command to create a `Podfile` in the project folder.

3. Open the `Podfile`, delete all contents, and input the following codes. Remember to replace `Your App` with the target name of your project and replace `version` with the version of the SDK that you want to integrate. For information about the SDK version, see <ph props="video" keyref="release-notes-video-apple"/><ph props="live" keyref="release-notes-live-apple"/>.

   <p>
   <codeblock>
   # platform :<ph keyref="cocoapods-platform"/>
   target 'Your App' do
       pod '<ph keyref="cocoapods-library"/>', 'version'
   end
   </codeblock>
   </p>

4. Return to Terminal, and run the `pod install` command to install the Agora SDK. Once you successfully install the SDK, it shows `Pod installation complete!` in Terminal, and you can see an `xcworkspace` file in the project folder.

5. Open the generated `xcworkspace` file.

## Method 2: Through your local storage

You need to use different integration methods to integrate different versions of the SDK.

### From v3.2.0 to v3.2.1

1. Copy the `AgoraRtcKit.framework`, `Agorafdkaac.framework`, <ph props="video live lives">`Agoraffmpeg.framework`, </ph>and `AgoraSoundTouch.framework` dynamic libraries to the `./project_name` folder in your project (`project_name` is an example of your project name).

2. Open Xcode, and navigate to **TARGETS > Project Name > General > Frameworks, Libraries, and Embedded Content**.

3. Click **+** > **Add Other…** > **Add Files** to add the `AgoraRtcKit.framework`, `Agorafdkaac.framework`, <ph props="video live lives">`Agoraffmpeg.framework`, </ph>and `AgoraSoundTouch.framework` dynamic libraries. Ensure that the **Embed** attribute of these dynamic libraries is **Embed & Sign**.

   Once these dynamic libraries are added, the project automatically links to other system libraries.

   <note type="attention">
   <ul>
   <li>According to the official requirements of Apple, the Extension of an app cannot contain the dynamic library. If you need to integrate the SDK with the dynamic library in the Extension, change the file status to <b>Do Not Embed</b>.</li>
   <li>The Agora SDK uses libc++ (LLVM) by default. Contact support@agora.io if you want to use libstdc++ (GNU). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.</li>
   </ul>
   </note>

### From v3.0.1 to v3.1.2

1. Copy the `AgoraRtcKit.framework` dynamic library to the `./project_name` folder in your project (`project_name` is an example of your project name).
2. Open Xcode, and navigate to **TARGETS > Project Name > General > Frameworks, Libraries, and Embedded Content**.
3. Click **+ > Add Other… > Add Files** to add the `AgoraRtcKit.framework` dynamic library. Ensure that the **Embed** attribute of the dynamic library is **Embed & Sign**. Once the dynamic library is added, the project automatically links to other system libraries.
 
   <note type="attention">
   <ul>
   <li>According to the official requirements of Apple, the Extension of an app cannot contain the dynamic library. If you need to integrate the SDK with the dynamic library in the Extension, change the file status to <b>Do Not Embed</b>.</li>
   <li>The Agora SDK uses libc++ (LLVM) by default. Contact support@agora.io if you want to use libstdc++ (GNU). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.</li>
   </ul>
   </note>

### v3.0.0

In v3.0.0, the SDK package contains an `AgoraRtcKit.framework` dynamic library and an `AgoraRtcKit.framework` static library. Choose which of these libraries to add according to your needs.
The paths of the two libraries in the SDK package are as follows:

- The path of the dynamic library: `./Agora_Native_SDK_for_macOS_..._Dynamic/libs`.
- The path of the static library: `./Agora_Native_SDK_for_macOS_.../libs`.

<p>
<note type="attention">If you need to check the type of a library, run the following command: <codeph>file /path/xxx.framework/xxx</codeph> (<codeph>xxx</codeph> refers to the library name).
<ul>
<li>When Terminal returns <codeph>dynamically linked shared library</codeph>, the library is a dynamic library.</li>
<li>When Terminal returns <codeph>current ar archive random library</codeph>, the library is a static library.</li>
</ul>
</note>
</p>

#### Integrate the dynamic library

1. Copy the `AgoraRtcKit.framework` dynamic library from the `./libs` path of the SDK package to the `./project_name` folder in your project (`project_name` is an example of your project name).
2. Open Xcode, and navigate to **TARGETS > Project Name > General > Frameworks, Libraries, and Embedded Content**.
3. Click **+ > Add Other… > Add Files** to add the `AgoraRtcKit.framework` dynamic library. Ensure that the **Embed** attribute of the dynamic library is **Embed & Sign**. 
 Once the dynamic library is added, the project automatically links to other system libraries.
 
   <note type="attention">
   <ul>
   <li>According to the official requirements of Apple, the Extension of an app cannot contain the dynamic library. If you need to integrate the SDK with the dynamic library in the Extension, change the file status to <b>Do Not Embed</b>.</li>
   <li>The Agora SDK uses libc++ (LLVM) by default. Contact support@agora.io if you want to use libstdc++ (GNU). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.</li>
   </ul>
   </note>

#### Integrate the static library

1. Copy the `AgoraRtcKit.framework` static library from the `./libs` path of the SDK package to the `./project_name` folder in your project (`project_name` is an example of your project name).
2. Open **Xcode**, navigate to **TARGETS > Project Name > Build Phases > Link Binary with Libraries**, and click **+** to add the following libraries. To add the `AgoraRtcKit.framework` static library, you need to click **+**, and then click **Add Other...**.

   | SDK | Library |
   | ---------------- | ---------------- |
   | Voice SDK      |<ul><li>`AgoraRtcKit.framework`</li><li>`Accelerate.framework`</li><li>`CoreWLAN.framework`</li><li>`libc++.tbd`</li><li>`libresolv.9.tbd`</li><li>`SystemConfiguration.framework`</li></ul>|
   | Video SDK | <ul><li>`AgoraRtcKit.framework`</li><li>`Accelerate.framework`</li><li>`CoreWLAN.framework`</li><li>`libc++.tbd`</li><li>`libresolv.9.tbd`</li><li>`SystemConfiguration.framework`</li><li>`VideoToolbox.framework`</li></ul> |

   <note type="attention">The Agora SDK uses libc++ (LLVM) by default. Contact support@agora.io if you want to use libstdc++ (GNU). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.</note>

### Earlier than v3.0.0

1. Copy the `AgoraRtcEngineKit.framework` static library from the `./libs` path of the SDK package to the `./project_name` folder in your project (`project_name` is an example of your project name).
2. Open **Xcode**, navigate to **TARGETS > Project Name > Build Phases > Link Binary with Libraries**, and click **+** to add the following libraries. To add the `AgoraRtcEngineKit.framework` static library, you need to click **+**, and then click **Add Other...**.

   | SDK | Library |
   | ---------------- | ---------------- |
   | Voice SDK      |<ul><li>`AgoraRtcEngineKit.framework`</li><li>`Accelerate.framework`</li><li>`CoreWLAN.framework`</li><li>`libc++.tbd`</li><li>`libresolv.9.tbd`</li><li>`SystemConfiguration.framework`</li></ul>|
   | Video SDK | <ul><li>`AgoraRtcEngineKit.framework`</li><li>`Accelerate.framework`</li><li>`CoreWLAN.framework`</li><li>`libc++.tbd`</li><li>`libresolv.9.tbd`</li><li>`SystemConfiguration.framework`</li><li>`VideoToolbox.framework`</li></ul> |

   <note type="attention">The Agora SDK uses libc++ (LLVM) by default. Contact support@agora.io if you want to use libstdc++ (GNU). The SDK provides FAT image libraries with multi-architecture support for both 32/64-bit audio emulators and 32/64-bit audio/video real devices.</note>