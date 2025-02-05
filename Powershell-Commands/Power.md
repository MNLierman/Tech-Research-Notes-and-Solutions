## Powershell Commands - Power Related

<br/>

### WMI Alises

```
Set-Alias -Name gwmi -Value Get-WmiObject -Option AllScope -Force
Set-Alias -Name swmi -Value Set-WmiInstance -Option AllScope -Force
Set-Alias -Name iwmi -Value Invoke-WmiMethod -Option AllScope -Force
```

<br/>

### Query active power plans in formatted wordwrapped table

The `-wrap` argument seems to be important here, as for whatever reason a autoformatted table still cuts off / truncates text, not sure why Microsoft thought this was a good idea.

`Get-WmiObject -Namespace Root\CIMV2\power -Class win32_powerplan  -Filter isActive=’true’ | select ElementName , Description, InstanceID | Format-Table -Wrap -Autosize`

<br/>

