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


## Les Providers

Les Providers permettent d'accéder à différentes sources de données à la manière d'un Filesystem.

On peut alors lancer les commandes cd, dir, ls...

Pour lister les Providers
```PowerShell
Get-PSProvider
```

Pour lister les Drives fournis par ces Providers
```PowerShell
Get-PSDrive
```

En important le module ActiveDirectory, on ajoute un Provider Active Directory
```PowerShell
Import-Module ACtiveDirectory
```

### Exercices

Créer un dossier sans utiliser la commande mkdir ou un de ses aliases
```PowerShell
New-Item -itemtype directory c:\ScriptOutput
```

Créer un nouveau PSDrive et le mapper sur le répertoire déjà créé
```PowerShell
New-PSDrive -Name S -PSPRovider FileSystem -Root C:\ScriptOutput
```

Créer une nouvelle clé (Scripts) dans HKCU\Software
```PowerShell
New-Item HKCU:\Software\Scripts
```

Ajouter une clé/valeur dans HKCU:\Software\Scripts
```PowerShell
New-ItemProperty HKCU:\Software\Scripts -Name "Windows PowerShell" -value "test"

Get-ItemProperty HKCU:\Software\Scripts | Select-Object * -exclude pspath,psparentpath,pschildname,psdrive,psprovider
```


```PowerShell
Set-Item -Path wsman:\localhost\service\maxconnections -value 250
```

## Formatage

### Format-Wide (FW)
```PowerShell
Get-Service | Format-Wide
Get-Service | Format-Wide -Col 5
Get-Service | FW -autosize
```

### Format-List (FL)
```
Get-Process | Format-List -Poperty Name,ID
Get-Service | FL -Poperty *
```

### Format-Table (FT)
```PowerShell
Get-Service | Format-Table -Prop Name,DisplayName
Get-Service | FT Name -Prop Name,DisplayName,Status -Wrap
Get-EventLog -logname security -newest 50 | FT -property eventid,timewritten,message
```

### Out-GridView (uniquement si on a PS ISE sur la machine)
```PowerShell
Get-Process | Out-GridView
```

### Exercices

Computername,Description,Domain,Manufacturer,Model,number of processors,installed physical memory (GB)

```PowerShell
Get-CimClass
Get-CimInstance  -class Win32_ComputerSystem | get-member
Get-CimInstance  -class Win32_ComputerSystem | Format-List -Property Name,Description,Domain,Manufacturer,Model,Numberofprocessors,@{n='TotalPhysicalMemory';e={$_.TotalPhysicalMemory / 1GB}}
```

Afficher les processus par 'BasePriority'
```PowerShell
Get-Process | Sort BasePriority | FT -GroupBy BasePriority
``` 

Afficher les .exe de c:\windows par ordre decroissant 
```PowerShell
Get-ChildItem C:\windows | where { $_.extension -eq '.exe' } | sort length -descending | FT -property name,@{name='Size(KB)';expression={ $_.length / 1KB }} -auto
```
Ou plus simple et mieux formaté...
```PowerShell
Get-ChildItem C:\windows\*.exe | Sort-Object Length -Descending | FT -property name,@{name='Size(KB)';expression={ $_.length / 1KB };formatString='N2'} -auto
```

## CIM & WMI

WMI = Windows Management Instrumentation
CIM = Common Information Model (plus recent, depuis Win7)

```PowerShell
Get-WmiObject -namespace root\cimv2 -list -recurse

Get-WmiObject -Class Win32_LogicalDisk -Filter "DriveType=3"
Get-CimInstance -ClassName Win32_LogicalDisk -Filter "DriveType=3"
```

Pour avoir le contenu d'une classe, on peut utiliser `Get-Member`
```PowerShell
Get-WmiObject -Class Win32_Service | Get-Member
```

```PowerShell
Get-WmiObject -Class Win32_OperatingSystem | Invoke-WMiMethod -name win32shutdown -argument 0
```

Pour lister les methodes et les propriétés d'une classe
```
Get-WmiObject -Class <classname> | Get-Member -Type Method
Get-WmiObject -Class <classname> | Get-Member -Type Property
```

### Exercices

Lister les adresses configurées en DHCP
```PowerShell
Get-WmiObject -class Win32_NetworkAdapterConfiguration | Where DHCPEnabled -eq 'True'
```

Récupérer la version, servicepackmajorversion et le buildnumber de l'OS
```PowerShell
Get-CimInstance -ClassName Win32_OperatingSystem | FT -Property Version,ServicePackMajorVersion,BuildNumber -auto
```

Récupérer les informations utilisateur suivantes: caption,domain,sid,fullname,name
```PowerShell
Get-CimInstance -classname Win32_UserAccount | Select-Object -property cpation,domain,sid,fullname,name
```
