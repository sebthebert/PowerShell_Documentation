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
