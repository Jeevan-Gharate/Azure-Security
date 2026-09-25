
### AAD (Azure Active Directory)

- IAM Service
- Allows users to and apps to authenticate to and access resources (VM, M365 Suite, etc)
- everything inside a "Tenant" (think of Tenant as a "Company" or a "Domain")

### Azure Resource(service) Hierarchy

- Root Management Groups > Child Management Groups > Subscriptions > Resource Groups > Resources
- 
   ![Alt Text](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-setup-guide/media/organize-resources/scope-levels.png)


### Azure Services (Resources
- more than 200 services
- segregated under several categories
  
  ### common services
	- Applications
	- Service Principal - "identity" tied to an application used to access other resources
	- key vaults - vault to store secrets, api keys, passwords, etc
	- automation accounts - accounts that runs an automation scripts in a scripting language to perform a certain task
	- storage accounts - access cloud storage service
	- SQL DB - cloud hosted DB 
	- Managed Identity - "Identity" to log in to / facilitate Authentication or access a resource or multiple resources without the need of a username/password

### Service Principal

- Represents the identity of an application 
- automatically created when crating an application and then tied to that application
- denoted by an 128bit UID
- applications are granted further access to resources and service principal tied to these applications is how it is possible to give permissions to these applications to resources
- single factor authentication
- applications use this Service principal to authenticate to resource/resources they have permission to (uses secret or certificate)
- best vector for privilege escalation

### Managed Identities
- similar to service principal
- Mi represents more than just applications - at current point in time there are 54 services that can be assigned an managed identities
- ex. VM, kubernetes, etc
- used to access other resources without knowing the creds

### Roles in Azure

- Azure AD roles and Azure RBAC (resource-based access) roles

  ### Azure AD Roles (Microsoft Entra built-in Roles)
	 - Administrative settings / manage access to Azure AD high level stuff (ex. editing users)
	 -  some roles -  Global Admin, User Admin, hybrid admin
   
   ### Azure RBAC Roles
    - manage access to azure resources that comes under subscription plane of resource hierarchy
    - ex. VM access, Storage accounts access
    - some roles are - Owner, Contributor, Reader, etc


### Azure Users
- UPN (ex. john.doe@tenant.onmicrosoft.com)
- ObjectID (128bit UID)
### Azure Groups
- manage accounts in bulk (admins, devs, hrs, etc)
- can be assigned <u>manually</u> added by an administrator / group owner
- <u>automatically</u> (when certain conditions are met)

### Hybrid Azure Environments
- configures federation with on premises AD federation service (ADFS) and Azure AD
- allows users to sign in to azure ad with their on-premises AD creds
- an agent is used to faciliate this hybrid federation
- ![Alt Text](https://raw.githubusercontent.com/Jeevan-Gharate/Azure-Security/refs/heads/main/images/hybridauth.png)

### Hybrid Identity Auth Methods
- .
   ### Azure AD password hash sync
    - users uses same credentials as their on-prem creds and can authenticate to AAD
    - no contact with the on-prem AD
    - because the user passwords hashes are synced with the on-prem AD
    
    ### Azure AD Pass-Through Auth
     - An Executable "agent" executes on on-prem AD server whhich validates the user creds then let AAD know that the user is valid and authentication is done
     - password validation does not happen on cloud
     - this helps enforce on-prem security / password policies
     
    ### Federation
     - entire authentication occurs on-premises via ADFS
	
   
### Useful Log Sources (for blue team or investigation)
- unified audit logs - collection of all logs pertaining to m365 (https://security.microsoft.com/auditlogsearch)
- azure audit logs - tracks changes across azure ad at a subscription level
- azure sign in logs - user sign-in events in azure ad
- azure activity logs - track activities and actions takes at a subscription level
- message tracing logs - tracks the flow of email in an organisation (https://admin.exchange.microsoft.com)
- ![logs sources 2](https://raw.githubusercontent.com/Jeevan-Gharate/Azure-Security/refs/heads/main/images/logsources2.png)

### Accessing Azure

- ps> `Import-Module AADInternals`
  ### web application
	- portal.ms
	- admin.exchange.s
	- compliance.ms
	- security.ms
  ### graph explorer API or graph ps SDK
	- ps> `Connect-MgGraph`
  ### MsOnline (PS)
	- ps> `Connect-MsolService`
  ### Azure Cli (PS) 
	- ps> `az login`

### Primary Refresh Token
