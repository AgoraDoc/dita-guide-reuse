# Project setup

1. For new projects, create a directory named `agora_electron_quickstart`. For a basic Electron app, create the following files in the directory:

    - `package.json`: The configuration of your Electron project, such as project name and dependencies.
    - `index.html`: The visual interface with the user.
    - `main.js`: The main process of your Electron app.
    - `renderer.js`: The renderer process of your Electron app, where the APIs provided by the Agora SDK are implemented.

2. Integrate the [sdk-name] into your project.

   1. Modify the `package.json` file as follows:

      - `electron_version`：The version number of Electron. You can set it to `12.0.0`, `11.0.0`, `10.2.0`, `9.0.0`, `7.1.2`, `6.1.7`, `5.0.8`, `4.2.8`, `3.0.6`, or `1.8.3`.
      - `prebuilt`: Set it as `true` to prevent compatibility issues.
      - `agora-electron-sdk`: The version number of the [sdk-name] for [platform]. See [Release notes](https://docs.agora.io/en/Voice/release_electron_video?platform=Electron) for details.
      - `electron`: Set it as the same as  `electron_version`.

      **macOS**
       ```
      {
        "name": "electron-demo-app",
        "version": "0.1.0",
        "author": "your name",
        "description": "My Electron app",
        "main": "main.js",
        "scripts": {
          "start": "electron ."
        },
        "agora_electron": {
          "electron_version": "10.2.0",
          "platform": "darwin",
          "prebuilt": true
        },
        "devDependencies": {
          "agora-electron-sdk": "3.4.2",
          "electron": "10.2.0"
        }
      }
       ```

       **Windows**
       ```
      { 
        "name": "electron-demo-app",
        "version": "0.1.0",
        "author": "your name",
        "description": "My Electron app",
        "main": "main.js",
        "scripts": {
          "start": "electron ."
        },
        "agora_electron": {
          "electron_version": "10.2.0",
          "platform": "win32",
          "prebuilt": true,
          "arch": "ia32"
        },
        "devDependencies": {
          "agora-electron-sdk": "3.4.2",
          "electron": "10.2.0"
        }
      }
       ```

   2. In Terminal, go to the root folder of your project:
      <note>If you already have a node_modules folder in the root folder of your project, Agora recommands deleting the <code>node_modules</code> folder before running the following commands. Otherwise, an error might occur.</note>

       **macOS**
       
       Run the following command to install dependencies:
    
      ```
      npm install
      ```
  
      **Windows**
      1. Run the following command to install the 32-bit version of Electron:
          ```
          npm install -D --arch=ia32 electron
          ```

      2. Run the following command to install the other dependencies:
          ```
          npm install
          ```


