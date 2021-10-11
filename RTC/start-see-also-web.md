# See also

- Agora also provides an open-source [sample project](https://github.com/AgoraIO/API-Examples-Web) on GitHub for your reference.

- In addition to integrating the Agora Web SDK into your project through npm, you can also choose either of the following methods to integrate the Agora Web SDK into your project:

  - Through the CDN: Add the following code to the line before `<style>` in the `index.html` file of your project.

    ```html
    <script src="https://download.agora.io/sdk/release/AgoraRTC_N-4.7.1.js"></script>
    ```

  - Manually download the latest [Agora Web SDK 4.x](https://docs.agora.io/en/Video/downloads?platform=Web), copy the `.js` file to the directory of your project files, and add the following code to the line before the `<style>` tag in your project.

    ```html
    <script src="./AgoraRTC_N-4.7.1.js"></script>
    ```

    <note>If you use the above methods, the SDK fully exports an `AgoraRTC` object. You can visit the `AgoraRTC` object to operate the Web SDK.</note>