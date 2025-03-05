## Supplemental Research Notes for Experimental Windows Performance Tweaks
#### ⚠️ Be Cautious! These are bleeding edge undocumented (except here) and only briefly tested modifications. These changes may very well break Windows. Modifications should be, ideally, tested in isolated VMs to avoid making too many **_experimental_** tweaks at once and not knowing which one may have caused an issue. Even simple actions like deleting a registry key/value or creating an incorrect one can render the OS unbootable or prevent login.

The performance gains from these experimental changes are uncertain. However, this effort aims to lower resource and RAM usage for unsupported hardware like “beater laptops” – a term we use at my IT shop to mean *old, but functional laptops meant for basic use cases* like speed tests, wifi checks, and cabling tests checks. While Linux is an option for these types of laptops, many tools or vendor test suites require Windows.

Running an unsupported OS like Windows 10 or older versions without security updates is not ideal, and EOL machines should not be used. But many small businesses, like ours, are in critical growth stages and cannot afford new hardware. Therefore, these experimental modifications are justified. When combined together as whole, performance tweaks and modifications enable old laptops with 2-4GB of RAM to run Windows 11 and perform for these needed test cases. By the way, if Microsoft is *really* wanting to target IoT vendors and developers with Windows IoT Enterprise, 2GB of RAM is **_generous_**.

<br/>

## Combining SvcHost Groups

Highly experimental modification which merges PenService into Bluetooth App Svchost Group. They have a nearly identical set of security requirements, so nothing *should* go wrong here.

* `reg add "HKLM\SYSTEM\ControlSet001\Services\PenService" /v ImagePath /t REG_EXPAND_SZ /d "%SystemRoot%\system32\svchost.exe -k BthAppGroup" /f`
* `powershell.exe -Command "Set-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Svchost' -Name 'BthAppGroup' -Value @('BluetoothUserService', 'PenService') -Type MultiString"`

<p>-----</p>

Another highly experimental change which merges WcmSvc into the main LocalServiceNetworkRestricted group, I see no sense in breaking it out, it barely does anything anyways.

`reg add "HKLM\SYSTEM\ControlSet001\Services\Wcmsvc" /v ImagePath /t REG_EXPAND_SZ /d "%SystemRoot%\system32\svchost.exe -k LocalServiceNetworkRestricted -p" /f`
