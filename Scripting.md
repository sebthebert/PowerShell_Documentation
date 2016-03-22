# Scripting PowerShell

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

