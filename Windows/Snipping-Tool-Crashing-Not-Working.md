## Research Notes About Snipping Tool - Crashing, Errors, and Overall Snipping Tool Not Working
#### I was going to merge these notes into another one but opted to keep this separate. A blog post will go up about this shortly. However, essentially, snipping tool is one of the most finicky and most error prone of the Microsoft apps that I've ever experienced. 

<br/>

### Snipping Tool Error 80070424, Not Opening, Crashes Without an Error Message
Few things frustrate me more in the digital realm than vague error messages – the kind that tell you absolutely nothing about the problem, let alone how to solve it. Then, sometimes, you’re met with an error so elegantly cryptic, it almost feels like a riddle, take for example `0x4D5` which quite literally means “An error occurred; the process will be retried.” **Thanks, Microsoft. I got the memo that _something_ went wrong. Care to elaborate? No?**

This lack of clarity feels absurd, considering Microsoft employs some of the most talented minds in tech. Would it be too much to expect good error practices from them, esepcially considering a significantly large amount of their user-base is managed by IT professionals just like me. A bit of meaningful context – a whisper of a clue about what actually happened, would make a world of difference.

*Don't get me going...* Let's just jump right into this one. The error the service Snipping Tool relies on either isn’t there or can’t be found. In more technical terms, a service called CaptureService kicks in at login to spawn a temporary child "per-user service." This service is crucial for Snipping Tool to work, yet nowhere in Microsoft’s documentation is this even mentioned.
The error could easily say:
"App Host error: Snipping Tool encountered error . The required service, CaptureService, could not be located."
Imagine how much simpler troubleshooting would be with just that additional sentence.
And this isn’t an isolated issue—it’s emblematic of a larger problem. In another case, I encountered persistent DISM failures when certain services were disabled. Hours of searching across multiple days turned up sparse clues about which service DISM relies on. Even the usual suspects—Windows Update, BITS, DismHost—checked out fine. My current hunch? It's yet another one of those ephemeral "per-user services."
Ironically, Microsoft itself offers guidance on which services can be disabled for IoT Enterprise editions. But follow their advice, and you might cripple essential tools like DISM, leaving them incapable of servicing or creating images. How’s that for helpful?
I’ll dive into those intricacies in a separate post, but for now, let’s circle back to Snipping Tool. If CaptureService is disabled, here’s the type of error you can expect when it attempts to create its child process:



#### This has got to be one of my biggest gripes with Windows right now, error messages that are non-descript, and provide no information on how to proceed with troubleshooting. Just an error code, and sometimes the error quite literally means "an error occured, the process will be retired." Thanks Microsoft, I know an errror occured. How about logging a little bit about WHAT happened? Of course not, that's too complicated for a company of Microsoft's stature, of whom employs some of the most talented engineers on this planet.

Let's not get me going. The error `80070424` literally means that the required service cannot be located or was deleted. In Windows 10+, a service called CaptureService triggers the creation of a temporary child “per-user service” on login, which is required by Snipping Tool. No where in the documentation does it mention that Snipping Tool requires this service. Further, this is exactly what I mean by Microsoft's habit of giving no information and no context. The error could state WHAT service it's looking for that it couldn't find. How about something like:
"App Host provided error: SnippingTool encountered unhandled error 80070424. The required service, CaptureService, could not be located."

That would make troubleshooting a million times easier. This is a problem all over. In another investigation, which I am still working on, DISM can fail to complete most of it's tasks if certain services are disabled. I've searched for 6+ hours over multiple days trying to find more information about WHAT service it's expecting. The standard WU, BITS, DismHost, etc etc are all fine. So far, I believe it's been narrowed down to another one of these per-user services. In another article, Microsoft discusses services that are safe to disable for IoT Enterprise editions, which is essentially stating, that this is a list of services that are generally not needed for minimal user machines. Following that article results in DISM breaking and being unable to create and service images. I will discuss this in another post.

**Here is a sample of the Snipping Tool error one would expect to find if the Capture Service is disabled and not creating it's child per-user service.**

```
Fault bucket , type 0
Event Name: MoAppCrash
Response: Not available
Cab Id: 0

Problem signature:
P1: Microsoft.ScreenSketch_11.2307.52.0_x64__8wekyb3d8bbwe
P2: praid:App
P3: 11.2307.52.0
P4: 65bd666e
P5: combase.dll
P6: 10.0.26100.3360
P7: 838f05bb
P8: 80070424
P9: 00000000000e8bf4
P10: 
```
