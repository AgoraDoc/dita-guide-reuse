# Common issues

If the deployment environment is Android, users in mainland China may get stuck in `Running Gradle task 'assembleDebug'...` or see the following error:

```
Running Gradle task 'assembleDebug'...
Exception in thread "main" java.net.ConnectException: Connection timed out: connect
```

Take the following steps to resolve this issue:

1. In the `build.gradle` file of the Android project, use mirrors in China for `google` and `jcenter.`

    ```java
    buildscript {
        ext.kotlin_version = '1.3.50'
        repositories {
            // google()
            // jcenter()
            maven { url 'https://maven.aliyun.com/repository/google' }
            maven { url 'https://maven.aliyun.com/repository/public' }
        }
     
    ...
     
    allprojects {
        repositories {
            // google()
            // jcenter()
            maven { url 'https://maven.aliyun.com/repository/google' }
            maven { url 'https://maven.aliyun.com/repository/public' }
        }
    }
    ```

2. In the `gradle-wrapper.properties` file, set `distributionUrl` to a local file. For example, for gradle 5.6.4, you can copy `gradle-5.6.4-all.zip` to `gradle/wrapper` and set `distributionUrl` to:

    ```
    distributionUrl=gradle-5.6.4-all.zip
    ```


