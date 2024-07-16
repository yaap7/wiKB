# How to reverse the mouse scroll direction in Windows 11

Based on: <https://gist.github.com/gagarine/d313ee6510009b3f3973c6e0929b1e1c>

To use "natural scrolling" (pad-style) on all mouses currently connected to the computer, execute this PowerShell snippet with admin rights and **reboot the computer** (seriously Microsoft...):

``` powershell
Get-PnpDevice -Class Mouse -PresentOnly -Status OK | ForEach-Object {
    "$($_.Name): $($_.DeviceID)";
    Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Enum\$($_.DeviceID)\Device Parameters" -Name FlipFlopWheel -Value 1;
    "+--- Value of FlipFlopWheel is set to " + (Get-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Enum\$($_.DeviceID)\Device Parameters").FlipFlopWheel + "`n"
}
```
