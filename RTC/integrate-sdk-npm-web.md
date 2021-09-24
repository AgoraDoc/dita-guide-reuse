# Integrate the SDK

Integrate the the [sdk-name] for [platform] into your project through npm, as follows:

1. In `package.json`, add `agora-rtc-sdk-ng` and its version number to the `dependencies` field:

   ```json
   {
     "name": "agora_web_quickstart",
     "version": "1.0.0",
     "description": "",
     "main": "main.js",
     "scripts": {
       "test": "echo \"Error: no test specified\" && exit 1"
     },
     "dependencies": {
       "agora-rtc-sdk-ng": "latest",
     },
     "author": "",
     "license": "ISC"
   }
   ```   

2. To import the `AgoraRTC` module in your project, copy the following code into `main.js`.

   ```javascript
   import AgoraRTC from "agora-rtc-sdk-ng"
   ```
