# Test your app

Before testing your app, you need to install and run your project on a device first.Agora recommends you run this project on a physical mobile device, as some simulators may not support the full features of this project.

**To run the Android app on a physical device:**

1. Enable **Developer options** on your Android device, and then connect it to your computer using a USB cable.
2. Run `npx react-native run-android` in the project root directory.

**To run the iOS app on a physical device:**

1. Open the `ProjectName/ios/ProjectName.xcworkspace` folder with Xcode.
2. Connect your iOS device to your computer using a USB cable.
3. Click the **Build and run** button in Xcode.

A moment later you will see the project installed on your device. Then, take the following steps to test the [feature] app:

1. Grant microphone and camera access to your app.
2. When the app launches, you should be able to see yourself on the local view.
3. Ask a friend to join the video call with you on the [demo app](https://webdemo.agora.io/basicVideoCall/index.html). Enter the same App ID and channel name.
4. After your friend joins the channel, you should be able to see and hear each other.
