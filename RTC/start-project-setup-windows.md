# Set up the development environment

In this section, we will create a Windows project and integrate the SDK into the project.

## Create a Windows project

Now, let's build a Windows project from scratch. Skip to [Integrate the SDK](#inte) if a Windows project already exists.

1. Open **Microsoft Visual Studio** and click **Create new project**.
2. On the **New Project** panel, choose **MFC Application** as the project type, input the project name, choose the project location, and click **OK**.
3. On the **MFC Application** panel, choose **Application type > Dialog based**, and click **Finish**.

### Integrate the SDK {#inte}

Follow these steps to integrate the Agora Video SDK into your project.

1. Configure the project files

    - Go to [SDK Downloads](https://docs.agora.io/en/Video/downloads?platform=Windows), download the latest version of the Agora SDK for Windows, and unzip the downloaded SDK package.
    - Copy the **x86** or **x86_64** folder of the downloaded SDK package to your project files according to your development environment.

    <note>The libraries with the extension suffix are optional. Add the corresponding libraries according to your needs. For the feature of each extension library, see <xref href = "https://docs.agora.io/en/Interactive%20Broadcast/faq/reduce_app_size_rtc?platform=Windows#extension_libraries" formt="html" scope="external">extension libraries</xref>.</note>

2. Configure the project properties

    Right-click the project name in the **Solution Explorer** window, click **Properties** to configure the following project properties, and click **OK**.

    - Go to the **C/C++ > General > Additional Include Directories** menu, click **Edit**, and input **$(SolutionDir)include** in the pop-up window.
    - Go to the **Linker > General > Additional Library Directories** menu, click **Edit**, and input **$(SolutionDir)** in the pop-up window.
    - Go to the **Linker > Input > Additional Dependencies** menu, click **Edit**, and input **agora_rtc_sdk.lib** in the pop-up window.