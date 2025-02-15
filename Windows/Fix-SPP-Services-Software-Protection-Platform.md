## Research Notes on the Software Protection Platform (SPP) Service
### Commonly referred to as **`SPP-Security`** in Event Log.

<p></p>

### PROBLEM: Hundreds of `Security-SPP` events in Event Log, which can also correlate with PC lag, jitter, and general unresponsiveness.<p></p>
### SOLUTION: If SPPSvc has gone out to lunch, then we need to adjust the SPPSvc variables in registry, disable RulesEngine, change the ActivationInterval, and ensure proper permissions.<p></p>

**DISCLAIMER: Proceed with Caution!**

Before we begin, let's make one thing crystal clear: *you are embarking on this journey at your own risk*. These instructions are provided as-is, and while I've done my best to ensure accuracy and safety, unforeseen circumstances can always arise. **If you find yourself facing a digital apocalypse – and your PC becomes a molten heap of silicon and regret – well, that's on you, friend.** I'm not liable.

That said, following these instructions carefully should help you regain normal function of the SPP service. 

Alright, with that out of the way, let's get started. Here's how we restore order to the Software Protection Platform Service (SPPSvc). Let's review a quick game plan.

Game Plan:
1. Backup the registry keys you will be modifying and create a Restore Point.
2. Create a thorough permissions portfolio for SPPSvc, ensuring that permissions aren't an issue now, and won't be in the future.
3. Make the modifications outlined in this GitHub document.
4. Monitor the issue and make any necessary follow-up changes, if needed.
5. Keep your backups, zip them up and save them in your OneDrive or email them to yourself. If this doesn't work or you need to restore them, Windows often doesn't touch much of the registry during upgrades, resets, DISM, or SFC.
6. Report any feedback so that I know what's working and we can better the community and this page with any additional learned information.
7. Report this issue to Microsoft, and hopefully they will get around to fixing SPPSvc.

<br/>

**Phase 1: Fortification – Backing Up Your System**

Think of this as building a digital bunker before the storm. We're taking precautions to ensure that, even in the worst-case scenario, you can safely retreat to a known good state.

1.  **Registry Key Backup – Safeguarding in case of issues:**<br/>
It's not anticipated that you will run into any issues requiring you to restore the old registry keys, but it's wise perform such backups before modification projects like this, as the registry is typically not touched, even in a Windows repair/reset.

    We need to create a copy of these keys to prevent data loss should something go wrong. Before you do anything, backup these registry keys:

    *   `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform`
    *   `HKLM\SYSTEM\CurrentControlSet\Services\sppsvc`
    *   `HKEY_USERS\S-1-5-20\Software\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform`

    Simply right-click on each key, select "Export," and save the resulting `.reg` file to a safe location. It doesn't matter where exactly, but choose a folder you'll remember.

2.  **System Restore – Creating an undo button, in case this doesn't work:**

    Some may dismiss System Restore as a vestige of a bygone era, but that's not the case, it is quite effective. When you end up in a situation where you wish you had a way to restore your PC to yesterday, System Restore does exactly that. If you've disabled System Restore in the past, now's the time to reconsider. Re-enable it if it isn't already, then create a restore point *before* proceeding with the following modifications. 

**Phase 2: Modifying the Software Protection Platform (SPP) Registry Keys**

Now, with our digital defenses in place, we can proceed with the necessary modifications to the SPP registry keys.

1.  **Fix Permissions – Ensuring Clear Communication:**

    While the existing permissions may appear perfectly fine, we're taking a proactive approach to eliminate potential permission-related issues. Microsoft support has historically identified permission issues as a source of SPP-related problems, so by completeing this step, we're ensuring this remains a comprehensive solution to the problem.

    Where to set these permissions:
    
    *   `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform`
    *   `HKLM\SYSTEM\CurrentControlSet\Services\sppsvc`
    *   `HKEY_USERS\S-1-5-20\Software\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform`
    *   `HKLM\Software\Microsoft\Windows NT\Schedule\Tasks\{GUID}` (Since these are not unique and are hard-coded for SPP, I will be revisiting this to provide additional registry keys you should ensure have Full Control for SPPSvc. Task Cache is a very intregal part of Task Scheduler and thus missing permissions here could negitvely ffect how SPPSvc behaves. Check back later.)

    Set the following registry permissions (right-click the key, select "Permissions"):

    *   `NT Service\sppsvc`: **Full Control**
    *   `Network Service`: **Full Control**
    *   `SYSTEM`: **Full Control**
    *   `Local Service`: **Read Only** (*Do NOT grant Full Control. Granting excessive permissions to Local Service could create a security vulnerability, allowing rogue services to tamper with SPP.*)

    Next, ensure that `Network Service` owns the registry keys by going to Advanced, and clicking Change next to Owner.

