## Powershell commands and notes regarding their usage that doesn't fit in other categories
Information here may be moved around and reorganized at any time. If something is missing, use search or drill down by expected category.

<br/>

### Get & Set Computer Name (Hostname)
Windows Settings app and sysdm.cpl would not let me set a short PC name. Not sure if Microsoft changed this to enforce a minimum, but the name I wanted was 5 characters. Should have been fine, but had to use Powershell.

**Set name in Powershell:** `Rename-Computer -NewName "ExamplePCName-01"` <br/>
**Set name using WMI:** `wmic computersystem where name="%computername%" call rename name="ExamplePCName-01"` <br/>

**Get name (universal:** `[Environment]::MachineName` <br/>
**Get name (only Windows):** `[System.Net.Dns]::GetHostName()` <br/>
**Get Name & IPs:** `[System.Net.DNS]::GetHostByName('')` <br/>

<br/>

### Fixing Truncated Powershell `...` Output
These should work in a lot of situations, but for list items, truncated with `...` see further below.
`Out-String -width 9999` <br/>
`Format-Table -Wrap -Autosize` <br/>
`ConvertTo-Csv` <br/>

**To prevent truncating lists of items/strings:**
Set array size to unlimited or desired number: `$FormatEnumerationLimit=-1` <br/>
