# Implement [feature]

This section shows how to use the [sdk-name] to implement [product-name] into your app step by step.

Open `main.dart`, remove all code after the following line:

```dart
void main() => runApp(MyApp());
```

Use these steps to add the following code:


## 1: Import packages

```dart
import 'dart:async';
 
import 'package:agora_rtc_engine/rtc_engine.dart';
import 'package:agora_rtc_engine/rtc_local_view.dart' as RtcLocalView;
import 'package:agora_rtc_engine/rtc_remote_view.dart' as RtcRemoteView;
import 'package:flutter/material.dart';
import 'package:permission_handler/permission_handler.dart';
```

## 2: Define App ID and Token

```dart
/// Define App ID and Token
const APP_ID = '<Your App ID>';
const Token = '<Your Token>';
```

## 3: Define MyApp Class

```dart
class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}
```

## 4: Define app states

<p conref="conref/get-started-sample-code-flutter.dita#get-started-sample-code/video" props="video"/>
<p conref="conref/get-started-sample-code-flutter.dita#get-started-sample-code/live" props="live"/>

