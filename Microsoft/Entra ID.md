Entra ID, formerly Azure Active Directory is a cloud-based identity and access management service. It is used to manage authorization and access to resources including:
- Internal resources: 
  apps on a corporate network / intranet and cloud apps developed in-house 
- External resources:
  Microsoft Office 365, [[Azure]] portal and any SaaS application used

Entra ID provides a single identity system for cloud and on-premise applications and can be synchronized with existing on-premises Active Directory or other directory services.

# Basic Terminology
- **Tenant**: 
  an instance in which information about a single organization is stored. It also includes an unique tenant ID, a domain name (xyz.onmicrosoft.com) and servers as a security and administrative boundary. 
  Information stored in an instance include:
	- users and groups 
	- devices
	- application registration 
	- compliance policies for resources 
- **Directory**:
  a logical container where the information of a tenant is stored 
  a Entra tenant consists of only on directory
- **Multi-tenant**:
  an organization can have multiple instances of Entra ID, the reason for this can be a organization with multiple subsidiaries or business units that operate independently, organizations that merge or acquire companies and more 

# Identity Types
![[Identity Types.png]]
## User
A user identity can represent an employee or an external user (customer, consultant, vendor and partners) and is characterized by authentication and user type property.
They authenticate either internally or externally:
- *Internal authentication*:
  the user has an account on the organizations Entra ID and uses that account to authenticate
- *External authentication*:
  the user has an account belonging to another organization, a social network identity or other external identity provider

The user type property describes the relationship between the organization and the user:
![[User type property.png]]
## Workload Identities
A workload identity is assigned to a software workload, enabling the workload to authenticate to and access other services and resources.

## Application and service principals
A service principal is an identity for an application, which can be registered and integrated into Entra ID, which enables the application to delegate its identity and access functions to Entra ID. Once the application is registered, a service principal is created in each Entra ID tenant where the application is used. The service principal allows the application to authenticate and authorize to other resources. 
>[!note]
>The application developers must manage and protect the credentials, with which the service principals accesses resources. If done incorrectly it can lead to security vulnerabilities

### Managed Identities
Managed identities are service principals that are automatically managed by Entra ID and provide an identity for applications to use when connecting to Azure resources that support Entra authentication.
There are two types of managed identities:
- *System-assigned*:
  Azure resources such as VMs allow you to enable a managed identity directly on the resource, meaning that an identity is created in Entra and is tied to the liecycle of that Azure resource. When the Azure resource is deleted the identity is also automatically deleted.
- *User-assigned*:*
  User-assigned managed identities are also assigned to one or more instances of Azure services, but are managed separately form the resources that use it. The identity has to be manually deleted. 
## Device
Device identities give administrators information they can use when making access or configuration decisions.
Device identities can be set up in different ways:
- **Entra registered devices**:
  Used for bring your own device (BYOD); personal devices are registered to Entra ID without requiring an organizational account and can then access organizational resources. 
- **Entra joined devices**:
  These devices are usually owned by the organization and join Entra ID through an organizational account.
- **Entra hybrid joined devices**:
  Devices are joined to an existing on-premises Active Directory and Entra ID requiring an organizational account

Registering an joining devices to Entra ID gives users *Single Sign-on (SSO)* to cloud based resources and Entra joined devices have SSO to resources and applications that rely on on-premises Active Directories.

>[!note]
>Admins can use [[Intune]] for *mobile device management (MDM)* and *mobile application management (MAM)*

## Groups
If multiple identities need access to the same resources, you can create a group to give access permissions to all members of it.
There are two groups:
- **Security**:
  used to manage user and device access to shared resources and set security policies such as password reset or requiring multi factor authentication (MFA).
  Only admins can create security groups.
- **Microsoft 365**:
  used for grouping users according to collaboration needs , f.e. shared mailbox, calendar, SharePoint and more.
  Users and admins can create M 365 groups.

## Hybrid Identity
Hybrid Identities are used to create a single identity for authentication and authorization spanning on-premises and cloud-based resources.
Hybrid identity is accomplished through provisioning and synchronization:
- *Inter-directory provisioning*
  provisioning an identity between two different directory services system, f.e. an existing Active Directory identity is provisioned into Entra ID
- *Synchronization*:
  responsible for ensuring that identity information of on-premises users and groups is matching the cloud

One synchronization method is Entra Cloud Sync, which uses the Entra cloud provisioning agent and the System for Cross-domain Identity Management (SCIM) specification with Entra ID to provision and deprovision users and groups. The organization only needs to deploy the agent in their on-premises or IaaS-hosted environment.
## External Identities
To share a company's applications and services with business partners and guests use External ID for B2B. The business guest authenticates with their home organization or identity provider and the organization checks the user's eligibility for guest collaboration.