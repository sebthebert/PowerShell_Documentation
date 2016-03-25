# Scripting PowerShell

Pour connaitre la politique d'execution des scripts PowerShell:
```PowerShell
Get-ExecutionPolicy
```

## Les Variables

Les variables sont stockées dans le PSDrive `variable:`.

Quelques exemples:
```PowerShell
$x1=100
$procs=get-process
$procs[0].name
$procs | sort VM -descending
"The content of x1 is $x1"
$x2=100
$x1 * $x2
$procs[0] | format-list *
$procs | gm
```


## Les paramètres

```PowerShell
[CmdletBinding()]
Param(
  [Parameter(Mandatory=$True)] [string]$ComputerName
)
```


## Verbose

```
write-verbose "<msg>"
```
Le message <msg> ne sera affiché que si on passe le parametre `-verbose`


## Les commentaires

```PowerShell
# une ligne de commentaire

<#
plusieurs lignes
de commentaires
#>
```


## Documentation

```PowerShell
<#
.SYNOPSIS
.DESCRIPTION
.PARAMETER
.EXAMPLE
#>
```

## Les Modules

```PowerShell
Env:PSModulePath
```

## Gestion des erreurs

```PowerShell
$ErrorActionPreference
```
permet de definir l'action a suivre apres une erreur

```PowerShell
Try {
} 
Catch {
}
```

```
$Error[0]
ErrorVariable
```

## Logique

If/Elsif/Else
```PowerShell
if ()
{
}
elseif ()
{
}
else
{}
```

Switch
```PowerShell
switch ()
{
  "" { }
  "" { }
  "" { }
}
```
peut executer plusieurs matchs sauf si on utilise `break`.

## Manipulation de String

```PowerShell
$str.ToUpper()

$str.ToLower()

$str.Replace("o", "*")

$str.Length()

# IndexOf trouve la premiere occurence (-1 si il ne trouve rien)
$str.IndexOf("P")

$str.Substring(8, 5)
```

```PowerShell
-replace
-join
-split
```

Documentation sur les String:
go microsoft.com 306159


## Les Regex

```PowerShell
[regex]::Matches("1X2Y3Z", "\D") -join("")
# => XYZ
```

http://www.powershelladmin.com/wiki/Powershell_regular_expressions#Class_Methods


## Les Dates

```PowerShell
$today = Get-Date
$today.Year
$today.AddDays(10)

# pour voir toutes les possibilités
$today | gm
```

## Autres

-Contains/-In
```PowerShell
$collection = "red","green","blue"

$collection -Contains "green"

$collection -In "blue"
```

Parametres par defaut pour des commandes
```PowerShell
$PSDefaultParameterValues = @{"Get-CimInstance:ClassName"="win32_bios"}

Get-CimInstance
```

```PowerShell
# Tout ce qu'il y a apres --% ne sera pas interprété par le Shell
--% 
```
