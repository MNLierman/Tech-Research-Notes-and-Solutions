## Odd Windows Problems and Solutions
### A collaborative research log of misc odd problems encountered in the daily life of IT, focused on Windows.
This section documents unusual Windows issues, bugs, and their solutions. It's inspired by a larger internal knowledge base we've been building over years of IT service, though that data requires anonymization before it can be shared publicly. This GitHub repo will serve as a collaborative research and documentation log, and won't contain the stories or much of the troubleshooting process that occured. A companion tech blog (launching soon) will provide more detailed explanations and case studies.

<br/>

## Let's get started...

#### **Problem: Adding a startup entry to the `CurrentVersion\Run` key doesn't work. Neither by `.reg` file or `reg add`, the startup entry simply doesn't appear.**
Attempts to add a program/script to Windows startup via the `CurrentVersion\Run` registry key using `.reg` files or `reg add`is unsuccessful, but no error is given. The registry entry simply does not appear despite both the `.reg` file and `reg add` stating “`x` has been sucessfully added to the registry.” **This is a syntax issue**, and it ***should*** be providing an error stating as such. I’m also unaware if this is an issue in prior versions of Windows, we only ran into this recently.

**Solution:** `SOFTWARE\Microsoft\Windows\CurrentVersion\Run` requires special syntax in the string. Specificaly, **the string must use double slashes in the path**. This is strange because according to Microsoft developer documents, they warn against using double slashes, ie `\\`, in paths, they state this is bad practice and Windows chooses to compensate but this behavior isn't guarunteed. <p></p>
In any case –
* This works:  `"IT Startup Script"="C:\\IT\\Scripts\\Test.cmd"`
* This doesn't: `"IT Startup Script"="C:\IT\Scripts\Test.cmd"`
* If you want to add quotes inside the string: `"IT Startup Script"="\"C:\IT\Scripts\Test.cmd\""`&nbsp;&nbsp;(Not required.)
