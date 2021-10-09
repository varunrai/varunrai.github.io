---
layout: post
title: "Powershell AZ Module Update"
date: 2021-08-20 23:20:00 +0800
categories: azure
tags: azure powershell module
---

Recently was trying to build the Azure Bicep templates and tried executing them from Powershell. In order to use Bicep the AZ powershell modules should be using v5.6.0 or later. 

Check your AZ module version

```[powershell]
Get-InstalledModule -Name Az
```

Check specific AZ sub-modules version

```[powershell]
Get-InstalledModule -Name Az.* -listAvailable
```


Here is the snippet of the powershell script that lets you update the AZ module to latest versions. 

Firstly, Launch Powershell in Administrator mode.

```[powershell]

Get-Module -Name az.* -ListAvailable |
  Where-Object -Property Name -ne ‘Az.’ |
  ForEach-Object {
      #Get the existing version
      $existingVersion = [Version] $_.Version
      
      # Query the new version of the module
      $newVersion = [Version] (Find-Module -Name $_.Name).Version
  
      # Check if the versions are different 
      if ($newVersion -gt $existingVersion) {
          Write-Host -Object "Updating $_ Module from $existingVersion to $newVersion"
          # Install the new version
          Update-Module -Name $_.Name -RequiredVersion $newVersion -Force
          
          # Delete the old version
          Uninstall-Module -Name $_.Name -RequiredVersion $existingVersion -Force
      }
  }

```