2.  **Adjusting SPP Variables**

    This is where we take direct control over the SPP's behavior, suppressing the RulesEngine's runaway loops and establishing a more predictable schedule.

    *   **Suppress the RulesEngine:**

        Create a new DWORD (32-bit) Value named `SuppressRulesEngine` and set its value to `1` in the following locations:

        *   `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform\Activation`
        *   `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform`
        *   `HKEY_USERS\S-1-5-20\Software\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform`

        This effectively disables the RulesEngine's default behavior, preventing it from launching scans and rescans arbitrarily. But, if you stop here and you don't change the default values, SPPSvc will literally play dead and act like a zombie, which is not what we want. This is where we set our own schedule.

    *   **Establish a Restart Schedule:**

        Rename the existing DWORD Value named `ActivationInterval` to `ActivationInterval.bak`. This is a way to keep it around should you need to check the old value.
        Create a new DWORD (32-bit) Value named `ActivationInterval` and set its value to `43200000` (decimal). This value is in milliseconds and represents 12 hours. It instructs SPPSvc how often to "wake up," check its licensing status, and trigger a service restart. As long as the RulesEngine is suppressed, SPPSvc will adhere to a rough version of this schedule. Rough meaning, it's programmed to add a time delay to actions so as a best practice standard, the concept being, that if all services do this, then there shouldn't ever be one specific time that your device freezes or grinds to a halt because of a bunch of tasks occuring all at once.

        *IMPORTANT: Do not set `ActivationInterval` to a value longer than 12 hours. Doing so may result in applications like Office or Store apps entering read-only mode and Windows preventing you from customizing your settings. 12 hours represents the upper limit for reliable license check-in times.*

    *   **Disable Error Control**

        Rename `ErrorControl` and `FailureActions` and add a `1` or a `z` at the end of their names, so that they read as `ErrorControl1`, etc. This prevents Windows Services Control Manager from attempting to continually restart SPP when it's not necessary.

    *   **Prevent UI Disruptions:**

        Set the value of `NoExpirationUX` to `1`. While rare, SPPSvc may occasionally fail to restart as scheduled. This setting provides a safety net, temporarily preventing Windows from making drastic UI changes (such as nag screens) due to a temporary lapse in license check coverage. This value is not guarunteed to provide much coverage.

    *   **Validate Key Components and Regenerate If Needed**

        `\SoftwareProtectionPlatform\Activation\Manual` needs to be `0` and `LicStatusArray` needs to exist. If you do not have these two registry values, this could be a big problem. DISM and SFC generally do not touch the registry. If you are missing `LicStatusArray` then you should try to force SPPSvc to regenerate the entire registry key:

        1.  Save the `SoftwareProtectionKey` 🠆 right-click Export
        2.  Then delete the key
        3.  Reboot windows 1-3 times until the key regenerates. This should hopefully force SPPSvc to regenerate all it's licensing information. Make sure you save that registry key via export in case deleting it causes any problems.

**Phase 3: Monitoring & Troubleshooting – Addressing Potential Additional Modifications Needed**

Even with the utmost care, unforeseen issues can sometimes arise. This section will be expanded to include additional changes meant to handle unique circumstances where the SPPSvc does not quite behave in the expected way. The goal is to bring the service back to normal intended behavior. If additional changes are needed, you will find that described here.

*   **The "Zombie" SPPSvc:**

    In some cases, SPPSvc may enter a dormant state, failing to restart itself or check in with licensing unless manually triggered. If you encounter this behavior, you'll need to create a scheduled task to force SPPSvc to restart periodically. *Note: This step will be detailed in a future update to this guide. Check back soon!*

*   **Dynamically Turning Off and On Rules Engine and Actions Module: _(early preview teaser)_**

    It may be necessary in certain situations that we got a step further and create a dynamic Task Scheduler profile which follows our own custom set of rules to turn off and on the Actions module and RulesEngine. After a certain period of time has elapsed, SPPSvc and certain modules can be turned off until our next scheduled check-in. *Note: This step will be detailed in a future update to this guide. Check back soon!*

    Please note that this page is currently a work in progress. Additional information and troubleshooting tips will be added as they become available.

<br/><br/>

**That's a wrap!**

Feel free to reach out if you encounter any issues or problems, but make sure you have double-checked the content I have provided here first. If you do require assistance, you will need to be able to detail how far you have gotten, what troubleshooting steps you completed on your own, and provide any logs and relevant information. Open a discussions item here on GitHub and tag me.



