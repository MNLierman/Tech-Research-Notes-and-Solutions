## Research Notes and Descriptions of Windows Services
#### Most guides and articles posted online regurgitate the same info that Microsoft provides. We investigate the true purpose of each Windows Service. Ultimately, our goal is to disable services for unneeded features – services the user (you) doesn't intend on needing. Doing so decreases the attack surface, and frees up system resources.

#### >> This GitHub page is a WIP, as are most our resources we are uploading to GitHub. Our strategy is 1 is greater than 0; part of this concept is accepting that nothing is perfect, a published WIP is better than something collecting dust.

**Table of Contents**

1. [Services Safe to Disable](#safe-to-disable)
2. [Services Optional to Disable](#optional-to-disable)
3. [Services Unsafe to Disable](#unsafe-to-disable)
4. [Research Notes](#research-notes)
5. [Microsoft Resources](#ms-learn-resources): MS Learn pages about Windows Services
6. [All Other Resources](#all-other-resources)

-----

<br/>

### Safe to Disable

|Display Name|Name|Notes|
|---|---|---|
|Adobe Acrobat Update Service|`AdobeARMservice`||
|Adobe Genuine Monitor Service|`AGMService`|CC 5.4.5.550 sometimes fails to load Apps tab when this service is disabled.|
|Adobe Genuine Software Integrity Service|`AGSService`||
|AdobeUpdateService|`AdobeUpdateService`||
|Auto Time Zone Updater|`tzautoupdate`||
|AVCTP service|`BthAvctpSvc`|Might be needed for Bluetooth audio devices, but not other BT devices like game controllers.|
|Bluetooth Audio Gateway Service|`BTAGService`|Needed for Your Phone "Calls" feature, but not other BT devices like game controllers.|
|Bluetooth Driver Management Service|`BcmBtRSupport`|Third-party service for Broadcom Bluetooth adapters, not required for BT connections.|
|Certificate Propagation|`CertPropSvc`||
|Connected Devices Platform Service|`CDPSvc`|Needed for Night Light.|
|Connected Devices Platform User Service_\*|`CDPUserSvc_*`|May only be disabled in registry by setting `HKLM\SYSTEM\CurrentControlSet\Services\CDPUserSvc_*\Start` = `0x4`.|
|Connected User Experiences and Telemetry|`DiagTrack`||
|Data Usage|`DusmSvc`|Shows the "last 30 days" section in Settings › Network & Internet. When disabled, the Windows 11 Settings home screen will always say Disconnected.|
|Delivery Optimization|`DoSvc`|If you have a second Windows 10 computer in the same LAN, they can share Windows Update downloads, reducing your ISP's traffic quota usage.|
|Device Association Service|`DeviceAssociationService`|Needed for Bluetooth.|
|Device Management Wireless Application Protocol (WAP) Push message Routing Service|`dmwappushservice`||
|Diagnostic Policy Service|`DPS`||
|Display Enhancement Service|`DisplayEnhancementService`||
|Display Policy Service|`DispBrokerDesktopSvc`||
|Geolocation Service|`lfsvc`||
|IP Helper|`iphlpsvc`|Not needed for IPv6.|
|IPsec Policy Agent|`PolicyAgent`|Not needed for IPv6.|
|Microsoft (R) Diagnostics Hub Standard Collector Service|`diagnosticshub.standardcollector.service`||
|Microsoft App-V Client|`AppVClient`||
|Microsoft Storage Spaces SMP|`smphost`|Might be needed for Storage Spaces.|
|Net.Tcp Port Sharing Service|`NetTcpPortSharing`||
|Network Connected Devices Auto-Setup|`NcdAutoSetup`||
|NVIDIA Display Container LS|`NVDisplay.ContainerLocalSystem`|Only needed to launch Nvidia Control Panel.|
|OpenSSH Authentication Agent|`ssh-agent`||
|Payments and NFC/SE Manager|`SEMgrSvc`||
|Quality Windows Audio Video Experience|`QWAVE`||
|Radio Management Service|`RmSvc`|Might be needed for Wi-Fi and Bluetooth.|
|Remote Access Connection Manager|`RasMan`||
|Remote Registry|`RemoteRegistry`||
|Routing and Remote Access|`RemoteAccess`||
|Shared PC Account Manager|`shpamsvc`||
|Smart Card Device Enumeration Service|`ScDeviceEnum`||
|Sync Host_\*|`OneSyncSvc_*`|May only be disabled in registry by setting `HKLM\SYSTEM\CurrentControlSet\Services\OneSyncSvc_*\Start` = `0x4`.|
|Telephony|`TapiSrv`||
|Touch Keyboard and Handwriting Panel Service|`TabletInputService`|Needed by Windows Terminal (at least in Windows 11).|
|User Experience Virtualization Service|`UevAgentService`||
|Windows Biometric Service|`WbioSrvc`|Needed to load Settings › Accounts › Sign-in options and maybe to use fingerprint readers.|
|Windows Connect Now - Config Registrar|`wcncsvc`||
|WLAN AutoConfig|`WlanSvc`|Might be needed for Wi-Fi.|

<br/>

### Optional to Disable
Optional services support features that many people may want to use, such as printer or bluetooth headphones. If you don't use the listed features, you can disable these services for better performance. Be aware, however, that Windows may occationally log errors about these disabled services, but it's harmless.

|Display Name|Name|Notes|
|---|---|---|
|This GitHub page is a WIP| | |

<br/>

### Unsafe to Disable

|Display Name|Name|Notes|
|---|---|---|
|Bluetooth Support Service|`bthserv`|Required for Bluetooth pairing and usage. Can only be disabled if you don't use Bluetooth.|
|Bluetooth User Support Service_\*|`BluetoothUserService_*`|Required for Bluetooth pairing and usage. Can only be disabled if you don't use Bluetooth.|
|Clipboard User Service_\*|`cbdhsvc_*`|Required to copy screenshots taken using Snip & Sketch (Win+Shift+S).|
|JetBrains ETW Host Service \*|`JetBrainsEtwHost`|Required to start Visual Studio if dotTrace is installed.|
|LGHUB Updater Service|`LGHUBUpdaterService`|Required to start Logitech G Hub. Can only be disabled if you have no custom button bindings and are using On-board Memory Mode.|
|Microsoft Office Click-to-Run Service|`ClickToRunSvc`|Required to start Office programs, not just install updates.|
|Network Location Awareness|`NlaSvc`|When disabled, network connections can show the wrong state in `ncpa.cpl`.|
|VMAuthdService|`VMware Authorization Service`|Required to start VMware Workstation guests in Windows 10 host. Optional in Windows 7 host.|
|Windows Connection Manager|`Wcmsvc`|Required for Ethernet page to appear in Settings › Network & Internet.|
|Windows Push Notifications System Service|`WpnService`|Required for Action Center (Win+A) to appear.|
|Windows Push Notifications User Service_\*|`WpnUserService_*`|Required for Action Center (Win+A) to appear.|

<br/>

## RESEARCH NOTES
RPC Locator Service: Description says that it's only used in 2003 and lower, and provided in Windows for legacy compatibility only. So then why is it running all the time on new installs? Two audit articles rec. disabling ([1](https://www.syxsense.com/syxsense-securityarticles/cis_benchmarks/syx-1033-12591.html), [2](https://www.tenable.com/audits/items/CIS_MS_Windows_7_v3.2.0_Level_1_Bitlocker.audit:9035cdeb68be58c44a2561709a02c904))

<br/>

## Microsoft Learn Resources
#### Articles, pages, and developer notes writting by Microsoft about Services in Windows.

* Services Safe to Disable for Windows IoT Enterprise
* Per-user Services Explainer

<br/>

## All Other Resources
- [Aldaviva/KillUnwantedProcesses](https://github.com/Aldaviva/KillUnwantedProcesses)
- [Black Viper’s Windows 10 Service Configurations](http://www.blackviper.com/service-configurations/black-vipers-windows-10-service-configurations/) (unmaintained)
