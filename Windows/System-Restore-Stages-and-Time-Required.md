## system restore stages in time required 

0. Windows logs off and winlogonui takes over, all other services are stopped. It then starts the System Restore service to begin the process. Black screen with white text
1. System Restore is initializing 
2. System Restore is restoring the registry 
3. System Restore is restoring files (sometimes skipped
Reboot
Back to lockscreen.

Also, they enabled DPMS (Display Power Management System) during all stages of System Restore, and your monitors will go to sleep after 5-10 min of no mouse movement. When they go off, don't be fooled into thinking Windows is rebooting. 

<p></p>

## Explanation:
**Step 1. “System Restore is initializing...”**
ETA: ~10 - 20 min, max. 

If it takes longer than 30 min and you didn't select a very far away Restore Point, the. it may have hung. Check the HDD activity light. <br/>

<br/>
<br/>

**Step 2. “System Restore is restoring the registry...”**
ETA: This takes the longest. ~10-30 min.
<p></p>
They may have mashed a bunch of other stuff in there. Who knows, they're such a mess. <br/>

<br/>
<br/>

**Step 3. “System Restore is restoring files...”**
ETA: ~5-8 min. This was the shortest stage. <br/>

<br/>
<br/>

**Step 4. Back to the lockscreen.**
There really isn't a lot of stages to System Restore. No progress indicators, no percentages, it really wouldn't kill them to add these, but they never will. It may seem frozen during certain stages but if the activity light is still flashing, then it's still working.


<br/>
<br/>
------
**Additional details:**
Doesn't really make a lot of sense, because restoring the registry on a 1,000+MB/s M.2 SSD such as many of our own shop PCs, shouldn't take that long. I promise, and this may sound negative, but I'm kind of getting sick of it, some pieces of Windows are the most inefficient pieces of garbage. They never change, they never update, they don't take advantage of modern performance and technology. You can watch some of these pieces of Windows use one core, 5% of the CPU, and nothing else, and then they take an ungodly amount of time to do so. They aren't multi-threaded, or designed to take advantage of modern systems from the last 15 years. 


<p></p>
<p></p>

Hey, Microsoft... 👋 
I know you don't care, but some suggestions for you would be: 
<p></p>
To me, initializing means that the process or service is starting up, so when it sits there for 10 minutes, it makes me want to reboot the computer because it obviously hasn't started and it instead got hung. You could do a lot better job by changing the message or adding the stage that says something like, “System Restore is preparing the restore operation...” The “initializing” message for the entire stage really does not fit. <br/>
<br/>
Also, how about some kind of percentage indicator. even if it's a rough guess, it's better than nothing. Surely you know how many items are in the array and approximately how much work needs to be done. A progress percent indicator goes a long ways! You have nothing here!<br/>
<br/>
But you won't change it, it's one of those legacy things that hasn't changed since Viata. Exact same UI, exact same messages, it's like there is a “hands-off” list of things that MS tells all engineers, “we do not touch or modify these things. We don't update them, we don't upgrade them, we don't modify them in any way, we don't make them modern or newer we don't do anything with them, so don't touch them!”
