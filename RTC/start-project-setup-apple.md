# Project setup

For new projects, in **Xcode**, follow the steps to create the environment necessary to add live streaming into your app.

1. Create a new [platform] app and configure the following settings:
   - **Product Name**: Any name you like.
   - **Team**: If you have added a team, choose it from the pop-up menu. If not, you can see the **Add account** button. Click it, input your Apple ID, and click **Next** to add your team.
   - **Organization Identifier**: The identifier of your organization. If you do not belong to an organization, use any identifier you like.
   - **Interface**: Choose **Storyboard**.
   - **Language**: Choose **Swift**.

2. Integrate the [sdk-name] into your project:

   <p props="ios">Go to <b>File</b> > <b>Swift Packages</b> > <b>Add Package Dependencies...</b>, and paste the following URL:
   <codeph>https://github.com/AgoraIO/AgoraRtcEngine_iOS</codeph>. In <b>Choose Package Options</b>, specify the <ph keyref="sdk-name"/> version you want to use. You need to fill in x.y.z for version x.y.z and x.y.z-r.a for version x.y.z.a. For example, fill in 3.5.0 for version 3.5.0 and 3.5.0-r.2 for version 3.5.0.2.
   </p>
   <note props="ios" type="attention">
   <ul>
   <li>For the <ph keyref="sdk-name"/>, Agora provides Swift Packages for 3.4.3 or later versions.</li>
   <li>If you have issues installing this Swift Package, try going to <b>File</b> > <b>Swift Packages</b> > <b>Reset Package Caches</b>.</li>
   <li>For more integration methods, see <xref href="start-see-also-mac.md#othermethods">Other approaches to integrate the SDK</xref>.</li>
   </ul>
   </note>
   
   <ol props="mac">
   <li>Install CocoaPods if you have not. See <xref href="https://guides.cocoapods.org/using/getting-started.html#getting-started" scope="external" format="html">Getting Started with CocoaPods</xref>.</li>
   <li>In Terminal, navigate to the root of your project folder, and run the <codeph>pod init</codeph> command to create a <codeph>Podfile</codeph> in the project folder.</li>
   <li>Open the <codeph>Podfile</codeph>, and replace all contents with the following code. Remember to replace <codeph>Your App</codeph> with the target name of your project.</li>
   <codeblock>
   # platform :macos, '10.11'
   target 'Your App' do
       pod 'AgoraRtcEngine_macOS'
   end
   </codeblock>
   <note props="mac" type="attention">For more integration methods, see <xref href="start-see-also-mac.md#othermethods">Other approaches to integrate the SDK</xref>.</note>
   <li>In Terminal, run the <codeph>pod install</codeph> command to install the SDK. When the SDK is installed successfully, you can see  <codeph>Pod installation complete!</codeph> in Terminal and an <codeph>xcworkspace</codeph> file in the project folder.</li>
   <li>Open the <codeph>xcworkspace</codeph> file for any further steps.</li>
   </ol>


3. [Enable automatic signing](https://help.apple.com/xcode/mac/current/#/dev23aab79b4) for your project.
4. Set the deployment target for your app:
   1. In the [project editor](https://help.apple.com/xcode/mac/current/#/devdab46c612), choose your target and click **General**.
   2. In the **Deployment Info** settings, choose the operating system version for your [platform] app from the pop-up menu.
5. Add permissions for microphone and camera usage.
   
   In the [Project navigator](https://help.apple.com/xcode/mac/current/#/dev7d7220fbb), open the `info.plist` file of your project and [add the following properties](https://help.apple.com/xcode/mac/current/#/dev3f399a2a6):
   - Privacy - Microphone Usage Description
   - Privacy - Camera Usage Description
   
   <p props="mac">Navigate to <b>TARGETS > Project Name > Signing & Capabilities</b>, and add the following permissions in <b>App Sandbox</b> and <b>Hardened Runtime</b>:
   <ul props="mac">
   <li>App Sandbox > Network: Incoming Connections (Server) and Outgoing Connections (Client)</li>
   <li>App Sandbox > Hardware: Camera and Audio Input</li>
   <li>Hardened Runtime > Resource Access: Camera and Audio Input</li>
   </p>