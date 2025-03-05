## Research Notes Concerning Feedback I've Given to Microsoft and Their Relation to Ongoing Issues
#### Includes on-going issues that have not been addressed yet.

<br/>

## My Feedback Regarding: Microsoft's Article “Guidance on disabling system services on Windows IoT Enterprise”
([Article Link]([url](https://learn.microsoft.com/en-us/windows/iot/iot-enterprise/optimize/services)))
This is a great guide and I appreciate the docuemtnation efforts by Microsoft. However, despite only disabling services that are notated as safe to disable, DISM stopped working on the device, and required a complete services reset. The following is my feedback to Microsoft:

“This guide is a GREAT start, and I must commend the authors and managers for approving or initiating this effort. It’s refreshing to see Microsoft provide clear documentation - a much-needed resource compared to guides from third parties.

That said, I’m giving it a thumbs down due to significant omissions. Key services, like WSAIFabricSvc, are missing.

More importantly, the guide fails to include dependencies and components that trigger errors when services are disabled. For instance, after following the guide, DISM repeatedly failed with "1058 Class Not Registered," indicating a disabled service. Despite enabling numerous services, the error persisted, and I ran out of time to identify the culprit. I'm not saying take this article down, but it needs to be reviewed and appropriate dependencies documented. I only disabled the ones with green checkmarks next to them and DISM stopped working thereafter.”
