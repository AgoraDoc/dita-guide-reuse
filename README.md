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

