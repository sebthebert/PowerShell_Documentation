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
