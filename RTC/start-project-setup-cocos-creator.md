# Project setup

Use the following steps to build a project from scratch:

1. Open Cocos Creator, and navigate to New Project.

2. Choose an example (**Hello World** shown here), choose a storage path, and enter a project name.

3. Click **Create**.

4. Integrate the [sdk-name].

   1. Open a Cocos Creator project, and click **Service** on the right to open the **Service** panel.

   2. Click the <image href="https://web-cdn.agora.io/docs-files/1603983326448" format="png" scope="external" placement="inline"></image> button following **AppID**, and click **Set Cocos AppID**.

      <image href="https://web-cdn.agora.io/docs-files/1603983352672" format="png" scope="external"></image>

   3. Click the drop-down box, select a game, and click **Association** to associate your project with your game.

   4. On the **Service** panel, scroll down to find and click **Agora RTC**.

   5. Click the <image href="https://web-cdn.agora.io/docs-files/1603983397604" format="png" scope="external"  placement="inline"></image> button following **Agora RTC**, read the prompts carefully, and click **Open Service**.

   6. On the bottom of the **Agora RTC** panel, select **Video** as the SDK type, and click **Save** to integrate the Agora Cocos Creator SDK. Then, in **Node Library** > **Cloud Nodes**, you can see **AgoraVideoRender**.

After integrating the Agora [platform] SDK, Cocos Creator automatically adds codes in your project for obtaining device permissions, so you do not need to write these codes yourself.

Cocos Creator also uses your Cocos Creator account to automatically register an Agora account and create an Agora project. You can click **Dashboard** on the **Agora RTC** panel to visit Agora Console.