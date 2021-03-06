# Invoke-AtomicRedTeam

## Setup

### Install Atomic Red Team

Get started quickly with our simple Powershell [script](install-atomicredteam.ps1).

### Manual


`set-executionpolicy Unrestricted`

[PowerShell-Yaml](https://github.com/cloudbase/powershell-yaml) is required to parse Atomic yaml files:


`Install-Module -Name powershell-yaml`

`Import-Module .\Invoke-AtomicRedTeam.psm1`

## Getting Started

### Execute a Single Test

```powershell
$T1117 = Get-AtomicTechnique -Path ..\..\atomics\T1117\T1117.yaml
Invoke-AtomicTest $T1117
```

## Additional Examples

If you would like output when running tests using the following:

#### Informational Stream

```powershell
Invoke-AtomicTest $T1117 -InformationAction Continue
```

#### Verbose Stream

```powershell
Invoke-AtomicTest $T1117 -Verbose
```

#### Debug Stream

```powershell
Invoke-AtomicTest $T1117 -Debug
```

#### WhatIf

If you would like to see what would happen without running the test

```powershell
Invoke-AtomicTest $T1117 -WhatIf
```

#### Confirm

To run all tests without confirming them run using the Confirm switch to false

```powershell
Invoke-AtomicTest $T1117 -Confirm:$false
```

Or you can set your `$ConfirmPreference` to 'Medium'

```powershell
$ConfirmPreference = 'Medium'
Invoke-AtomicTest $T1117
```

## Generate All Tests

```powershell
[System.Collections.HashTable]$AllAtomicTests = @{}
$AtomicFilePath = 'C:\AtomicRedTeam\atomics\'  
Get-ChildItem $AtomicFilePath -Recurse -Filter *.yaml -File | ForEach-Object {
    $currentTechnique = [System.IO.Path]::GetFileNameWithoutExtension($_.FullName)  
    $parsedYaml = (ConvertFrom-Yaml (Get-Content $_.FullName -Raw ))
    $AllAtomicTests.Add($currentTechnique, $parsedYaml);
}
$AllAtomicTests.GetEnumerator() | Foreach-Object { Invoke-AtomicTest $_.Value -GenerateOnly }
```
