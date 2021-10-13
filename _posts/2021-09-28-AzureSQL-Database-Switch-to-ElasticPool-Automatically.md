---
layout: post
title: "Powershell AZ Module Update"
date: 2021-09-28 13:20:00 +0800
categories: azure
tags: azure powershell automation azuresql elasticpool
---


```[powershell]
$connectionName = "AzureRunAsConnection"
$servicePrincipalConnection = Get-AutomationConnection -Name $connectionName

$logonAttempt = 0
$logonResult = $False

while(!($connectionResult) -And ($logonAttempt -le 10))
{
    $LogonAttempt++
    #Logging in to Azure...
    $connectionResult = Connect-AzAccount `
                           -ServicePrincipal `
                           -Tenant $servicePrincipalConnection.TenantId `
                           -ApplicationId $servicePrincipalConnection.ApplicationId `
                           -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint

    Start-Sleep -Seconds 30
}

$servers = Get-AzResourceGroup | Get-AzSqlServer

Write-Host "Elastic Pool Enforcement Started"

$servers | ForEach-Object {
	$serverName = $_.ServerName
	$resourceGroupName = $_.ResourceGroupName
	
	Write-Host "Probing Server: " $serverName " (Resource Group: " $resourceGroupName ")"
	
	$elasticPool = Get-AzSqlElasticPool -ResourceGroupName $resourceGroupName -ServerName $serverName
	$elasticPoolName = $elasticPool.ElasticPoolName
	
	Write-Host "Found ElasticPool: " $elasticPoolName " (DB Server: " $serverName ")"
	
	$sqlserver = Get-AzSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName
	$nonElastic = $sqlserver | where { $_.skuName -ne "ElasticPool" -and $_.skuName -ne "System" }
	$nonElastic | ForEach-Object {
		
		Write-Host "Found Database not in Pool: " $_.DatabaseName " (DB Server: " $serverName "). Moving to Pool: " $elasticPoolName
		
		$database = Set-AzSqlDatabase -ResourceGroupName $resourceGroupName `
							-ServerName $serverName `
							-DatabaseName $_.DatabaseName `
							-ElasticPoolName $elasticPoolName
		
		$date = Get-Date -Format "dd-MM-yyyy HH:mm"
		Write-Host "[" $date "] Database: " $database.DatabaseName " has been moved to the Sku: " $database.skuName
	}		
	
	Write-Host "Elastic Pool Enforcement Completed"
}

```