<p><br/></p>

### Troubleshooting:
* There is a possibility that SPPSvc will act dead and not do anything, won't restart itself, and won't check in with licensing without something manually restarting the service. If this happens to you, then you will need an SPPSvc restart task for Task Scheduler. I will upload more information about this later on. This page is unfinished.

<br/>

### Screenshots - References
(Click to enlarge.)<p></p>
<br/>
**Showing a PC which used to log SPP events every 2 min in an aggressive broken rules engine schedule, which caused a lot of IO, CPU, and process issues, and now logs SPP only when it starts on custom schedule. After which, it successfully consumes the licenses.**
<img src="https://github.com/user-attachments/assets/2074a778-b541-4b7b-a796-8677a7a34ccc" width=600 title="SPP (Software Protection Platform) Custom User Schedule - Fixes SPP-Security Event Log Issue" alt="" />

<img src="https://github.com/user-attachments/assets/6ca4bd11-0aac-4263-a100-241599c73fc1" width=600 title="" alt="" />

<img src="https://github.com/user-attachments/assets/b8ba5369-c5e6-4a01-a374-730091acccd4" width=600 title="" alt="" />

<br/>
<br/>

## FAQ's

**Q: I've heard whispers that the Software Protection Platform Service is a relic of the past, soon to be abandoned by Microsoft itself. Is this true?**

**A:** The exact inner workings of Redmond's product strategy often remain shrouded in mystery. However, reports of SPP's obsolescence are greatly exaggerated. When observing the intricate interactions between Windows and its services through tools like Process Monitor, one quickly realizes that SPP is deeply intertwined with the operating system's core functionality. While it might be tempting to simply disable it, doing so risks disrupting the delicate balance of the Microsoft software ecosystem.

Microsoft products are not designed to operate without SPP, and disabling it could lead to unforeseen consequences. You might find yourself locked out of downloading apps from the Microsoft Store, unable to open or view Office documents, or deprived of Microsoft-exclusive features like OneDrive, Copilot, or even the ability to personalize your desktop background. Customization, as many know, requires activation – SPP is how Windows knows the state of activation for a wide variety of products and programs. For example, it was recently observed that on several of our client's systems, after taking the 24H2 upgrade, SPP started writing errors to the Event Log stating that it was unable to consume the license due to a failure. Searching this error code shows many people reporting that this was caused by overusing their license for Office. Ie. they were licensed for 5 and had 6 activated on their Microsoft account – requiring them to remove the old inactive systems to resolve the error. This same exact error occurred on our client's systems, upon double-checking their account, it was clear they were not even close to overusing their licenses. We then decided to run an online repair of Office 365, which resolved the SPP error. This is evidence that SPP is directly associated with ensuring the correct licensure of all Microsoft products.

So, while the precise extent of SPP's reach remains an area ripe for further exploration, it's almost guaranteed that complete disabling of the service will result in Microsoft software either not functioning at all or being limited functionality. A far more graceful and effective approach is to follow the instructions provided here, allowing SPP to return to a more normal and manageable state.

**Q: What's the significance of the "100 years" rescheduling behavior?**

**A:** Ah, the riddle of the century! This has baffled many across the internet, it seems. I, too, have puzzled over the significance of this recurring rescheduling to dates nearly a century into the future. While some have speculated that 100 years represents the maximum allowable scheduling horizon within Task Scheduler, empirical evidence quickly dispels this notion; you can readily schedule tasks further out than that.

My initial thought was, could this be an accidental bug within SPPSvc and its modules – a coding oversight where a developer inadvertently hardcoded the "21" prefix for the 4 digit year instead of "20" (ie. 21xx vs 20xx)?

The problem is, the dates employed by SPP are not precisely 100 years into the future, but rather drift, being closer to 99 years, 11 months, and a few days. This hypothesis, though intriguing, doesn't entirely account for the observed discrepancy. An incorrect prefix with a negative calculation offset instead of a positive offset, remains the closest guess I can offer without reverse engineering SPPSvc itself. The exact underlying cause for the 100 years certainly beckons the curious mind.

Essentially, it's believed that the way SPPSvc is supposed to calculate the next action is based on an offset, but since the year prefix is incorrect, the mathematical calculation subtracts time instead of adding it, since you can't subtract from a negative. Thus, we end up with a date close to 100 years but a few weeks before today's today if we're ignoring the year factor. This could be wrong though.

**Q: Why is the "RulesEngine" cycle problematic? What tangible issues does it cause?**

