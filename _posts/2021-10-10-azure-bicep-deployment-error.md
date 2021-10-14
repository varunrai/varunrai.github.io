---
layout: post
title: "Azure: Bicep Deployment Error - Another operation on this or dependent resource is in progress"
date: 2021-10-10 07:15:00 +0800
categories: azure
tags: azure bicep deployment error arm templates
---

I had been working on converting the ARM templates into Bicep by writing from scratch. One of the reason was the learning curve (by writing from scratch it will give a better understanding) and the other was complex ARM templates do give an error when trying to decompile.


I noticed one annoying issue for a simple tasks as deploying a virtual network with multiple subnets - dependency / another operation in progress [see below error]. As far as i have read, Bicep should be able to calculate the dependencies automatically without explicitly defining in the code but that does not work always.

![bicep error](/images/azure-bicep-dep-error.jpg "Bicep Deployment Error")

**Solution:**
The updated code contains a **dependsOn** parameter (similar to ARM Templates) as part of the subnet blocks (except the first subnet). What this allows is to create a sequential deployment of the subnets within the virtual network.


**Here is the original code:**
```json
resource firewallSubnetResource 'Microsoft.Network/virtualNetworks/subnets@2021-02-01' = {
  name: '${mainVirtualNetworkResource.name}/${mainVirtualNetwork.firewallSubnetName}'
  properties: {
    addressPrefix: mainVirtualNetwork.firewallSubnetSpace
    delegations: []
    privateEndpointNetworkPolicies: 'Disabled'
    privateLinkServiceNetworkPolicies: 'Disabled'
  }
}

resource appSubnetResource 'Microsoft.Network/virtualNetworks/subnets@2021-02-01' = {
  name: '${mainVirtualNetworkResource.name}/${mainVirtualNetwork.mainSubnetName}'
  properties: {
    addressPrefix: mainVirtualNetwork.mainSubnetSpace
    delegations: []
    privateEndpointNetworkPolicies: 'Disabled'
    privateLinkServiceNetworkPolicies: 'Disabled'
  }
}

resource managementSubnetResource 'Microsoft.Network/virtualNetworks/subnets@2021-02-01' = {
  name: '${mainVirtualNetworkResource.name}/${mainVirtualNetwork.managementSubnetName}'
  properties: {
    addressPrefix: mainVirtualNetwork.managementSubnetSpace
    delegations: []
    privateEndpointNetworkPolicies: 'Disabled'
    privateLinkServiceNetworkPolicies: 'Disabled'
  }
}

resource bastionSubnetResource 'Microsoft.Network/virtualNetworks/subnets@2021-02-01' = {
  name: '${mainVirtualNetworkResource.name}/${mainVirtualNetwork.bastionSubnetName}'
  properties: {
    addressPrefix: mainVirtualNetwork.bastionSubnetSpace
    delegations: []
    privateEndpointNetworkPolicies: 'Disabled'
    privateLinkServiceNetworkPolicies: 'Disabled'
  }
}

resource aaddsSubnetResource 'Microsoft.Network/virtualNetworks/subnets@2021-02-01' = {
  name: '${mainVirtualNetworkResource.name}/${mainVirtualNetwork.aaddsSubnetName}'
  properties: {
    addressPrefix: mainVirtualNetwork.aaddsSubnetSpace
    delegations: []
    privateEndpointNetworkPolicies: 'Disabled'
    privateLinkServiceNetworkPolicies: 'Disabled'
  }
}

resource mainVirtualNetworkResource 'Microsoft.Network/virtualNetworks@2020-11-01' = {
  name: virtualNetworkName
  location: location
  properties: {
    addressSpace: {
      addressPrefixes: [
        mainVirtualNetwork.addressSpace
      ]
    }
    enableDdosProtection: false
  }
}
```

**Updated code:**
```json
resource firewallSubnetResource 'Microsoft.Network/virtualNetworks/subnets@2021-02-01' = {
  name: '${mainVirtualNetworkResource.name}/${mainVirtualNetwork.firewallSubnetName}'
  properties: {
    addressPrefix: mainVirtualNetwork.firewallSubnetSpace
    delegations: []
    privateEndpointNetworkPolicies: 'Disabled'
    privateLinkServiceNetworkPolicies: 'Disabled'
  }
}

resource appSubnetResource 'Microsoft.Network/virtualNetworks/subnets@2021-02-01' = {
  name: '${mainVirtualNetworkResource.name}/${mainVirtualNetwork.mainSubnetName}'
  properties: {
    addressPrefix: mainVirtualNetwork.mainSubnetSpace
    delegations: []
    privateEndpointNetworkPolicies: 'Disabled'
    privateLinkServiceNetworkPolicies: 'Disabled'
  }
  dependsOn: [
    firewallSubnetResource
  ]
}

resource managementSubnetResource 'Microsoft.Network/virtualNetworks/subnets@2021-02-01' = {
  name: '${mainVirtualNetworkResource.name}/${mainVirtualNetwork.managementSubnetName}'
  properties: {
    addressPrefix: mainVirtualNetwork.managementSubnetSpace
    delegations: []
    privateEndpointNetworkPolicies: 'Disabled'
    privateLinkServiceNetworkPolicies: 'Disabled'
  }
  dependsOn: [
    appSubnetResource
  ]
}

resource bastionSubnetResource 'Microsoft.Network/virtualNetworks/subnets@2021-02-01' = {
  name: '${mainVirtualNetworkResource.name}/${mainVirtualNetwork.bastionSubnetName}'
  properties: {
    addressPrefix: mainVirtualNetwork.bastionSubnetSpace
    delegations: []
    privateEndpointNetworkPolicies: 'Disabled'
    privateLinkServiceNetworkPolicies: 'Disabled'
  }
  dependsOn: [
    managementSubnetResource
  ]
}

resource aaddsSubnetResource 'Microsoft.Network/virtualNetworks/subnets@2021-02-01' = {
  name: '${mainVirtualNetworkResource.name}/${mainVirtualNetwork.aaddsSubnetName}'
  properties: {
    addressPrefix: mainVirtualNetwork.aaddsSubnetSpace
    delegations: []
    privateEndpointNetworkPolicies: 'Disabled'
    privateLinkServiceNetworkPolicies: 'Disabled'
  }
  dependsOn: [
    bastionSubnetResource
  ]
}

resource mainVirtualNetworkResource 'Microsoft.Network/virtualNetworks@2020-11-01' = {
  name: virtualNetworkName
  location: location
  properties: {
    addressSpace: {
      addressPrefixes: [
        mainVirtualNetwork.addressSpace
      ]
    }
    enableDdosProtection: false
  }
}
```




