## Compute > Image > Console Guide

## Create Images

An image is created at a default disk of an instance. Select a closed instance to create an image. Without it, images cannot be created. 

To create an image of a Widows instance, prepare an image creation by using Sysprep and close the next instance. For more details of sysprep, refer to [Guide to Windows Sysprep](#sysprep).  

Close a Linux instance within an instance or in the TOAST console and create an image. 

## Modify Images

Modify an image name.

Check **Protected** and update images, in order to prevent images from deleted by mistake. To delete an image where Protected is enabled, uncheck **Protected** in Modify Image and update an image.

## Share with Other Projects

Select a project to share images with and share.


## Guide to Windows Sysprep

To create a Windows image, an image must return to default, by deleting information included to the hardware and user, and be made available for creating an instance. The process can be executed with Sysprep, which is a system preparation utility provided by Microsoft to distribute Windows OS.

First, access a Windows instance and execute an Order Prompt at the manager's authority.
![[Figure 1 Execute Order Prompt 명령 프롬프트 실행]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/001_170524_800px.PNG)

When the order prompt window pops, execute the following order: 

	cd C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf
	C:\Windows\System32\Sysprep\sysprep.exe /generalize /oobe /shutdown /unattend:Unattend.xml

![[Figure 2. Execute Sysprep]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/002_170524_800px.PNG)

When Sysprep is completed, Windows instance is automatically closed. When Windows instance is confirmed closed, user's Windows image can be created by using [Create Images](./console-guide/#_1). 

### Option Details of Sysprep


`\generalize`

Remove the original system information registered in Windows. In this phase, Security ID (SID) is reset, while all system restoration points, as well as event logs are removed. After that, it is rebooted and the `Windows Configuration Phase` will be tried.   
> [Note]
In this phase, deleting SID and user information may affect operations of existing programs.  


`\oobe`
Reboot Windows and enter with a start mode, then apply pre-defined user settings, such as setting by nation, network location, language, and time zone. 

`\shutdown`
Shut down Windows. 

`\unattend`
Reinstall Windows, and restore the user settings as recorded in the previous phase. Here, Windows user information is registered, and drivers and products are updated, while additional software can be installed. Further user configuration is also available, adding to default settings provided by response files.     

> [Note]
Response files for Windows images that TOAST provides are located at C:\Program Files\Cloud Solutions\Cloudbase-Init\conf\Unattend.xml. As settings are all ready as required, no modification is necessary, except for special purposes.  
