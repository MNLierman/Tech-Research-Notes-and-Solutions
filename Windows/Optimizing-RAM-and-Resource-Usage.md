## Optimzing RAM and Resource Usage in Windows 11

#### Work in progress note combining information and research for reducing resource and RAM usage in Windows.

#### If you have more than 16GB of RAM, many of these changes may not be for you, as you may not see much benefit from these. While you may decrease RAM usage, you may also loose some stability in the process.

<br/>

## RAM Optimization Strategy Outline

**1. Optimizing Applications for RAM Usage**
   * **Potential Benefits:**
     * If one is willing to individually review each application thoroughly and inspect the settings for RAM saving options, as well as disabling startup items, this can definitely be a huge benefit. Eg. Edge RAM controls helps ensure that even with a lot of tabs open, resources are not used much above the desired level.
   * **Considerations:**
     * Don't set RAM controls too low. Depends on how many tabs and windows you expect to keep open.
     * Eg. In Edge, setting the RAM controls too low can negatively impact performance, as the cached files and data is immediately discarded, causing the system to have to pick it back up again on the next open tab. DNS resolution speeds and website load times could be slower.
    
**2. Startup and Process Tweaks (e.g., Windows Server Approach):**

Windows Server Approach: Disallow startup processes except for services. Startups will need to be run through Task Scheduler or a startup script.

   * **Potential Benefits:**
     * Reduced startup time.
     * Lower memory footprint.
     * Improved system responsiveness.
   * **Considerations:**
     * Some startup apps are required for certain functions, it's important that these are encapsulated into an alternative startup mode.
     * Keep in mind: The server version of windows is designed for a different use case.

**3. Combining Svchost Processes into Groups:**
   * **Potential Benefits:**
     * Reduced overhead by combining `svchost` processes into groups.
   * **Considerations:**
     * Could decrease stability; while this is unlikely in most cases, an unexpected unhandled crash in one service *could* crash the entire group. However, Windows 11 has great error handling within services.

**4. Combining Single-Service Svchost Groups:**
   * **Potential Benefits:**
     * Elimination of unnecessary `Svchost` processes.
     * Simplified service management.
   * **Considerations:**
     * If Windows is unaware of the new group and what is combined, the functions may not work any longer.
     * Security implications if services with different permission levels are grouped.
     * Thorough testing is important.

**5. Disabling Unneeded Services:**
   * **Potential Benefits:**
     * Significant RAM savings.
     * Reduced CPU usage.
     * Improved security by reducing the attack surface.
   * **Considerations:**
     * Careful identification of truly unneeded services is essential.
     * Disabling critical services can lead to system instability or application failures.
     * It is very important to document what services are disabled.

**6 Disabling User-Mode Service Creation for Certain user Services:**
   * **Potential Benefits:**
     * Prevents unwanted background processes from consuming resources.
     * Enhances security by disabling/removing unused features these groups were targeting, which also reduces attack surface.
   * **Considerations:**
     * Applications that rely on these user-mode services will fail to run.
     * This would require an understanding of what services are and are not needed.

**7. Other Windows and Kernel Tweaks:**
   * **Potential Benefits:**
     * Fine-grained control over system resource usage.
     * Improved performance and responsiveness.
   * **Considerations:**
     * Requires in-depth knowledge of Windows internals.
     * Incorrect tweaks can lead to system instability or data corruption.
     * Kernel level tweaks are very risky.

<br/>

### Overall Considerations:

* **Testing:**
    * Spin up VMs and test changes there before implementing them on a production machine.
    * Test with a wide range of applications and workloads.
    * Test for periods of time.
* **Documentation:**
    * Document every tweak and change.
    * This will be invaluable for troubleshooting and reverting changes.
* **Backup:**
    * Create regular system backups. System Restore can be a valuable asset.
    * Have a recovery plan in case of unexpected issues.
* **Incremental Changes:**
    * Implement tweaks incrementally.
    * Monitor performance and stability after each change.
* **Security:**
    * Always consider the security implications of any changes.
    * Reducing the attack surface is good, but make sure it does not create new vulnerabilities.

<br/>

### The Tweaks

*Some great information will go here soon.*
    