**A:** The SPP's RulesEngine is designed to validate your Microsoft licenses against a series of predefined rules. It is designed to trigger as needed. However, when the service becomes entrapped within this bug cycle, it triggers rescans repeatedly. Each scan iterates through your local file system and registry, consuming considerable system resources: CPU cycles, disk I/O, and memory. You often won't see the CPU spike in SPPSvc itself, as it uses kernel-mode code in-memory to protect Microsoft genuine product key algorithms which could otherwise be reverse engineered if SPP conducted this work in user-space where the process could be dumped and examined.

The end result of this activity are a degradation in system responsiveness, and a mess of your Application Event Log. Applications may run slowly, or this repetitive process loop could interrupt background operations and starve important tasks from necessary system resources. For users with older hardware, or systems already running close to their resource limits, this RulesEngine loop could possibly even cause partial lockups and crashing, hence why many are taking to the forums and Reddit to complain about this. To date, Microsoft has never acknowledged this issue, addressed any of these concerns, or many any noticeable effort to remedy this problem, which is very concerning to me.

**Q: Is this a bug, or is the RulesEngine cycle intended behavior?**

**A:** Whether this behavior constitutes a "bug" in the strictest sense of the word remains a matter of conjecture. It is entirely possible that the initial design of SPP and its RulesEngine envisioned such a scenario under specific, unforeseen circumstances. However, the critical point is that regardless of the initial intent, the behavior has a detrimental impact on the user experience. From the perspective of the end-user, the RulesEngine's relentless cycling creates a clear and undeniable performance problem, necessitating intervention to restore the system to a usable state.

**Q: Why does suppressing the RulesEngine seem to resolve the problem? What's the mechanism at play?**

**A:** By creating the "SuppressRulesEngine" registry value, we are essentially instructing the SPP service to bypass, or ignore, the RulesEngine scheduling component altogether. It's like putting the service on a leash. We are then setting up our own rules for the Rules Engine. Instead of launching a full-scale rules evaluation on its own accord, SPP will adhere to a predefined schedule that is independent of whatever caused it to enter the problematic cycle. In addition, with an optional custom Task Scheduler task configured to stop SPPSvc after consumption and restart it in12 hours, SPP Svc will only scan when we want it to scan. 

While there doesn't appear to be any other consequences of suppressing the RulesEngine and creating our own custom rules, this does warrant further investigation. That being said, my team and I have tested this, and my preliminary evidence suggests that the core licensing functionality of SPP remains completely intact. The service continues to perform its fundamental duties of license verification and enforcement, albeit without the constant and resource-intensive RulesEngine evaluations.

Just keep in mind that 24 hours for restarting the service, with a 12 hour self-restart defined setting, is the maximum comfortable time for SPP and pushing the license scans beyond these time limits could create unintended, unstable, and unknown behavior within Windows.  

**Q: This sounds like an "unofficial" fix. Is it safe to implement, and what are the potential risks?**

**A:** You are correct to approach this resolution with caution. It is, indeed, an "unofficial" solution, meaning that it is not supported by Microsoft. As such, there are inherent risks associated with implementing it. The primary risk is the potential for unforeseen side effects or compatibility issues with future Windows updates. We cannot guarantee that suppressing the RulesEngine will not inadvertently impact other system components or introduce unexpected behavior.

**However, based on observed behavior and community feedback, the risks appear to be relatively low.** We have not encountered any widespread reports of critical errors or data loss resulting from this approach. That said, as long as you follow the backup steps and create a restore point, then the risk drops to near zero, because at any time you can restore the original behavior of SPPSvc by re-importing the registry key backups and rebooting your PC. Ultimately, the decision to proceed rests with you as the individual user, as you must weigh the potential benefits against any perceived potential risks. Everything you see here is provided for research purposes only, and is not intended to be used on servers or production equipment where application and registry changes could significantly impact other systems.

### Additional Notes
*   **Feedback:** We encourage users to **provide your feedback, observations, or any issues you may encounter** after implementing this fix. This will help us refine the documentation, add any new insights or improvements, and help us identify any potential problems.
*   **Testing:** We strongly want to emphasize the importance of testing your system after making the changes to ensure that everything is working as expected. Reboot several times. Launch Xbox games, purchased apps, and Office products over the course of several reboots – all while monitoring the Event Log for changes in how SPPSvc behaves and interacts with your system.
*   **Further Exploration:** In conclusion, there is still much to be understood about the intricacies of the SPP service and how its Rules Engine works. **We welcome further research and analysis from the community.**
