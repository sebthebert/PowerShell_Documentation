# Documentation PowerShell

## Base

Afficher la version PowerShell:
```PowerShell
$PSVersionTable
```

Obtenir de l'aide:
```PowerShell
Get-Help

Get-Help <cmd> -ShowWindow

Get-Help <cmd> -Examples

Get-Help *<cmd>*

Get-Command *<cmd>*
```

Mise a jour de l'aide:
```PowerShell
Update-Help
```

Avoir la liste des alias:
```PowerShell
Get-Alias
```

Afficher les logs...:
```PowerShell
Get-EventLog -LogName Application -Newest 10
```

`-Confirm` permet de demander confirmation avant action

`-WhatIf` affiche ce qui va etre effectué


### Exercices:

Quelle commande pour faire une résolution DNS ?
```PowerShell
Get-Command *dns* -type Cmdlet
```
--> `Resolve-DnsName`

Retrouver la fonction qui permet de modifier l'interface réseau ("NetAdapter")
```PowerShell
Get-Command set*adapt*
```
--> `Set-NetAdapter`

Et le parametre pour modifier l'adresse MAC dans cette commande ?
```PowerShell
Get-Help Set-NetAdapter
```
--> `-MacAddress`

Quelle commande pour reactiver une tache planifiée ?
```PowerShell
Get-Command *scheduledtask*
```
--> `Enable-ScheduledTask`

Quelle commande pour bloquer l'accès vers un partage pour un user specifique ?
```PowerShell
Get-Command *block*
```
--> `Block-SmbShareAccess`

Quelle commande pour effacer le cache local ("BranchCache") ?
```PowerShell
Get-Command *cache*
```
--> `Clear-BCCache`

Quelle commande pour afficher liste des règles firewall ?
```PowerShell
Get-Command get*firewall*
```
--> `Get-NetFirewallRule`

Et uniquement les règles actives ?
--> `Get-NetFirewallRule -Enabled true`

Quelle commande pour modifier le type de démarrage du service BITS en automatique ?
```PowerShell
Get-Help Set-Service
```
--> `Set-Service -Name BITS -StartupType Automatic`


## Le pipe

Connaitre les 'membres' (champs) retournés par une commande:
```PowerShell
Get-Service | Get-Member
```

Pour trier la sortie d'une commande par un de ces 'membres' (champs):
```PowerShell
Get-Service | Sort-Object status
```

Compter le nombre d'éléments d'une liste:
```PowerShell
Get-ChildItem | Measure-Object
```

Faire une sélection à partir d'une liste:
```PowerShell
<cmd> | Select-Object
```

### Exercices

```PowerShell
Get-Date | Get-Member
Get-Date | Select-Object -property dayofyear
```

Afficher les Hotfixes
```PowerShell
Get-Help hotfix
--> Get-Hotfix
Get-Hotfix | Get-Member
--> get-hotfix | Select-Object -property HotFixID,InstalledOn,InstalledBy
```

```PowerShell
Get-Help Scope
--> Get-dhcpserverv4scope -computername LON-DC1 | Select-Object -Property scopeid,subnetmask,name
```

Afficher les règles firewall actives avec seulement leur displayname,profile,direction,action:
```PowerShell
Get-NetFirewallRule -enabled true | Select-Object displayname,profile,direction,action
```

Quelle commande pour afficher les objets du cache DNS
```PowerShell
Get-Command get*dns*
```
--> Get-DnsClientCache

Afficher seulement le nom d'enregistrement:
```PowerShell
Get-DnsClientCache | Select-Object -Property Name,Type,TimeToLive
```


## Les imports/exports

```
Get-Command *convert*
Get-Command *export*
Get-Command *import*
```

### Exercices

Afficher les processus (id,name,vm) en cours d'execution dans l'ordre alphabetique inverse:
```PowerShell
Get-Process | Sort Name -Descending | ConvertTo-Html Name,Id,VirtualMemorySize,pm | Out-File procreport.html
```

```PowerShell
Get-EventLog -LogName System -Newest 10 | Export-Csv sys_eventlogs.csv
```

```PowerShell
Get-Service | Sort status -descending | Export-CliXML services.xml
```


## Les filtres

```PowerShell
GetService | Where { $_.Status -eq 'Running' }

GetService | Where { $_.Name.Length -lt 5 }
```

### Exercices

Récupérer les users dans l'AD qui ont un certain 'cn':
```PowerShell
Get-ADUSer -filter * -searchbase "cn=<...>"
```

Récupérer les logs du type 4624 (successfully logged on)
```PowerShell
Get-EventLog -LogName Security | Where eventid -eq 4624
```

Lister les volumes qui n'ont plus beaucoup d'espace (<100 Mo):
```PowerShell
Get-Volume | Get-Member
-> SizeRemaining
Get-Volume | Where SizeRemaining -lt 100MB 
```

```PowerShell
Get-Volume | Where { $_.SizeRemaining -gt 0 -and ($_.SizeReaining / $_.Size) -lt 0.99 }
```

```PowerShell
Get-ControlPanelItem -Category 'System and Security'
```

## Foreach...

```PowerShell
Get-ChildItem -path cert: -recurse | foreach getkeyalgorithm
```

