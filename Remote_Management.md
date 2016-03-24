# Remote Management

Le proctocole utilisé est WS-MAN (WebService MANagement) et il utilise HTTP(defaut)/HTTPS.

Le service lancé est `WinRM`.

`Enable-PSRemoting`

Pour se connecter en PowerShell a un ordinateur distant
```PowerShell
Enter-PSSession -ComputerName <computername>
```

Lancer une commande PowerShell sur plusieurs machines
```PowerShell
Invoke-Command -ComputerName <srv1>,<srv2>,<srv3> -ScriptBlock { <commande> }
```

Importer un module depuis un autre serveur
```PowerShell
$dc=New-PSSession -computername LON-DC1
Import-Module -pssession $dc -name smbshare -prefix dc
Get-dcSmbShare
Get-SmbShare
```
Si on ne met pas de préfixe, ca masque completement le fait qu'on fait les actions sur une autre machine !
