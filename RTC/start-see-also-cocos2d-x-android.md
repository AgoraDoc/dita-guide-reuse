# See also

This section includes reference information you need to know when using the [sdk-name].

## Other ways to integrate the SDK

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
