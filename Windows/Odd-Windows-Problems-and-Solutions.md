## Odd Windows Problems and Solutions
### A collaborative research log of misc odd problems encountered in the daily life of IT, focused on Windows.
This document is intended to include research notes on unusual Windows issues, bugs, and their solutions. It's inspired by a larger internal knowledge base we've been building over years of IT service, though that data requires anonymization before it can be shared publicly. This GitHub repo will serve as a collaborative research and documentation log for myself and my teams, but it won't contain the stories or much of the troubleshooting process that occured. A companion tech blog (launching soon) will provide more detailed explanations and case studies. You are strongly encouraged to read the write up before embarking on registry and system file modifications, as these may not even apply directly to your situation.

<br/>

## Let's get started...

#### ◇ Problem: Adding a startup entry to the `CurrentVersion\Run` key doesn't work. Neither by `.reg` file or `reg add`, the startup entry simply doesn't appear.
Attempts to add a program/script to Windows startup via the `CurrentVersion\Run` registry key using `.reg` files or `reg add`is unsuccessful, but no error is given. The registry entry simply does not appear despite both the `.reg` file and `reg add` stating “`x` has been sucessfully added to the registry.” **This is a syntax issue**, and it ***should*** be providing an error stating as such. I’m also unaware if this is an issue in prior versions of Windows, we only ran into this recently.

#### ◆ Solution: `SOFTWARE\Microsoft\Windows\CurrentVersion\Run` requires special syntax in the string.
Specificaly, **the string must use double slashes in the path**. This is strange because according to Microsoft developer documents, they warn against using double slashes, ie `\\`, in paths, they state this is bad practice and Windows chooses to compensate but this behavior isn't guarunteed. <p></p>
In any case –
* This works:  `"IT Startup Script"="C:\\IT\\Scripts\\Test.cmd"`
* This doesn't: `"IT Startup Script"="C:\IT\Scripts\Test.cmd"`
* If you want to add quotes inside the string: `"IT Startup Script"="\"C:\IT\Scripts\Test.cmd\""`&nbsp;&nbsp;(Not required.)

――――――――――――――――――――――――――――――――――――

#### ◇ Problem: Edit in Notepad Missing from Modern Right Click in Windows 11
#### ◆ Solution: (Research in progress, but upgrading to Windows 11 Dev fixed the issue. This isn't a viable solution. Check back later or feel free to comment.)
In-place Upgrade might also work. Switching to Windows 11 Dev is not a solution, and definitely not something I would suggest. I would recommend we export the class from regedit for importing on PCs where this issue has been introduced. It's likely handled during some state of the modern apps installation phase towards the end.

<!--
Template
<br/>

#### ◇ Problem: 
#### ◆ Solution: 

Clean and modern symbols: 
» Right-pointing double angle bracket
› Right-pointing single angle bracket
• Bullet point
◦ White bullet point
· Interpunct
● Black circle
○ White circle
▪ Black square
□ White square
✦ Four-pointed star
✧ Four-pointed star
◇ Diamond
◆ Black diamond

For a more decorative touch:
★ Black star
☆ White star
❖ Dingbat star
❀ Cherry blossom
✿ Flower
✼ Snowflake
✽ Flower
❦ Floral heart
❧ Floral heart ornament

For emphasis or highlighting:
➤ Heavy right-pointing arrow
➜ Right arrow
✓ Check mark
✔ Check mark
✕ Multiplication sign
✖ Multiplication sign
✚ Plus sign
✪ Circled star
▼ Black down-pointing triangle
▲ Black up-pointing triangle

For separators or dividers:
— Em dash
– En dash
― Horizontal bar
… Horizontal ellipsis
· Interpunct
• Bullet point
-->
