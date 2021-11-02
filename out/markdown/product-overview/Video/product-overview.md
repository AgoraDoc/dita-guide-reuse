# Video Call Overview {#product-name-overview}

Video Call enables easy and convenient one-to-one or one-to-many calls and supports voice-only and video modes with the Agora Video SDK.

## Functions and scenarios {#functions-and-scenarios}

Video Call boasts a flexible combination of functions for different scenarios.

|Function|Description|Scenario|
|--------|-----------|--------|
|Audio mixing|Sends the local and online audio with the user's voice to other audience members in the channel.|-   Online KTV.
-   Interactive music classes for children.

|
|Screen sharing|Hosts share their screens with the audience in the channel. Supports specifying which screen or which window to share, and supports specifying the sharing region.|-   Interactive online classes.

|
|Basic image enhancement|Sets basic beauty effects, including skin smoothening, whitening, and cheek blushing.|Image enhancement in a video call.|
|Modify the raw data|Developers obtain and modify the raw voice or video data of the SDK engine to create special effects, such as a voice change.|-   To change the voice in an online voice chatroom.
-   Image enhancement in a live stream.

|
|Customize the video source and renderer|Users process videos \(from self-built cameras, screen sharing, or files\) for image enhancement and filtering.|-   To use a customized image enhancement library or pre-processing library.
-   To customize the application's built-in image and video modules.
-   To use other video sources, such as a recorded video.
-   To provide flexible device management for exclusive video capture devices to avoid conflicts with other services.

|

## Key Properties {#key-properties}

|Property|Video Call specifications|
|--------|-------------------------|
|SDK package size|4.61 MB to 13.94 MB

|
|Capacity|17 users|
|Video profile|-   SDK video source: Up to 1080p @ 60 fps
-   Custom video source: Up to 4K resolution

|
|Audio profile|-   Sample rate: 16 kHz to 48 kHz
-   Support for mono and stereo sound

|
|Audio anti-packet-loss rate|80% \(uplink and downlink\)|

## Compatibility {#compatibility}

Video Call is supported on platforms such as iOS, Android, Windows, macOS, Electron, Unity, and Web, and allows for cross-platform connections. The following is a list of supported platforms and their versions.

The following is a list of supported platforms and their versions.

|Platform|Supported Version|
|--------|-----------------|
|Android|4.1+

The Android SDK supports the following ABIs:2.  armeabi-v7a
3.  arm64-v8a
4.  x86
5.  x86-64


|
|iOS|8.0+|
|Windows|Windows 7+

The Windows SDK supports the following architecture:2.  x86
3.  x86-64


|
|macOS|10.0+

The macOS SDK supports the x86 architecture.

|
|Unity|2017+

The Unity SDK supports the following platforms:

-   Android \(armeabi-v7a, arm64-v8a, x86\)
-   iOS
-   Windows \(x86, x86-64\)
-   macOS

|
|Web|See [Web SDK Compatibility](https://docs.agora.io/en/Video/web_sdk_compatibility?platform=Web)|
|Electron|Electron 1.8.3 or later|
|Flutter|Flutter 1.0.0 or later|
|React Native|React Native 0.59.10 or later|

## Reference {#reference}

[How many users can Agora RTC SDK support at the same time?](https://docs.agora.io/en/faq/capacity)

