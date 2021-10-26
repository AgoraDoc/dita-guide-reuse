# Project setup

In order to create the environment necessary to integrate [product-name] into your app, do the following.

1. For new projects, download [Cocos2d-x](https://www.cocos.com/products#Cocos2d-x) and [create a new project using the `cocos new` command](https://docs.cocos2d-x.org/cocos2d-x/v4/en/editors_and_tools/cocosCLTool.html#creating-a-new-project).
2. Integrate the [sdk-name] into your project:
   1. Go to [SDK downloads](https://docs.agora.io/en/Video/downloads?platform=Cocos2d-x), download the latest version of the [sdk-name] for Android, and extract the files from the downloaded SDK package.
   2. Create an `sdk` folder in your project root directory for the Agora Video SDK. You can create the folder and add subfolders in the following structure:
   
      ```xml
      └─sdk
         ├─android
         │  ├─agora
         │  └─lib
         └─ios
             └─agora
      ```
      
   3. Copy the following files and folders from the `video/libs/android` dolder to your project:
   
      | File or folder | Path in your peoject |
      | :------------- | :------------------- |
      | `agora-rtc-sdk.jar` file | `<project_root>/sdk/android/lib` |
      | `arm64-v8a` folder	| `<project_root>/sdk/android/agora` |
      | `armeabi-v7a` folder	| `<project_root>/sdk/android/agora` |
      | `include` folder      | `<project_root>/sdk/android/agora` |
      | `x86_64` folder       | `<project_root>/sdk/android/agora`|
      | `x86` folder          | `<project_root>/sdk/android/agora` |
   
   4. Add the configuration of the `.so` libraries to the build files with the following steps:
   
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
      
3. Add project permissions.

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

4. Get the device permission.

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
      
