# Les Jobs

Lancer un job:
```PowerShell
# en local
Start-Job -ScriptBlock { }

# en remote
Invoke-Command -ScriptBlock { } -ComputerName ... -AsJob
```

Manipuler les jobs:
```PowerShell
Get-Job -ID/-Name

Stop-Job
Remove-Job
Wait-Job
```
