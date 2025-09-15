Intune is an platform for managing endpoint devices, which provides cloud-based *mobile device management (MDM)*, *mobile application management (MAM)* and computer management. 
Intune consists of Intune, Microsoft Endpoint Configuration Manager and Microsoft Autopilot.
It can also be integrated with:
- *Microsoft [[Entra ID]]* for identity and access control
- *MS Purview* Information Protection
- *MS Defender* advanced threat protection

# Endpoint Configuration Manager
With the Endpoint Configuration Manager you can manage desktops computers, laptops and servers that are on your network or the internet, this includes:
- deploying apps, software updates and OS
- configure sites and clients
- run and monitor management tasks
It supports Windows and macOS, additionally you can attach the Configuration Manager to the M 365 cloud to provide integration with Intune, [[Entra ID]], Defender and other cloud services. 
## Cloud attach
Cloud attach allows you to use both Microsoft Intune and Microsoft Endpoint Configuration Manager from within Intune.
### Tenant attach
With Tenant attach you register the Configuration Manager deployment with an Intune Tenant. To do this you use the Configuration Manager connector, which requires a connection to an Intune tenant, to enable data flow. This allows the Intune cloud service to recognize and act on Configuration Manager devices and infrastructure from within Intune 
### Co-management
Co-management enables you to manage Windows 10/11 devices with both Configuration Manager and Intune and combines your existing on-premise Configuration Manager and Active Directory investment with the cloud by using Intune, Microsoft [[Entra ID]] and other M 365 services.
It is also possible to choose which tasks are running on-premise and which run in the cloud, further you can set the Configuration Manager or Intune the management authority.
There are two ways to co-manage:
- For *existing* clients:
  set up a hybrid Entra ID and enroll the devices into Intune
- For *new* clients:
  join Entra ID and automatically enroll to Intune, install the Configuration Manager clients to enable co-management.

Co-management gives you the following capabilities:
- Conditional access with device compliance
- Intune-based remote actions, such as restart, remote control or factory reset 
- Centralized device health visibillity
- Entra ID to link users, devices and apps
- Modern provisioning with Windows Autopilot
# Endpoint analytics
Endpoint analytics can:
- Help identify configuration or hardware issues and provide recommendations for fixing
- Make changes without disrupting end users or generate help desk tickets

# Windows Autopilot
Autopilot lets you configure enrollment details, which are then automatically staged on a new computer after it powers up for the first time. The user only has to connect the computer to a network and enter their credentials, everything else is automated.
The following tasks can be done by Autopilot using cloud-based services:
- Automatically join devices to [[Entra ID]]
- Autoenroll devices into mobile device management (MDM) such as Intune
- Restrict rights on the device
- Autoassign devices to configuration groups
- Present customized out-of-the-box-experience (OOBE) content 
- Reset, repurpose and recover devices

# Deploying Applications
## Add Applications to Intune
The following app types can be added in Intune:
- **Store Apps**:
  Apps that are uploaded to the Microsoft, iOS or Android store and are maintained by the developer.
  You select the app from a list in Intune.
- **Microsoft 365 Apps**:
  Apps included in the M 365 suite. You can also install apps for the MS Project Online desktop and MS Visio Pro.
  The apps are displayed as a single entry in the list of app types.
- **Web Link**:
  Client-Server applications, where the server provides the web app, including the UI, content and functionality. The app is separately maintained in the web. 
- **Build-in app**:
  Here you can assign curated managed apps, such as MS 365, like Excel, OneDrive, Outlook and others. After adding an app the app type is displayed as an Built-in iOS or Built-in Android app.
- **Line-of-business (LOB) app**: 
  LOB apps are apps that are added from an app installation file. 
- **Windows app (Win32)**:
  Can be used to add, install and uninstall apps in various formats such as MSIs, Setup.exe or MSP.
## Manage Win32 apps
