# Test your app

Agora recommends running and debugging your project through a physical Android or iOS device. See [Publish to native](https://docs.cocos.com/creator/manual/en/publish/publish-native.html).

For example, to build and run an iOS project, you can do the following:

1. In Cocos Creator, click **Project** > **Build**.

1. Enter the title (such as hello_world), the platform, and the build path, and click **Build**.

1. Once the build is completed, click the **Open** button at the right of the Build Path to open the build folder.

1. Under the `./jsb-link/frameworks/runtime-src/proj.ios_mac` path, find and open your iOS project (such as `hello_world.xcworkspace`).

1. In Xcode, select your iOS device, and click the button to build and run your project. After the project runs successfully run the project, you can see your video.

1. If you want to experience a [product-name], you can also use the Agora Web sample app to interact with your iOS device. Ensure that you enter the same App ID, channel name, and temporary token in the Agora Web sample app as in your iOS project.

<p props="video">When the video call starts successfully, you can see your video and the video of other users in the app.<p>

<p props="live">When the interactive live video streaming starts successfully, hosts can see their own videos, and audience members can see the video of hosts in the app.<p>