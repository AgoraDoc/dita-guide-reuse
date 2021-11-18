# Get Started with Video Call for Flutter {#get-started-with-product-name-for-platform}

The Agora Video SDK enables you to develop rapidly to enhance your social, work, education, and IoT apps with real-time engagements.

This page shows the minimum code you need to add video call into your app by using the Agora Video SDK for Flutter.

## Understand the tech {#understand-the-tech}

The following figure shows the workflow of a video call implemented by the Agora SDK.

![Basic workflow](https://web-cdn.agora.io/docs-files/1627550978702)

To start a video call, you implement the following steps in your app:

1.  Retrieve a token
2.  Join a channel
3.  Publish and subscribe to audio and video in the channel.

For an app client to join a channel, you need the following information:

-   The App ID: A randomly generated string provided by Agora for identifying your app. You can get the App ID from [Agora Console](https://console.agora.io/).

-   The user ID: The unique identifier of a user. You need to specify the user ID yourself, and ensure that it is unique in the channel.

-   A token: In a test or production environment, your app client retrieves tokens from your server. For rapid testing, you can use a temporary token with a validity period of 24 hours.

-   The channel name: A string that identifies the channel for the video call.


## Prerequisites {#prerequisites}

Before proceeding, ensure that your development environment meets all the requirements necessary.

Agora recommends running `flutter doctor` to check whether the development environment and the deployment environment are correct.

### Development environment {#development-environment}

If your target platform is iOS, your development environment must meet the following requirements:

-   Flutter 1.0.0 or later

-   Dart 2.12.0 or later

-   macOS

-   Xcode \(latest version recommended\)


If your target platform is Android, your development environment must meet the following requirements:

-   Flutter 1.0.0 or later

-   macOS or Windows

-   Android Studio \(latest version recommended\)


### Deployment environment {#deployment-environment}

-   If your target platform is iOS, you need a physical iOS device.

-   If your target platform is Android, you need an Android simulator or a physical Android device.


### Other prerequisites {#other-prerequisites}

-   A valid [Agora account](https://console.agora.io/).
-   An active Agora project with an App ID and a temporary token. For details, see [Get Started with Agora](https://docs.agora.io/en/Agora%20Platform/get_appid_token).
-   A computer with access to the internet. If your network has a firewall, follow the instructions in [Firewall Requirements](https://docs.agora.io/en/Agora%20Platform/firewall).

## Create a Flutter project {#create-a-flutter-project}

This article uses Visual Studio Code to create a Flutter project. Before you begin, you need to install the Flutter plugin in Visual Studio Code. See [Set up an editor](https://flutter.dev/docs/get-started/editor?tab=vscode).

1.  Launch Visual Studio Code. Select the **Flutter: New Project** command in **View \> Command**. Enter the project name and press `<Enter>`.

2.  Select the location of the project in the pop-up window. Visual Studio Code automatically generates a simple project.


### Add dependencies {#add-dependencies}

Add the following dependencies in `pubspec.yaml`:

1.  Add the `agora_rtc_engine` dependency to integrate Agora Flutter SDK. See https://pub.dev/packages/agora\_rtc\_engine for the latest version of `agora_rtc_engine`.

2.  Add the `permission_handler` dependency to add the permission handling function.

    ```yaml
    environment:
      sdk: ">=2.12.0 <3.0.0"
    
    dependencies:
      flutter:
        sdk: flutter
    
    
      # The following adds the Cupertino Icons font to your application.
      # Use with the CupertinoIcons class for iOS style icons.
      cupertino_icons: ^0.1.3
      # Please use the latest version of agora_rtc_engine
      agora_rtc_engine: ^4.0.5
      permission_handler: ^6.0.0
    ```


Because JCenter is about to retire, Agora publishes the RTC Android SDK package to JitPack instead of JCenter. Therefore, to build an Android application, add the following line in the `/android/build.gradle` file of your project:

```java
allprojects {
    repositories {
        ...
        maven { url 'https://www.jitpack.io' }
    }
}
```

## Implement video call {#implement-feature}

This section shows how to use the Agora Video SDK to implement Video Call into your app step by step.

Open `main.dart`, remove all code after the following line:

```dart
void main() => runApp(MyApp());
```

Use these steps to add the following code:

### 1: Import packages {#1-import-packages}

```dart
import 'dart:async';
 
import 'package:agora_rtc_engine/rtc_engine.dart';
import 'package:agora_rtc_engine/rtc_local_view.dart' as RtcLocalView;
import 'package:agora_rtc_engine/rtc_remote_view.dart' as RtcRemoteView;
import 'package:flutter/material.dart';
import 'package:permission_handler/permission_handler.dart';
```

### 2: Define App ID and Token {#2-define-app-id-and-token}

```dart
/// Define App ID and Token
const APP_ID = '<Your App ID>';
const Token = '<Your Token>';
```

### 3: Define MyApp Class {#3-define-myapp-class}

```dart
class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}
```

### 4: Define app states {#4-define-app-states}

```language-dart
// App state class
class _MyAppState extends State<MyApp> {
  bool _joined = false;
  int _remoteUid = 0;
  bool _switch = false;

  @override
  void initState() {
    super.initState();
    initPlatformState();
  }

  // Init the app
  Future<void> initPlatformState() async {
    await [Permission.camera, Permission.microphone].request();

    // Create RTC client instance
    RtcEngineContext context = RtcEngineContext(APP_ID);
    var engine = await RtcEngine.createWithContext(context);
    // Define event handling logic
    engine.setEventHandler(RtcEngineEventHandler(
        joinChannelSuccess: (String channel, int uid, int elapsed) {
          print('joinChannelSuccess ${channel} ${uid}');
          setState(() {
            _joined = true;
          });
        }, userJoined: (int uid, int elapsed) {
      print('userJoined ${uid}');
      setState(() {
        _remoteUid = uid;
      });
    }, userOffline: (int uid, UserOfflineReason reason) {
      print('userOffline ${uid}');
      setState(() {
        _remoteUid = 0;
      });
    }));
    // Enable video
    await engine.enableVideo();
    // Join channel with channel name as 123
    await engine.joinChannel(Token, '123', null, 0);
  }


  // Build UI
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Flutter example app'),
        ),
        body: Stack(
          children: [
            Center(
              child: _switch ? _renderRemoteVideo() : _renderLocalPreview(),
            ),
            Align(
              alignment: Alignment.topLeft,
              child: Container(
                width: 100,
                height: 100,
                color: Colors.blue,
                child: GestureDetector(
                  onTap: () {
                    setState(() {
                      _switch = !_switch;
                    });
                  },
                  child: Center(
                    child:
                    _switch ? _renderLocalPreview() : _renderRemoteVideo(),
                  ),
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }

  // Local preview
  Widget _renderLocalPreview() {
    if (_joined) {
      return RtcLocalView.SurfaceView();
    } else {
      return Text(
        'Please join channel first',
        textAlign: TextAlign.center,
      );
    }
  }

  // Remote preview
 Widget _renderRemoteVideo() {
    if (_remoteUid != 0) {
      return RtcRemoteView.SurfaceView(
        uid: _remoteUid,
        channelId: "123",
      );
    } else {
      return Text(
        'Please wait remote user join',
        textAlign: TextAlign.center,
      );
    }
  }

}   
```

## Run the project {#run-the-project}

1.  Run the following command in the root folder to install dependencies.

    ```shell
    flutter packages get
    ```

2.  Run the project.

    ```shell
    flutter run
    ```


## Next steps {#next-steps}

Generating a token by hand is not helpful in a production context. [Authenticate Your Users with Tokens](https://docs.agora.io/en/Video/token_server?platform=Android) shows you how to start the video call with a token that you retrieve from your server.

## Common issues {#common-issues}

If the deployment environment is Android, users in mainland China may get stuck in `Running Gradle task 'assembleDebug'...` or see the following error:

```
Running Gradle task 'assembleDebug'...
Exception in thread "main" java.net.ConnectException: Connection timed out: connect
```

Take the following steps to resolve this issue:

1.  In the `build.gradle` file of the Android project, use mirrors in China for `google` and `jcenter.`

    ```java
    buildscript {
        ext.kotlin_version = '1.3.50'
        repositories {
            // google()
            // jcenter()
            maven { url 'https://maven.aliyun.com/repository/google' }
            maven { url 'https://maven.aliyun.com/repository/public' }
        }
    
    ...
    
    allprojects {
        repositories {
            // google()
            // jcenter()
            maven { url 'https://maven.aliyun.com/repository/google' }
            maven { url 'https://maven.aliyun.com/repository/public' }
        }
    }
    ```

2.  In the `gradle-wrapper.properties` file, set `distributionUrl` to a local file. For example, for gradle 5.6.4, you can copy `gradle-5.6.4-all.zip` to `gradle/wrapper` and set `distributionUrl` to:

    ```
    distributionUrl=gradle-5.6.4-all.zip
    ```


