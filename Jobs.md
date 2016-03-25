# Les Jobs

## Les Jobs de base

Lancer un job:
```PowerShell
# en local
Start-Job -ScriptBlock { }

# en remote
Invoke-Command -ScriptBlock { } -ComputerName ... -AsJob
```

Manipuler les jobs:
```PowerShell
# Lister les jobs
Get-Job -ID/-Name

# Recuperer les resultats d'un job
Receive-Job -ID [ -Keep ]

Stop-Job
Remove-Job
Wait-Job
```

**Note:** Toutes les données des Jobs sont stockées en mémoire ! 


## Les Scheduled Jobs

```PowerShell
$option = New-ScheduledJobOption -waketorun -runelevated
$trigger1 = New-JobTrigger -once -at (get-date).addminutes(10)
$trigger2 = New-JobTrigger -atlogon

Register-ScheduledJob -ScheduledJobOption $option -Trigger $trigger1 -ScriptBlock { get-eventlog -logname security } -maxresultcount 5 -name localsecuritylog

Register-ScheduledJob -ScheduledJobOption $option -Trigger $trigger2 -ScriptBlock { get-process } -name proclist
```
