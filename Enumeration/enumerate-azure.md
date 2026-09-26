Tools -
- `Azure AD Internals` [AADInternals](https://github.com/gerenios/aadinternals) [AADInt-Docs](https://aadinternals.com/aadinternals/)
- [MicroBurst](https://github.com/Netspi/Microburst) [wiki](https://github.com/NetSPI/MicroBurst/wiki)
- [Awesome Azure Pentest](https://github.com/Kyuu-Ji/Awesome-Azure-Pentest)
- [Cloud-Azure PayloadAllTheThings](https://swisskyrepo.github.io/PayloadsAllTheThings/Methodology%20and%20Resources/Cloud%20-%20Azure%20Pentest/)
- [BlobHunter](https://github.com/cyberark/BlobHunter)
- [Cloud Enum](https://github.com/initstring/cloud_enum)
- [Azure IP Ranges & Service Tags Tracker](https://eliaquimbrandao.github.io/azure-service-tags-tracker/)


### check if company uses Azure 
 - https://login.microsoftonline.com/getuserrealm.srf?login=username@COMPANYNAMEHERE.onmicrosoft.com&xml=1
 - replace COMPANYNAMEHERE with the company name (ex. accenture)
 - using nslookup > `nslookup web.anywebsite.com` if result shows `windows.net` anywhere in results then we can conclude the domain is hosted in azure
 - or use tool like - [cloudipchecker](https://github.com/deanobalino/cloudipchecker) - it works by Checking if an IP address is part of an Azure Service Tag
 - or another online similar tool [here](Check if an IP address is part of an Azure Service Tag) 

### Enumerate using DNS Suffixes

- Many Azure services dynamically generate custom endpoints utilizing a trusted cloud suffix (such as `.cloudapp.azure.com` or `.windows.net`). Maintaining a structured inventory of these suffixes is critical for security auditing, zero-trust network design, and defensive threat research.

- These services can also be leveraged for domain fronting, subdomain takeover, or communication with an external C2 server when they are whitelisted by proxy or firewall rules.
 eg. 
	
| Services                | DNS Suffixes                            |
| ----------------------- | --------------------------------------- |
| Azure Blob Storage      | \*.blob.core.windows.net                |
| Azure Cloud / Azure VMs | \*.cloudapp.net / \*.cloudapp.azure.net |
| Azure Files             | \*.file.core.windows.net                |
| Azure SQL DBs           | \*.database.windows.net,                |
and there are several more such DNS suffixes you can read [here](https://swisskyrepo.github.io/InternalAllTheThings/cloud/azure/azure-services-web-domains/)

- we can pretty much automate it using the NetSPI tool called ["MicroBurst"](https://github.com/Netspi/Microburst) which has collection of different scripts for assessing Azure Security
- one of the script `invoke-EnumerateAzureSubDomain.ps1` can automate the discovery with these dns suffixes to list out azure services used by that tenant/domain
- ps > `Import-Module .\MiocroBurst.psm1`
- ps > `Invoke-EnumerateAzureSubDomains`

### Domain Info
- Tenant ID - `Get-AADIntTenantID -Domain <domain>`
- all domains of the tenant - `Get-AADIntTenantDomains -Domain <domain>`
- login info of tenant , including tenant name and domain auth type -
   `Get-AADIntLoginInformation -UserName <usertname>`
- more can be found [here]()
- eg, i want to recon the Tenant information i copied all the function names from the index at https://aadinternals.com/aadinternals/#tenant-information-and-manipulation-functions and then pasted it into an ai and asked "which of this scripts from AADInternals suite will be able to get tenant information from an extern POV (no internal creds or login or account)"
- ![[Pasted image 20260926141328.png]]
- there is even a symbol infront of each function in the given documentation 
- (\*) - outsider (no creds)
- \(A) - Authenticated (User / Admin Context)
- \(M) - MSOL / Special Context
- \(Z) - On-Prem / Hybrid Admin

 - enumerating SubDomains: `Invoke-AADIntReconAsOutsider -DomainName companyname.onmicrosoft.com | Format-Table`
 - 
### User Enum
- figure out a typical target email address format
- we can use https://huntr.io to generate a list (not necessary) there are otherways pentesters have
- can consider using LinkedIn / PasteBin searches
- or using tools like: [o365enum](https://github.com/gremwell/o365enum) - it shows 2 different ways of enumerating valid users, give the repository README a good read to understand the two ways namely: ActiveSync and AutoDiscoveryv1
- using users list: `Get-Content users.txt | Invoke-AADIntUserEnumerationAsOutsider -Method Normal`

### Resource Groups enum
- if logged in then: 
  - `Get-AADIntAzureResourceGroups -AccessToken $at -SubscriptionId "subscription-uuid"`
  - 