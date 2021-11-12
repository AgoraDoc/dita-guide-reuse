# Interactive Live Streaming Premium Overview {#product-name-overview}

Interactive Live Streaming Premium enables one-to-many and many-to-many audio or video live streaming with the Agora Video SDK.

Interactive Live Streaming Premium is different from [Agora Video Call](https://docs.agora.io/en/Video/product_video?platform=All%20Platforms). In a video call, all users are the same role and can talk to each other freely. In an Interactive Live Streaming Premium, users can be the host or audience, where only the host can talk. For details, see this [FAQ](https://docs.agora.io/en/faq/profile_difference).

Different from the traditional CDN live broadcast, which only allows one-way communication from the hosts to the audience, the Agora Video SDK empowers the audience to interact with the hosts by [becoming a host](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#becoming-host), like a viewer jumping onto the stage in the middle of a play to perform.

## Live streaming solution comparison {#live-streaming-solution-comparison}

The following table shows the differences between Agora live streaming products and traditional CDN live streaming:

||Agora Interactive Live Streaming Premium|Agora Interactive Live Streaming Standard|Traditional CDN live streaming|
|--|----------------------------------------|-----------------------------------------|------------------------------|
|Typical scenarios|Live streaming events where the host\(s\) and audience have frequent audio and video interactions that require ultra-low latency on the audiences' clients.|Live streaming events where the host\(s\) must be able to respond quickly to the audience's text messages, comments, and virtual rewards or that require less frequent audio and video interactions.|Live streaming events that do not require audio or video interactions.|
|Latency|-   Latency from one host to another host: < 400 ms
-   Latency on the audience's client: 400 ms - 800 ms

|Latency on the audience's client: 1500 ms - 2000 ms|Latency on the audience's client: \> 3000 ms|
|Synchronization|-   Excellent synchronization between the host\(s\) and the audience
-   Excellent synchronization among the audience

|-   Good synchronization between the host\(s\) and the audience
-   Good synchronization among the audience

|-   Poor synchronization between the host\(s\) and the audience
-   Poor synchronization among the audience

|
|Interactive experience|Excellent|Good|Poor|
|Pricing|High|Moderate|Low|

**Note:**

-   A host's client refers to the client that calls an API to set the user role as host. An audience's client refers to the client that calls an API to set the user role as audience. A host can both send and receive streams, while an audience member can only receive streams.
-   The latency between host's clients in Interactive Live Streaming Standard is the same as that in Interactive Live Streaming Premium, while the latency level on the audience's client in these two products is different:
    -   Interactive Live Streaming Standard: Low latency \(1500 ms - 2000 ms\) from the host's client to the audience's client.
    -   Interactive Live Streaming Premium: Ultra-low latency \(400 ms - 800 ms\) from the host's client to the audience's client.
-   You can switch seamlessly between Interactive Live Streaming Standard and Interactive Live Streaming Premium by calling `setClientRole` and setting the latency level as `low` on the audience's client.

## Functions and scenarios {#functions-and-scenarios}

Interactive Live Streaming Premium boasts a flexible combination of functions for different scenarios.

|Function|Description|Scenario|
|--------|-----------|--------|
|Co-hosting in a channel|An audience switches to a co-host and interacts with the existing host.|-   Large-scale live streams where hosts can invite the audience to interact with them.
-   Online games such as Murder Mystery and Werewolf Killing.

|
|Single Host|Low-latency live streaming by one host. The audience can join the channel and watch the live streaming.|-   Lecture Hall
-   Showroom
-   Live-stream Shopping
-   Live match streaming
-   Trivia Game

 For the specific description of the scenarios above, see the note under the table.

|
|Becoming a host|An audience member can switch their user role and become a host. Then they can interact with other hosts via audio and video.|-|
|Co-hosting across channels|Hosts interact with each other across channels.|PK Hosting.|
|Audio mixing|Sends the local and online audio with the user's voice to other audience members in the channel.|-   Online KTV.
-   Interactive music classes for children.

|
|Screen sharing|Hosts share their screens with the audience in the channel. Supports specifying which screen or which window to share, and supports specifying the sharing region.|-   Interactive online classes.
-   Live streaming of gaming hosts.

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
|Push streams to the CDN|Sends the audio and video of your channel to other RTMP servers through the CDN:-   Starts or stops publishing at any time.
-   Adds or removes an address while continuously publishing the stream.
-   Adjusts the picture-in-picture layout.

|-   To send a live stream to WeChat or Weibo.
-   To allow more people to watch the live stream when the number of audience members in the channel reached the limit.

|
|Play the sound effect files|Enables developers to play specific sound effect files, adjust the volume, and set the playback position of the sound effect files.|Online chess or card games.|
|Voice changer and reverberation|Provides multiple presets to easily change the voice and set reverberation effects, also supports adjusting the pitch and using the equalization and reverberation modes flexibly.|-   Online KTV.
-   To change the voice in an online chatroom.

|
|Spatial sound effects|Sets the spatial sound effects for remote users to provide immersive experiences.|FPS games.|
|Enable two-channel/high-quality sound mode|Enables the two-channel and the high-quality sound mode.|-   Online music classes.
-   FM applications.

|

**Note:** See the following sample code for application scenarios:

-   [PK Hosting](https://github.com/AgoraIO/ARD-Agora-Online-PK/blob/master/README.zh.md)
-   [Trivia Game](https://github.com/AgoraIO/HQ)
-   [Online KTV](https://github.com/AgoraIO/Agora-Online-KTV/blob/master/README.zh.md)
-   [Online Voice Chatroom](https://github.com/AgoraIO-Usecase/Chatroom)
-   [Clip Doll Machine](https://github.com/AgoraIO/Wawaji)
-   [Murder Mystery Game](https://github.com/AgoraIO-Usecase/Murder-Mystery-Game)

**Note:**

-   **Lecture Hall**: In a large online class, the low latency of Interactive Live Streaming Standard allows the teacher's audio and video to be quickly synchronized to the clients of thousands of students. The low latency between the audio and video of the teacher and the courseware makes real-time teaching more effective. Students can interact with the teacher through text messages, online quizzes, and similar means to enhance the learning experience.
-   **Showroom**: In a showroom live streaming, the audience can send virtual gifts to the host. Thanks to the low latency of Interactive Live Streaming Standard, the host can promptly thank the audience for their gifts, and the audience can receive the host's thanks in a timely fashion, which helps to strengthen the emotional connection on both sides.
-   **Live-stream Shopping**: In a live-stream shopping scenario, customers can ask sellers for product and activity information via real-time messages. The low latency of Interactive Live Streaming Standard allows sellers to quickly respond to customers' questions, and customers can promptly see sellers' answers, thus encouraging transactions. In addition, the high synchronization of Interactive Live Streaming Standard ensures a consistent experience on the audiences' clients for activities such as responding to flash sales, and grabbing time-sensitive offers \(seckilling\) and coupons.
-   **Live match streaming**: Live streaming sporting events or games in real time, synchronized so that there are no problematic delays.
-   **Trivia Game**: Apart from providing a smooth live streaming experience, the high synchronization of Interactive Live Streaming Premium also ensures that players receive the questions at the same time. This gives the audience members the same amount of time to answer—and an equal chance to win.

## Key Properties {#key-properties}

|Property|Interactive Live Streaming Premium specifications|
|--------|-------------------------------------------------|
|SDK package size|4.61 MB to 13.94 MB

 3.14 MB to 11.28 MB

|
|Host capacityCapacity|17 users|
|Audience capacity|1 million users|
|Video profile|-   SDK video source: Up to 1080p @ 60 fps
-   Custom video source: Up to 4K resolution

|
|Audio profile|-   Sample rate: 16 kHz to 48 kHz
-   Support for mono and stereo sound

|
|Audio anti-packet-loss rate|80%70% \(uplink and downlink\)|
|Latency on the audience's client|400 ms to 800 ms|
|Latency between the host and the co-host|200 ms to 600 ms|

## Compatibility {#compatibility}

Interactive Live Streaming Premium is supported on platforms such as iOS, Android, Windows, macOS, Electron, Unity, and Web, and allows for cross-platform connections. The following is a list of supported platforms and their versions.

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

