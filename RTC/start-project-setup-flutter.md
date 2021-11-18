# Create a Flutter project

This article uses Visual Studio Code to create a Flutter project. Before you begin, you need to install the Flutter plugin in Visual Studio Code. See [Set up an editor](https://flutter.dev/docs/get-started/editor?tab=vscode).

1. Launch Visual Studio Code. Select the **Flutter: New Project** command in **View > Command**. Enter the project name and press `<Enter>`. 
2. Select the location of the project in the pop-up window. Visual Studio Code automatically generates a simple project.

## Add dependencies

Add the following dependencies in `pubspec.yaml`:

1. Add the `agora_rtc_engine` dependency to integrate Agora Flutter SDK. See https://pub.dev/packages/agora_rtc_engine for the latest version of `agora_rtc_engine`.
2. Add the `permission_handler` dependency to add the permission handling function.

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
