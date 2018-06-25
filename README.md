
[![Deploy to Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fpaulcradduck%2AZSP%2Fmaster%2Fazuredeploy.json)


This template deploys Splunk Enterprise 6.6 on Azure as either **standalone** instance or distributed **cluster** (up to 30 indexers). Each instance has eight (8) 1-TB data drives in RAID0 configuration. 
The template also provisions a storage account, load balancer, a virtual network with subnet,and all network interfaces, network security groups and apllication security groups required.


Below is the list of template parameters:

| Name   | Required | Description |
|:--- |:--- |:---|
| location | :heavy_check_mark: | Location where Azure resources will be created |
| storageAccountType | :heavy_check_mark: | Storage account type which determines data redundancy and underlying drive type. Defaults to `Standard_LRS` |
| deploymentType | :heavy_check_mark: | Splunk deployment type. Allowed values: `Standalone` (Default), `Cluster` |
| standaloneVmSize | :heavy_check_mark: | VM Size of standalone instance. Applicable for `Standalone` deployment type |
| clusterMasterVmSize | :heavy_check_mark: | VM Size of cluster master. Applicable for `Cluster` deployment type |
| DeployServerVmSize | :heavy_check_mark: | VM Size of deploy server. Applicable for `Cluster` deployment type |
| clusterSearchheadVmSize | :heavy_check_mark: | VM Size of cluster search head. Applicable for `Cluster` deployment type |
| clusterIndexerVmSize | :heavy_check_mark: | VM Size of cluster indexer. Applicable for `Cluster` deployment type |
| LicenseVmSize | :heavy_check_mark: | VM Size of cluster master. Applicable for `Cluster` deployment type |
| clusterIndexerVmCount | :heavy_check_mark: | Count of indexers. Integer between 3 and 20. Defaults to 30 |
| clusterSecret |:heavy_check_mark: | Secret shared among cluster nodes to authenticate communication between the master, the peers and search heads |
| adminUsername | :heavy_check_mark: | Admin username for the VMs |
| adminPassword | :heavy_check_mark: | Admin password for the VMs |
| splunkAdminPassword | :heavy_check_mark: | Password for Splunk admin user |
| virtualNetworkNewOrExisting | :heavy_check_mark: | Identifies whether to use new or existing Virtual Network. Allowed values: `new` (Default), `existing` |
| virtualNetworkExistingRGName | :heavy_check_mark: | Name of resource group of existing Virtual Network. Applicable if `virtualNetworkNewOrExisting=existing` |
| virtualNetworkAddressPrefix | :heavy_check_mark: | Virtual network address CIDR |
| virtualNetworkName | :heavy_check_mark: | Name of the virtual network to be used |
| subnet1Name | :heavy_check_mark: | Subnet for Splunk Nodes |
| subnet1Prefix | :heavy_check_mark: | Subnet CIDR |
| subnet1StartAddress | :heavy_check_mark: | Splunk Node subnet start address |
| subnet1EndAddress | :heavy_check_mark: | Splunk Node subnet end address |
| sshFrom | :heavy_check_mark: | CIDR block from which SSH access is allowed. Default is ssh access from anywhere |
| forwardedDataFrom |:heavy_check_mark: | CIDR block from which forwarded data is allowed. Default is data can be received from anywhere |
| domainNamePrefix | :heavy_check_mark: | Prefix for domain name to access Splunk |
| loadBalancerName | :heavy_check_mark: | Name of the load balancer |
| templateLocation | :heavy_check_mark: | Base URL of the Publisher Template gallery package |
| deployServerVmCount | :heavy_check_mark: | Count of deploye server nodes, Default Value 2 |
| searchheadVmCount | :heavy_check_mark: | Count of search head nodes, Default Value 6 |
| splunkRole | :heavy_check_mark: | Do Not Change from: ["CM","CI","SH"] |



### Cluster Mode:
Cluster search head & cluster master have the following ports open:
* 22 for SSH
* 443 and 8000 for HTTPS & HTTP to access Splunk
* 8089 for Splunkd Management open to VNet only

Cluster indexers have the following ports open:
* 22 for SSH
* 443 and 8000 for HTTPS & HTTP to access Splunk
* 9997 for TCP receiver traffic
* 8088 for HTTP Event Collector
* 9887 for TCP replication traffic open to VNet only
* 8089 for Splunkd Management open to VNet only

##Third-party software credits
- VM utility shell script: MIT license
- [Opscode Chef Splunk Cookbook](https://github.com/rarsan/chef-splunk): Apache 2.0 license
