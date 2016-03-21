# Documentation PowerShell

## Base

Afficher la version PowerShell:
```
$PSVersionTable
```

Obtenir de l'aide:
```
Get-Help

Get-Help <cmd> -ShowWindow

Get-Help <cmd> -Examples

Get-Help *child*

Get-Command *<cmd>*
```

Mise a jour de l'aide:
```
Update-Help
```

Avoir la liste des alias:
```
Get-Alias
```

Afficher les logs:
```
Get-EventLog -LogName Application -Newest 10
```

`-Confirm` permet de demander confirmation avant action
`-WhatIf` affiche ce qui va etre effectué

### Exercices:

Quelle commande pour faire une résolution DNS:
```
Get-Command *dns* -type Cmdlet
```
--> `Resolve-DnsName`

Retrouver la fonction qui permet de modifier l'interface réseau ("NetAdapter")
```
Get-Command set*adapt*
```
--> `Set-NetAdapter`

Et le parametre pour modifier l'adresse MAC dans cette commande:
```
Get-Help Set-NetAdapter
```
--> `-MacAddress`

Quelle commande pour reactiver une tache planifiée:
```
Get-Command *scheduledtask*
```
--> `Enable-ScheduledTask`

Quelle commande pour bloquer l'acces vers un partage pour un user specifique:
```
Get-Command *block*
```
--> `Block-SmbShareAccess`

Quelle commande pour effacer le cache local ("BranchCache"):
```
Get-Command *cache*
```
--> `Clear-BCCache`

Quelle commande pour afficher liste des règles firewall:
```
Get-Command get*firewall*
```
--> `Get-NetFirewallRule`

Et uniquement les actives:
--> `Get-NetFirewallRule -Enabled true`

Quelle commande pour modifier le type de demarrage du service BITS en automatique:
```
Get-Help Set-Service
```
--> `Set-Service -Name BITS -StartupType Automatic`


## Le pipe

Connaitre les 'membres' (champs) retournés par une commande:
```
Get-Service | Get-Member
```

Pour trier la sortie d'une commande par un de ces 'membres' (champs):
```
Get-Service | Sort-Object status
```

Compter le nombre d'éléments d'une liste:
```
Get-ChildItem | Measure-Object
```

Faire une sélection à partir d'une liste:
```
| Select-Object
```

### Exercices

```
Get-Date | Get-Member
Get-Date | Select-Object -property dayofyear
```

```
Get-Help hotfix
--> Get-Hotfix
Get-Hotfix | Get-Member
--> get-hotfix | Select-Object -property HotFixID,InstalledOn,InstalledBy
```

```
Get-Help Scope
--> Get-dhcpserverv4scope -computername LON-DC1 | Select-Object -Property scopeid,subnetmask,name
```

Afficher les règles firewall actives avec seulement leur displayname,profile,direction,action:
```
Get-NetFirewallRule -enabled true | Select-Object displayname,profile,direction,action
```

Quelle commande pour afficher les objets du cache DNS
```
Get-Command get*dns*
```
--> Get-DnsClientCache

Afficher seulement le nom d'enregistrement:
```
Get-DnsClientCache | Select-Object -Property Name,Type,TimeToLive
```
