# Get Started with Video Call for Cocos2d-x {#get-started-with-product-name-for-platform}

The Agora Video SDK enables you to develop rapidly to enhance your social, work, education, and IoT apps with real-time engagements.

This page shows the minimum code you need to add video call into your app by using the Agora Video SDK for Cocos2d-x.

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

Before proceeding, ensure that your development environment meets the following requirements:

-   Cocos2d-x 3.0 or later.

-   Python 2.7.5 or later. \(Cocos2d-x does not support Python 3 or later.\)

-   Android Studio 3.0 or later.

-   NDK r18b or later.

-   A mobile device running Android 4.1 or later.


-   A valid [Agora account](https://console.agora.io/).

-   An active Agora project with an App ID and a temporary token. For details, see [Get Started with Agora](https://docs.agora.io/en/Agora%20Platform/get_appid_token).

-   A computer with access to the internet. If your network has a firewall, follow the instructions in [Firewall Requirements](https://docs.agora.io/en/Agora%20Platform/firewall).


## Project setup {#project-setup}

In order to create the environment necessary to integrate Video Call into your app, do the following.

1.  For new projects, download [Cocos2d-x](https://www.cocos.com/products#Cocos2d-x) and [create a new project using the `cocos new` command](https://docs.cocos2d-x.org/cocos2d-x/v4/en/editors_and_tools/cocosCLTool.html#creating-a-new-project).

2.  Integrate the Agora Video SDK into your project:

    1.  Go to [SDK downloads](https://docs.agora.io/en/Video/downloads?platform=Cocos2d-x), download the latest version of the Agora Video SDK for Android, and extract the files from the downloaded SDK package.

    2.  Create an `sdk` folder in your project root directory for the Agora Video SDK. You can create the folder and add subfolders in the following structure:

        ```xml
        └─sdk
           ├─android
           │  ├─agora
           │  └─lib
           └─ios
               └─agora
        ```

    3.  Copy the following files and folders from the `video/libs/android` dolder to your project:

        |File or folder|Path in your peoject|
        |--------------|--------------------|
        |`agora-rtc-sdk.jar` file|`<project_root>/sdk/android/lib`|
        |`arm64-v8a` folder|`<project_root>/sdk/android/agora`|
        |`armeabi-v7a` folder|`<project_root>/sdk/android/agora`|
        |`include` folder|`<project_root>/sdk/android/agora`|
        |`x86_64` folder|`<project_root>/sdk/android/agora`|
        |`x86` folder|`<project_root>/sdk/android/agora`|

    4.  Add the configuration of the `.so` libraries to the build files with the following steps:

        Add the following code to the `CMakeLists.txt` file in the project root directory:

        ```java
        # add cross-platforms source files and header files
          list(APPEND GAME_SOURCE
               Classes/AppDelegate.cpp
               Classes/HelloWorldScene.cpp
               // Add the source file VideoFrameObserver.cpp.
               Classes/VideoFrameObserver.cpp
               )
          list(APPEND GAME_HEADER
               Classes/AppDelegate.h
               Classes/HelloWorldScene.h
               )
          ...
          else()
              add_library(${APP_NAME} SHARED ${all_code_files})
              add_subdirectory(${COCOS2DX_ROOT_PATH}/cocos/platform/android ${ENGINE_BINARY_PATH}/cocos/platform)
              target_link_libraries(${APP_NAME} -Wl,--whole-archive cpp_android_spec -Wl,--no-whole-archive)
              // Add the following four lines:
              add_library(agora-rtc-sdk SHARED IMPORTED)
              set_target_properties(agora-rtc-sdk PROPERTIES IMPORTED_LOCATION ${CMAKE_CURRENT_SOURCE_DIR}/sdk/android/agora/${ANDROID_ABI}/libagora-rtc-sdk.so)
              include_directories(${CMAKE_CURRENT_SOURCE_DIR}/sdk/android/agora/include/)
              target_link_libraries(${APP_NAME} agora-rtc-sdk)
          endif()
        ```

        Add the following code after `assets.srcDir "../../Resources"` in the `proj.android/app/build.gradle` file to include the `.so` libraries:

        ```xml
        sourceSets.main {
             java.srcDir "src"
             res.srcDir "res"
             manifest.srcFile "AndroidManifest.xml"
             assets.srcDir "../../Resources"
             // Add the following two lines to include the .so libraries:
             if (PROP_BUILD_TYPE == 'cmake') {
                 jniLibs.srcDirs "../../sdk/android/agora"
             }
          }
          ...
          dependencies {
          // Add the following dependencies:
          implementation fileTree(dir: '../../sdk/android/lib', include: ['*.jar'])
          implementation project(':libcocos2dx')
          implementation 'androidx.annotation:annotation:1.1.0'
          implementation 'androidx.appcompat:appcompat:1.2.0'
          }
        ```

3.  Add project permissions.

    Add the following permissions in the `./proj.android/app/AndroidManifest.xml` file for device access according to your needs:

    ```java
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
     package="io.agora.gaming.cocos2d"
     android:installLocation="auto">
    
         <uses-permission android:name="android.permission.INTERNET"/>
         <uses-permission android:name="android.permission.READ_PHONE_STATE" />
         <uses-permission android:name="android.permission.INTERNET" />
         <uses-permission android:name="android.permission.RECORD_AUDIO" />
         <uses-permission android:name="android.permission.CAMERA" />
         <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
         <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
         <uses-permission android:name="android.permission.BLUETOOTH" />
         <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
         // Add the following permission if your scenario involves reading the external storage:
         <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
         // For devices running Android 10.0 or later, you also need to add the following permission:
         <uses-permission android:name="android.permission.READ_PRIVILEGED_PHONE_STATE" />
    
     ...
     </manifest>
    ```

4.  Get the device permission.

    Add the following code in `./proj.android/app/src/org/cocos2dx/cpp/AppActivity.java` to access the microphone and camera of the Android device when launching the activity:

    ```java
    // Add the import of the following files:
     import android.Manifest;
     import android.app.Activity;
     import android.content.pm.PackageManager;
     import android.widget.Toast;
     import androidx.annotation.NonNull;
     import androidx.core.content.ContextCompat;
     import java.util.ArrayList;
     import java.util.List;
    
     public class AppActivity extends Cocos2dxActivity {
         // Load the agora-rtc-sdk .so library.
         static {
             System.loadLibrary("agora-rtc-sdk");
         }
    
         private final static int REQUEST_CODE = 200;
    
         @Override
         protected void onCreate(Bundle savedInstanceState) {
             super.setEnableVirtualButton(false);
             super.onCreate(savedInstanceState);
    
             // Ask for Android device permissions at runtime.
             String[] needPermissions = {Manifest.permission.RECORD_AUDIO, Manifest.permission.CAMERA};
             checkAndRequestAppPermission(this, needPermissions);
         }
    
         private void checkAndRequestAppPermission(@NonNull Activity activity, String[] permissions) {
             if (Build.VERSION.SDK_INT < Build.VERSION_CODES.M) return;
             List<String> permissionList = new ArrayList<>();
             for (String permission : permissions) {
                 if (ContextCompat.checkSelfPermission(activity, permission) != PackageManager.PERMISSION_GRANTED)
                     permissionList.add(permission);
             }
             if (permissionList.size() == 0) return;
             String[] requestPermissions = permissionList.toArray(new String[0]);
             activity.requestPermissions(requestPermissions, AppActivity.REQUEST_CODE);
         }
    
         @Override
         public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
             if (requestCode == REQUEST_CODE) {
                 StringBuilder builder = new StringBuilder();
                 for (int i = 0; i < grantResults.length; i++) {
                     if (grantResults[i] != PackageManager.PERMISSION_GRANTED) {
                         builder.append(permissions[i]);
                         builder.append(" ");
                     }
                 }
                 if (builder.length() > 0) {
                     builder.append("not permitted!");
                     Toast.makeText(this, builder.toString(), Toast.LENGTH_SHORT).show();
                 }
             } else {
                 super.onRequestPermissionsResult(requestCode, permissions, grantResults);
             }
         }
     }
    ```


## Implement a Video Call client {#implement-a-product-name-client}

This section shows how to use the Agora Video SDK to implement Video Call into your app step by step.

Open your project file editor and create files in the following structure:

```xml
├─Classes
│ ...
│  │  HelloWorldScene.cpp
│  │  HelloWorldScene.h
│  │  VideoFrameObserver.cpp
│  │  VideoFrameObserver.h
│  │
```

### Import classes and add declaration {#import-classes-and-add-declaration}

In `HelloWorldScene.h`, add the following code import the Agora classes, declare variables, and add function declarations.

```java
// Import the AgoraRtcKit and IAgoraRtcEngine classes.
#ifdef __APPLE__
#include "AgoraRtcKit.h"
#elif __ANDROID__
#include "IAgoraRtcEngine.h"
#endif
// Import the following header files and map container.
#include "VideoFrameObserver.h"
#include "ui/CocosGUI.h"
#include <map>


// Replace <#YOUR APP ID#> with the App ID of your Agora project and add quotation marks, such as "xxxxxx".
#define AGORA_APP_ID <#YOUR APP ID#>
// Replace <#YOUR TOKEN#> with your token and add quotation marks, such as "xxxxxxx".
#define AGORA_TOKEN <#YOUR TOKEN#>


// Add function declarations
class HelloWorld : public cocos2d::Scene {
public:
static cocos2d::Scene *createScene();
bool init() override;
// Occur when a user enters the HelloWorld scene.
void onEnter() override;
// Occur when a user exits the HelloWorld scene.
void onExit() override;
// Define the update() method, which will be called by the scheduleUpdate() method.
void update(float delta) override;
...
public:
// Create and initialize the video view of a specified user.
void addTextureRender(uid_t uid, int width, int height);
// Remove the video view of a specified user.
void removeTextureRender(uid_t uid);
// Remove the video views of all users.
void removeAllTextureRenders();
private:
// Occur when a user clicks the join-channel button.
void onJoinChannelClicked();
// Occur when a user clicks the leave-channel button.
void onLeaveChannelClicked();
private:
cocos2d::ui::EditBox *editBox;
agora::rtc::IRtcEngine *engine;
agora::rtc::IRtcEngineEventHandler *eventHandler;
agora::cocos::VideoFrameObserver *videoFrameObserver;
std::map<uid_t, bool> users;
};
```

### Create the UI {#create-the-ui}

Create the user interface for the Video Call.

We recommend adding the following elements into the UI:

-   A channel name edit box

-   A join-channel button

-   A leave-channel button


In `HelloWorldScene.cpp`, add the following code:

```java
// Create an edit box to get the channel name entered by the user.
// You need to add the TextBox.png image to the ./Resources folder.
auto editBox = cocos2d::ui::EditBox::create(Size(120, 30), "TextBox.png");
if (editBox==nullptr) {
    problemLoading("'TextBox.png'");
} else {  
  editBox->setPlaceHolder("Channel ID");  
  editBox->setPosition(Vec2(origin.x + leftPadding + editBox->getContentSize().width/2,                         
                            origin.y + visibleSize.height                               
                                - editBox->getContentSize().height*1.5f));
    this->addChild(editBox, 0);
}

// Create a join-channel button. Clicking this button triggers the onJoinChannelClicked() function.
// You need to add the Button.png and ButtonPressed.png images to the ./Resources folder.
auto joinButton =
      cocos2d::ui::Button::create("Button.png", "ButtonPressed.png", "ButtonPressed.png");
  if (joinButton==nullptr) {
    problemLoading("'Button.png' and 'ButtonPressed.png'");
  } else {
    joinButton->setTitleText("Join Channel");

    joinButton->setPosition(Vec2(origin.x + leftPadding + joinButton->getContentSize().width/2,
                                 origin.y + visibleSize.height
                                     - 1*joinButton->getContentSize().height
                                     - 2*editBox->getContentSize().height));

    joinButton->addTouchEventListener([&](cocos2d::Ref *sender, ui::Widget::TouchEventType type) {
      switch (type) {
      case ui::Widget::TouchEventType::BEGAN:break;
      case ui::Widget::TouchEventType::ENDED:onJoinChannelClicked();
        break;
      default:break;
      }
    });

    this->addChild(joinButton, 0);
  }

// Create a leave-channel button. Clicking this button triggers the onLeaveChannelClicked() function.
auto leaveButton = ui::Button::create("Button.png", "ButtonPressed.png", "ButtonPressed.png");
  if (leaveButton==nullptr) {
    problemLoading("'Button.png' and 'ButtonPressed.png'");
  } else {
    leaveButton->setTitleText("Leave Channel");

    leaveButton->setPosition(Vec2(origin.x + leftPadding + leaveButton->getContentSize().width/2,
                                  origin.y + visibleSize.height
                                      - 2*leaveButton->getContentSize().height
                                      - 2*editBox->getContentSize().height));

    leaveButton->addTouchEventListener([&](cocos2d::Ref *sender, ui::Widget::TouchEventType type) {
      switch (type) {
      case ui::Widget::TouchEventType::BEGAN:break;
      case ui::Widget::TouchEventType::ENDED:onLeaveChannelClicked();
        break;
      default:break;
      }
    });

    this->addChild(leaveButton, 0);
  }
```

### Implement the Video Call logic {#implement-the-product-name-logic}

Take the following steps to initialize `IRtcEngine` and join a  channel.

#### Create and initialize the `IRtcEngine` object {#create-and-initialize-the-irtcengine-object}

In `HelloWorldScene.cpp`, add the following code:

```java
class MyIGamingRtcEngineEventHandler : public agora::rtc::IRtcEngineEventHandler {
private:
  HelloWorld *mUi;

public:
  explicit MyIGamingRtcEngineEventHandler(HelloWorld *ui) : mUi(ui) {}

  // Listen for the onJoinChannelSuccess callback.
  void onJoinChannelSuccess(const char *channel, uid_t uid,
                            int elapsed) override {
  }

  // Listen for the onLeaveChannel callback.
  void onLeaveChannel(const agora::rtc::RtcStats &stats) override {
  }

  // Listen for the onFirstRemoteVideoDecoded callback.
  void onFirstRemoteVideoDecoded(uid_t uid, int width, int height,
                                 int elapsed) override {
  }

  // Listen for the onUserOffline callback.
  void onUserOffline(uid_t uid,
                     agora::rtc::USER_OFFLINE_REASON_TYPE reason) override {
  }
};

...

void HelloWorld::onEnter() {
  cocos2d::Scene::onEnter();
  eventHandler = new MyIGamingRtcEngineEventHandler(this);
  // Create an IRtcEngine object.
  engine = createAgoraRtcEngine();
  // Define RtcEngineContext.
  agora::rtc::RtcEngineContext context;
  context.appId = AGORA_APP_ID;
  context.eventHandler = eventHandler;
  // Initialize the IRtcEngine object.
  engine->initialize(context);
}
```

#### Join a channel {#join-a-channel}

Call the `joinChannel` method, pass the token, user ID, and channel name to join a video call channel.

```java

void HelloWorld::onJoinChannelClicked() {
  if (editBox == nullptr || strlen(editBox->getText()) == 0) {
    return;
  }
    // Pass in your token and channel name to join the channel.
    // Set uid as 0, and the SDK assigns a user ID for the local user.
    engine->joinChannel(AGORA_TOKEN, editBox->getText(), "Cocos2d", 0);
}
                
```

#### Get and render raw data {#get-and-render-raw-data}

In the Cocos2d-x project, to display the local and remote video views, you need to use the raw video data function provided by the Agora SDK to get and cache the raw video data, and then send the video data to the Cocos2d-x engine for rendering.

![get-render-video-cocos2d](images/get_render_video_cocos2d.png)

1.  Get raw video data

    In the `VideoFrameObserver.h` file, add the following code:

    ```java
    #pragma once
    
    #include <map>
    #include <mutex>
    #include <vector>
    
    #ifdef __APPLE__
    #include <AgoraRtcKit/IAgoraMediaEngine.h>
    #include <AgoraRtcKit/IAgoraRtcEngine.h>
    #elif __ANDROID__
    #include "IAgoraMediaEngine.h"
    #include "IAgoraRtcEngine.h"
    #endif
    
    namespace agora {
    namespace cocos {
    // Define a CacheVideoFrame class, which caches the raw video data.
    class CacheVideoFrame {
    public:
      void resetVideoFrame(media::IVideoFrameObserver::VideoFrame &videoFrame);
    
    public:
      int width;
      int height;
      uint8_t *data;
    };
    
    // Define an IVideoFrameObserver class and register the callbacks of this class.
    class VideoFrameObserver : public media::IVideoFrameObserver {
    public:
      // Set the video format in the return value of the getVideoFormatPreference callback.
      VIDEO_FRAME_TYPE getVideoFormatPreference() override;
      // Set whether to rotate the video frame in the return value of the getRotationApplied callback.
      bool getRotationApplied() override;
      // Get the video data captured by the local camera in the onCaptureVideoFrame callback.
      bool onCaptureVideoFrame(VideoFrame &videoFrame) override;
      // Get the video data sent by the remote user in the onRenderVideoFrame callback.
      bool onRenderVideoFrame(unsigned int uid, VideoFrame &videoFrame) override;
    
    public:
      // Associate the uid with the textureId.
      void bindTextureId(unsigned int textureId, unsigned int uid);
    
    private:
      // Cache the video frame.
      void cacheVideoFrame(unsigned int uid,
                           media::IVideoFrameObserver::VideoFrame &videoFrame);
      // Render the cached video frame into a 2D texture image.                
      void renderTexture(unsigned int textureId, const CacheVideoFrame &frame);
    
    private:
      std::map<unsigned int, CacheVideoFrame> _map;
      std::mutex mtx;
    };
    } // namespace cocos
    } // namespace agora
    ```

    In `VideoFrameObserver.cpp`, add the following code to implement the `IVideoFrameObserver` class to get and cache the raw video data output by the Agora SDK.

    ```java
    #include "VideoFrameObserver.h"
    
    #if defined(__ANDROID__)
    #include <GLES/gl.h>
    #include <GLES2/gl2.h>
    #include <GLES2/gl2ext.h>
    #elif defined(__APPLE__)
    #include <OpenGLES/ES2/gl.h>
    #endif
    
    namespace agora {
    namespace cocos {
    void CacheVideoFrame::resetVideoFrame(
        media::IVideoFrameObserver::VideoFrame &videoFrame) {
      auto size = videoFrame.width * videoFrame.height * 4;
      if (size != width * height * 4) {
        if (data) {
          delete[] data;
          data = new uint8_t[size];
        } else {
          data = new uint8_t[size];
        }
      }
      memcpy(data, videoFrame.yBuffer, size);
      width = videoFrame.width;
      height = videoFrame.height;
    }
    
    // Set the video frame type as RGBA.
    media::IVideoFrameObserver::VIDEO_FRAME_TYPE
    VideoFrameObserver::getVideoFormatPreference() {
      return FRAME_TYPE_RGBA;
    }
    
    // Set the SDK to rotate the captured video frame.
    bool VideoFrameObserver::getRotationApplied() { return true; }
    
    // Get and cache the video data captured by the local camera.
    bool VideoFrameObserver::onCaptureVideoFrame(
        media::IVideoFrameObserver::VideoFrame &videoFrame) {
      cacheVideoFrame(0, videoFrame);
      return true;
    }
    
    // Get and cache the video data sent by the remote user.
    bool VideoFrameObserver::onRenderVideoFrame(
        unsigned int uid, media::IVideoFrameObserver::VideoFrame &videoFrame) {
      cacheVideoFrame(uid, videoFrame);
      return true;
    }
    
    // Bind the user ID with the texture ID.
    // If the specified uid is in the map, then the renderTexture() method will be called
    // to render the video frame of the user to a 2D texture image with the specified textureId.
    void VideoFrameObserver::bindTextureId(unsigned int textureId,
                                           unsigned int uid) {
      if (_map.find(uid) != _map.end()) {
        renderTexture(textureId, _map[uid]);
      }
    }
    
    // Cache the video frame output by the Agora SDK, and record the mapping relationship
    // between the video frame and user ID.
    void VideoFrameObserver::cacheVideoFrame(
        unsigned int uid, media::IVideoFrameObserver::VideoFrame &videoFrame) {
      if (_map.find(uid) == _map.end()) {
        _map[uid] = CacheVideoFrame();
      }
      mtx.lock();
      _map[uid].resetVideoFrame(videoFrame);
      mtx.unlock();
    }
    
    // Render the cached video frame into a 2D texture image. 
    void VideoFrameObserver::renderTexture(unsigned int textureId,
                                           const CacheVideoFrame &frame) {
      mtx.lock();
      glBindTexture(GL_TEXTURE_2D, textureId);
      glTexImage2D(GL_TEXTURE_2D, 0, GL_RGBA, frame.width, frame.height, 0, GL_RGBA,
                   GL_UNSIGNED_BYTE, frame.data);
      glBindTexture(GL_TEXTURE_2D, 0);
      mtx.unlock();
    }
    } // namespace cocos
    } // namespace agora
    ```

2.  Render raw video data

    After getting the raw video data, display the local and remote video views.

    1.  Register the video observer

        In `HelloWorldScene.cpp`, add the following code in the `onEnter` function to register the video observer:

        ```java
        void HelloWorld::onEnter() {
          ...
        
          // Register a video frame observer object.
          agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
          mediaEngine.queryInterface(engine, agora::AGORA_IID_MEDIA_ENGINE);
          if (mediaEngine) {
            videoFrameObserver = new agora::cocos::VideoFrameObserver;
            mediaEngine->registerVideoFrameObserver(videoFrameObserver);
          }
        }
        ```

    2.  Create and initialize the video views

        Define an `addTextureRender` method to create and initialize the video views of local and remote users. This method creates a 2D texture image according to the specified parameters, uses the 2D texture image to create a sprite object, and then adds the sprite object to the node to display the image in the scene. You can use this method to initialize the video views of the local and remote users and add the video views into the `HelloWorld` scene.

        ```java
        void HelloWorld::addTextureRender(uid_t uid, int width, int height) {
          Director::getInstance()->getScheduler()->performFunctionInCocosThread([=] {
            if (users.find(uid) == users.end()) {
              users[uid] = true;
            }
            // Create a 2D texture image.
            auto texture = new cocos2d::Texture2D;
            MipmapInfo info;
            texture->initWithMipmaps(&info, 1, Texture2D::PixelFormat::RGBA4444,
                                     width / 2, height / 2);
            // Use the 2D texture image to create a sprite object.
            auto sprite = Sprite::createWithTexture(texture);
            if (sprite != nullptr) {
              auto visibleSize = Director::getInstance()->getVisibleSize();
              Vec2 origin = Director::getInstance()->getVisibleOrigin();
        
              // Set the position of the sprite object in the scene.
              sprite->setPosition(Vec2(
                  origin.x + visibleSize.width - sprite->getContentSize().width / 2,
                  origin.y + visibleSize.height + sprite->getContentSize().height / 2 -
                      sprite->getContentSize().height * users.size()));
        
              // Add the sprite object as a child node to the scene.
              this->addChild(sprite, 0, "uid:" + std::to_string(uid));
            }
          });
        }
        ```

3.  Set the local video view

    In `HelloWorldScene. cpp`, add the following code to enable the local view so that the local user can see themselves in the call.

    ```java
    void HelloWorld::onJoinChannelClicked() {
      if (editBox == nullptr || strlen(editBox->getText()) == 0) {
        return;
      }
      // Enable the video.
      engine->enableVideo();
      ...
    }
    
    // In the onJoinChannelSuccess callback, call the addTextureRender() method to set the local video view.
    void onJoinChannelSuccess(const char *channel, uid_t uid,
                                int elapsed) override {
    
        mUi->addTextureRender(0, 640, 480);
      }
    ```

4.  Set the remote video view

    In `HelloWorldScene.cpp`, add the following code to set up the remote video view so that the local user can see remote users.

    ```java
    void onFirstRemoteVideoDecoded(uid_t uid, int width, int height,
                                  int elapsed) override {
     // Call addTextureRender in the onFirstRemoteVideoDecoded callback
     mUi->addTextureRender(uid, width, height);
    ```

    Call the `update()` method every frame to display each user's video frames as one continuous video.

    ```java
    void HelloWorld::update(float delta) {
      Node::update(delta);
      for (auto user : users) {
        auto sprite =
            this->getChildByName<Sprite *>("uid:" + std::to_string(user.first));
        if (sprite) {
          auto texture = sprite->getSpriteFrame()->getTexture();
    
          int textureId = texture->getName();
          videoFrameObserver->bindTextureId(textureId, user.first);
        }
      }
    }
    ```


### Leave the channel and release `IRtcEngine` {#leave-the-channel-and-release-irtcengine}

Call the `leaveChannel` method to leave the channel according to your scenario.

```java
void HelloWorld::onLeaveChannelClicked() { engine->leaveChannel(); }

void onLeaveChannel(const agora::rtc::RtcStats &stats) override {
    mUi->removeAllTextureRenders();
  }

void HelloWorld::onExit() {
  Node::onExit();
  agora::rtc::IRtcEngine::release(true);
  engine = nullptr;
  delete eventHandler;
  eventHandler = nullptr;
  delete videoFrameObserver;
  videoFrameObserver = nullptr;
}
```

## Test your app {#test-your-app}

To test your app, follow the steps:

1.  Fill in the `appId` and `token` parameters with the App ID and temporary token that you retrieve from Agora Console. Fill `channelName` with the channel name that you use to generate the temporary token.

2.  Connect an Android device to your computer, and click Build and run on your Android Studio. A moment later you will see the project installed on your device.

3.  When the app launches, you should be able to see yourself on the local view.

4.  Ask a friend to join the video call with you on the [demo app](https://webdemo.agora.io/basicVideoCall/index.html). Enter the same App ID and channel name.

    After your friend joins the channel, you should be able to see and hear each other.


## Next steps {#next-steps}

Generating a token by hand is not helpful in a production context. [Authenticate Your Users with Tokens](https://docs.agora.io/en/Video/token_server?platform=Android) shows you how to start the video call with a token that you retrieve from your server.

## See also {#see-also}

This section includes reference information you need to know when using the Agora Video SDK.

### Other ways to integrate the SDK {#other-ways-to-integrate-the-sdk}

If you use ndk-build as the build tool, that is, you set `PROP_BUILD_TYPE=ndk-build` in the `./proj.android/gradle.properties` file, and then add the following code to the `./proj.android/app/jni/Android.mk` file:

```java
LOCAL_PATH := $(call my-dir)         
// Add the following four lines to specify the source files for the shared libraries.
include $(CLEAR_VARS) 
LOCAL_MODULE := agora-rtc-sdk
LOCAL_SRC_FILES := $(LOCAL_PATH)/../../../sdk/android/agora/$(TARGET_ARCH_ABI)/libagora-rtc-sdk.so
include $(PREBUILT_SHARED_LIBRARY)
include $(CLEAR_VARS)
LOCAL_MODULE := MyGame_shared
LOCAL_MODULE_FILENAME := libMyGame
LOCAL_SRC_FILES := $(LOCAL_PATH)/hellocpp/main.cpp \
                 $(LOCAL_PATH)/../../../Classes/AppDelegate.cpp \
                 $(LOCAL_PATH)/../../../Classes/HelloWorldScene.cpp \
                 // Add the source file VideoFrameObserver.cpp.
                 $(LOCAL_PATH)/../../../Classes/VideoFrameObserver.cpp \
LOCAL_C_INCLUDES := $(LOCAL_PATH)/../../../Classes \
                  // Add the following line to add ./sdk/android/agora/include to the include search path.
                  $(LOCAL_PATH)/../../../sdk/android/agora/include
# _COCOS_HEADER_ANDROID_BEGIN
# _COCOS_HEADER_ANDROID_END
LOCAL_STATIC_LIBRARIES := cc_static
// Add the following line to link agora-rtc-sdk.
LOCAL_SHARED_LIBRARIES := agora-rtc-sdk
# _COCOS_LIB_ANDROID_BEGIN    
# _COCOS_LIB_ANDROID_END  
include $(BUILD_SHARED_LIBRARY)
$(call import-module, cocos)
# _COCOS_LIB_IMPORT_ANDROID_BEGIN
# _COCOS_LIB_IMPORT_ANDROID_END
```

