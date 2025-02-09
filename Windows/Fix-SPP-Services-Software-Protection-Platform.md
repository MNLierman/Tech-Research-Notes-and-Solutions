## Research Notes on the Software Protection Platform (SPP) Service
### Commonly referred to as **`SPP-Security`** in Event Log.

<p></p>

### PROBLEM: Hundreds of `Security-SPP` events in Event Log, which seem to the user to correlate with PC lag, jitter, and general unresponsiveness.<p></p>
### SOLUTION: If SPPSvc has gone out to lunch, then we need to adjust the SPPSvc variables in registry, disable RulesEngine, change the ActivationInterval, and ensure proper permissions.<p></p>
Alright, let's get started. Keep in mind you are following these instructions at your own risk, and these are also, at this current moment, not finished. Make a backup of your registry. I take zero responsibility for what happens from this point forward. If your PC turns into a molten heap of melted plastic and circuit board after applying these changes, that's on you, man.

**First, backup SPP registry keys:**
* Backup `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform`, `HKLM\SYSTEM\CurrentControlSet\Services\sppsvc` and `HKEY_USERS\S-1-5-20\Software\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform` before you do anything else. Export them to a folder somewhere, it doesn't matter where.
* System Restore isn't so much of a waste of IO and system resources when you end up in a situation where you wished you had a way to restore your PC to yeesterday. I recommend turning System Restore on if you turned it off, and create a restore point before making these changes.

**Modifying Software Protection Platform (SPP) Registry:*** <br/>
**Fix Permissions:** While the permissions may not necessarily be *broken*, this is part of the process, we want to ensure permissions are not an issue moving forward. It's been noted elsewhere by Microsoft support that permission issues have historically caused issues. We want to ensure this isn't a problem. Set the following registry permissions:
  * `NT Service\sppsvc`, `Network Service`, and `SYSTEM` 🠆 Full Control
  * `Local Services` 🠆 Read Only access. Don't set to Full Control, as this allows any service to modify SPP and that's a security risk.
  * `Network Service` should own the registry keys.
<p></p>

**Adjust SPP Variables:**
* Create new DWord called `SuppressRulesEngine` with a value of ``` in `SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform\Activation`, `SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform`, and `HKU\S-1-5-20\Software\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform`. This turns off Rules Engine, but then we need to set our own scheduling, and if we don't change the default values, SPPSvc will freak out.
* Rename `ActivationInterval` to `ActivationInterval.bak` and create a new DWord called `ActivationInterval` and set it to 12 hours which is the value of `43200000`. This value is in milliseconds, it tells SPPSvc how often it should wake up and check licensing status. It will follow this schedule we set as long as rules engine is turned off., which tells SPPSvc to trigger a restart and check licensing status at this time. Don't change this to longer than 12 hours or you may end up with apps that are partially broken, Office in read-only mode, Windows preventing you from setting a wallpaper or lockscreen. 12 hours is on the upper max end of check in times.
* Set `NoExpirationUX` to `1`. While rare, it's possible that SPPSvc may fail to restart, this setting will temporarily prevent Windows from making any drastic UI changes due to lapse in licensing check coverage.

**Possibilities:**
* There is a possibility that SPPSvc will act dead and not do anything, won't restart itself and wont' check in with licensing without restarting the service, in this case, you will need an SPPSvc restart task for Task Scheduler. I will show you what that should look like should your SPPSvc not follow the settings you configured (12 hour check in((.


![image](https://github.com/user-attachments/assets/6ca4bd11-0aac-4263-a100-241599c73fc1)

