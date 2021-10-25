# Set the channel profile
After initializing the Agora engine, call `setChannelProfile` to set the channel profile as `LIVE_BROADCASTING`.
If you want to switch to another profile, destroy the current Agora engine with the **release** method, and reinitialize the Agora engine before calling `setChannelProfile`.

```
    cc.Class({
    ...

    onLoad: function () {

    ...

    // Set the channel profile as interactive live streaming.
    agora.setChannelProfile(agora.CHANNEL_PROFILE_TYPE.CHANNEL_PROFILE_LIVE_BROADCASTING);
    },
    ...
    });
```

# Set the client role

An interactive streaming channel has two client roles: BROADCASTER and AUDIENCE; the default role is AUDIENCE.

After setting the channel profile to `LIVE_BROADCASTING`, your app may use the following steps to set the client role: 

1. In Cocos Creator, navigate to **Node Library** > **Builtin Nodes**, and add **Toggle Group** to the **Scene** panel. You can adjust the display position of toggle components as needed.

2. In the **Node Tree** panel, you can see that the **toggleContainer** contains three toggle components. You need to delete one redundant toggle, for example, **toggle3**.
    
    >In this section, designate **toggle1** as the option for audience, and **toggle2** as the option for host.

3. In `HelloWorld.js`, call `setClientRole`, and pass in the client role set by the user.
     ```
    cc.Class({
      
          ...
      
          setClientRole: function (toggle, customEventData) {
              // Set the client role. A user chooses a client role and the toggle component passes it through customEventData.
              // If the customEventData value is 1, the client role is set to audience, and the corresponding enumerator is agora
              CLIENT_ROLE_TYPE.CLIENT_ROLE_AUDIENCE.
              // If the customEventData value is 2, the client role is set to host, and the corresponding enumerator is agora.CLIENT_ROLE_TYPE
              CLIENT_ROLE_BROADCASTER.
              agora.setClientRole(customEventData);
          },
      
          ...
      });
   ```

4. Return to Cocos Creator, and click **toggle1**. In the **Properties** panel, find **Toggle** > **Check Events**, and add a check event.<span id="step4">

5. On the **Node Tree** panel, find and add the **Canvas** to the **cc.Node** property box of the check event. In the two drop-down boxes following the **cc.Node** property box, select the scene (**HelloWorld**) and the check event (**setClientRole**) in turn, and enter **1** in **CustomEventData**.<span id="step5">

6. Click **Add Component** > **Renderer Component** > **Label** to add a label for **toggle1**. navigate to **Label** > **String**, and enter **Audience**.

7. Repeat step 3 step 4, and step 5 to configure the **toggle2**. Note that you need to enter **2** in **CustomEventData** and **Host** in **Label** > **String**.

During the [feature], only the host can be seen. If you want to switch the client role after joining the channel, call the `setClientRole` method.