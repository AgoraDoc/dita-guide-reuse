# DITA Guide Reuse Demo
This is a demo repository for writing guides in markdown and utilizing DITA for reuse.

## Prerequisites

- Install [oXygen XML Author](https://confluence.agoralab.co/display/TEKP/Oxygen+XML+Author+v23).
- Basic knowledge of DITA and [LwDITA](http://docs.oasis-open.org/dita/LwDITA/v1.0/cnprd01/LwDITA-v1.0-cnprd01.html).

## Project structure

All files are in the RTC folder. The following table lists the major files and folders:

| Folder/File                       | Description                                                  |
| --------------------------------- | ------------------------------------------------------------ |
| **config**                        | Files for all the filter and variable settings.              |
| **conref**                        | Files for content that can be conrefed.                      |
| `_Live-Streaming-Premium.ditamap` | The DITA map for the documentation of Live Streaming Premium. Use this map to organize topics and build outputs for Live Streaming Premium. |
| `_Video.ditamap`                  | The DITA map for the documentation of Video Call. Use this map to organize topics and build outputs for Video Call. |
| `_rtc-guide.xpr`                  | The Oxygen project file that applies build configurations to the maps and organizes files in the DITA project. |
| `get-started.md`                  | The parent topic for the Get Started guides. Contains the doc title and the introductory paragraph. |
| All the `start-*.md` files        | All the sub topics in the Get Started guides. Each file is named after the section title. If the topic only applies to a specific platform, the filename is suffixed with the platform, for example `start-project-setup-android.md`. |

## Content reuse strategies 

In LwDITA, we can use the following elements for reuse:

### Keyrefs

Imagine keys as variables, and keyrefs are references to the variables.

Use keyrefs for paragraphs or topics with only phrase-level differences, for example the platform name.

Keyrefs support the following types of content:

- Phrases. Example:

  ```markdown
  This page shows the minimum code you need to add [feature] into your app by using the [sdk-name] for [platform].
  ```

  The definition of the`feature` and `sdk-name` keys in `keys-video.ditamap`:

  ```xml
  <keydef keys="feature">
      <topicmeta>
          <keywords>
              <keyword>video call</keyword>
          </keywords>
      </topicmeta>
  </keydef>
  <keydef keys="sdk-name">
      <topicmeta>
          <keywords>
              <keyword>Agora Video SDK</keyword>
          </keywords>
      </topicmeta>
  </keydef>
  ```

  The definition of the `platform` key in `keys-android.ditamap`:

  ```xml
  <keydef keys="platform">
      <topicmeta>
          <keywords>
              <keyword>Android</keyword>
          </keywords>
      </topicmeta>
  </keydef>
  ```

- Links. Example:

  ```markdown
  Agora provides an open source sample project [start-sample-project-android] on GitHub that implements one-to-one video call and group video call for your reference.
  ```

  The definition of the `start-sample-project-android` key in `keys-video.ditamap`:

  ```xml
  <keydef keys="start-sample-project-android" href="https://github.com/AgoraIO/Basic-Video-Call" scope="external" format="html">
  ```

- Images. Example:

  ```markdown
  The following figure shows the workflow of a [feature] implemented by the Agora SDK.
  
  ![start-basic-workflow]
  ```

  The definition of the `start-basic-workflow` key  in `keys-video.ditamap`:

  ```xml
  <keydef keys="start-basic-workflow" href="https://web-cdn.agora.io/docs-files/1627550978702"format="png" scope="external"/>
  ```

### Conditions

Apply conditions for content applicable to a specific product or platform.

The markdown syntax does not support marking conditions, you need to write DITA tags for content to be marked and filtered.

Example:

```xml
<ol>
<li props="live">Set the client role</li>
<li>Retrieve a token</li>
<li>Join a channel</li>
<li>Publish and subscribe to <ph keyref="media-type"/> in the channel</li>
</ol>
```

The first bullet only applies to the Live Streaming products.

The condition values you can use are defined in [subject-scheme-rtc.ditamap](https://github.com/AgoraDoc/dita-guide-reuse/blob/main/RTC/config/subject-scheme-rtc.ditamap):

- Product
  - video
  - live
- Platform
  - android
  - ios
  - mac
  - web
  - windows
  - electron
  - unity
  - rn
  - flutter
  - cocos2d
  - cocos-creator

> Do not use the product conditions and platform conditions at the same time.

### Conrefs

Use conrefs together with conditions for code blocks across the RTC products.

Example:

```xml
In `/app/java/com.example.<projectname>/MainActivity`, add the following lines after the `onCreate` function:
   
<p props="video" conref="conref/get-started-sample-code-android.dita#get-started-sample-code/init-video"/>
<p props="live" conref="conref/get-started-sample-code-android.dita#get-started-sample-code/init-live"/>
```

The conrefed content is in a separate file [get-started-sample-code-android.dita](https://github.com/AgoraDoc/dita-guide-reuse/blob/main/RTC/conref/get-started-sample-code-android.dita).

## Create the get started guide for a new platform

This section shows you how to write the guide for a new platform.

### Configure Oxygen for the platform

For easier reading and writing, configure the profiling settings so that Oxygen shows only the content concerning the new platform.

1. In Oxygen, Open the `_Video.ditamap` in DITA Maps Manager view.

2. Click the filter button on the upper-right and select **Profiling Settings**.

   ![image-20210922154732258](images/image-20210922154732258.png)

3. Under the **Profiling Condition Sets** section, click **New**.

   ![image-20210922154927087](images/image-20210922154927087.png)

4. In the pop-up windows, give the condition set a name, and check the conditions you need. For example, if you are writing the guide for iOS Video Call, check `ios` and `video`, and click **OK**.

5. Click the filter button and select the condition set you just created.

### Create topics for the platform

Now you should see something like this:

![image-20210922155417754](images/image-20210922155417754.png)

The topics with red exclamation marks are the sections that only apply to the platfrom and need to be created. You can quickly create the topics as follows:

1. Double click a topic with the red exclamation mark.
2. In the pop-up window, click **Create new file**, and click **Create**.

### Write markdown with reuse in mind

Fill the created files with the markdown source, and note the following:

- For each markdown file, start from heading one, and try to keep the headings to heading one and heading two. If you have heading threes in a topic, consider splitting the files.
- Keep in mind that the topics should be reused by four RTC products, and use variables and conditions accordingly.
   - For variables that can be used by one product, define them in the key map for that product.
   - For variables that can be used by one platform, define them in the key map for that platform.

### Syntax

Apart from the regular markdown syntax that we are familiar with, we need to use the following syntax:

- Keyrefs
  - `[key-name]` for phrases and links
  - `![key-name]`for images. To set the alt text for the image, use the navtitle attribute in the key definition.
- Cross-references
  - Anchor point: `{#anchor-id}`
  - Link: `[link-text](parent-topic.md#anchor-id)`
  - Example: `[Other approches to intergrate the SDK](get-started.md#other)` creates a link to the section title `## Other approches to integrate the SDK {#other}`.



### Build the docs

Before building the docs, configure the transformation scenarios in the project file in Oxygen:

1. Open `_rtc-guide.xpr`.

2. Under Main Files, right click the DITA map to be built and select **Transform** > **Configure Transformation Scenario(s)...**.

   In the pop-up window, you can see the map is associated with two transformation scenarios for two output formats: HTML5 and markdown.

3. To configure the transformation scenarios for your platform, do the following for each output format:

   1. Click the transformation scenario for Android and click **Duplicate**.
   2. Update the name of the scenario.
   3. Switch to the **Filters** tab, and change the ditaval file according to your platform.
   4. Switch to the **Output** tab, and change the output directory according to your platform.
   5. Click **OK**, and then click **Save and close**.

To generate the HTML and markdown outputs, do the following:

1. Right click the DITA map and select **Transform > Transform with...**.
2. In the pop-up window, select the scenarios you need (press cmd/ctrl for multi-select) and click **Apply selected scenarios**.

If the build succeeds, the generated HTML file automatically opens in your browser.

The output files are in the **out** folder of this repository.
