# PowerShell Remoting

``` powershell
$username = "contoso.local\admjdupond"
$password = "P@ssw0rd"
$secstr = New-Object -TypeName System.Security.SecureString
$password.ToCharArray() | ForEach-Object {$secstr.AppendChar($_)}
$cred = new-object -typename System.Management.Automation.PSCredential -argumentlist $username, $secstr
$sess = New-PSSession -ComputerName NYCVDC01.contoso.local -Credential $cred
Enter-PSSession $sess
```

