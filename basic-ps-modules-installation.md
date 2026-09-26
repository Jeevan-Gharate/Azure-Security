- to connect or interact to any of the azure services or manage azure tenant , you need to have PowerShell tools installed
- even though one can manage the tenant directly via the azure portal its recommended that you have these tools installed because 1. Pentesting/enumeration tools we will be using will require these cli modules installed and 2. knowing CLI wont hurt

### run the below commands in powershell

```
Install-Module Az
Install-Module AzureAd
Install-Module MSOnline
```

once installed you can authenticate these module with your azure tenant
```
Connect-AzAccount
Connect-AzureAD
Connect-MsolService
az login
```