## Compute > Image > Console Guide

## Create Image

Images can be created from the root block storage of an instance. In t2, m2, c2, r2, and x1 type instances except for u2 type instances, images can be created even when the instance is running, but data consistency is not guaranteed. In u2 type instances, images can be created only when the instance is stopped.

To create an image of a Linux instance, it is recommended to avoid duplication by initializing the machine-id. For more details on initialization of machine-id, see [Guide for Initialization of Linux machine-id](#Linux-machineid).

To create an image of a Windows instance, it is recommended to prepare for creating an image by using Sysprep and then stop the instance. For more details on how to use Sysprep, see [Guide for Windows Sysprep](#guide-for-windows-sysprep).

When creating an image from a running Windows instance, prerequisites are required for correct operation if the Windows instance has been made from an image before the distribution version 2019.05.28. You can check the Windows version of the image from which an instance has been created in **Image Name** of **Instance Details**. For more details, see [Guide to Creating Images from Running Windows Instances](#guide-to-creating-images-from-running-windows-instances).

## Modify Image

Modify an image name.

By selecting the **Protected** property and updating the image, you can prevent accidental deletion of the image. To delete an image with the Protected property selected, you must clear the **Protected** property in Modify Image and update the image.

## Copy to Another Region

Select the region you want to copy the image to, enter a name and description for the new image, and copy the image.

## Share with Other Projects

Select a project to share the image with, and share the image.

## Guide for Initialization of Linux machine-id

When creating a Linux user image to create an instance from the image, unexpected issues may occur due to duplicate machine-ids.
You can avoid duplication by initializing the machine-id before creating a user image.

	$ sudo sh -c "echo -n > /etc/machine-id"
	$ sudo rm /var/lib/dbus/machine-id
	$ sudo ln -s /etc/machine-id /var/lib/dbus/machine-id

## Guide for Windows Sysprep

To create a Windows image, you must reset the image by removing hardware-dependent and user-dependent information so that it can be used for instance creation. Image reset can be run from Sysprep, a system preparation utility provided by Microsoft for deploying Windows OS.

### If the original instance image version is 2018.08.18 or later
Connect to the Windows instance and run **Powershell** as administrator.
![[Figure 1 Running Powershell]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/win_sysprep1.png)

When the Powershell window appears, run the following command:

    ToastSysprep

![[Figure 2 Running ToastSysprep]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/win_sysprep2.png)
> [Note]
ToastSysprep is a command that makes it easy to use Sysprep provided by NHN Cloud.

Press **Y** to run a task.
![[Figure 3 Proceeding with ToastSysprep]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/win_sysprep3.png)

Once Sysprep is run, the Windows instance is automatically stopped. Confirm that the Windows instance is stopped in the NHN Cloud console, and create a custom Windows image with the [Create Image](./console-guide/#create-image) feature.

Once the Windows instance is reset by using Sysprep, the password is changed to blank, so you cannot log in. When using the image creation feature, it is recommended to select the **Reset Windows Password Created by Image** option and automatically reset the password of Windows instance. You can check the reset password from instance access information.

### If the original instance image version is earlier than 2018.08.18

First, connect to the Windows instance. In **Apps**, right-click **Command Prompt** and click **Run as administrator**.
![[Figure 1 Running Command Prompt]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/001_170524_800px.PNG)

When the Command Prompt window appears, run the following commands:

	sc config cloudbase-init start= demand
	cd C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf
	C:\Windows\System32\Sysprep\sysprep.exe /generalize /oobe /shutdown /unattend:Unattend.xml

![[Figure 2. Running Sysprep]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/002_170524_800px.PNG)

Once Sysprep is run, the Windows instance is automatically stopped. Confirm that the Windows instance is stopped in the NHN Cloud console, and create a custom Windows image with the [Create Image](./console-guide/#create-image) feature.

Once the Windows instance is reset by using Sysprep, the password is changed to blank, so you cannot log in. When using the image creation feature, it is recommended to select the **Reset Windows Password Created by Image** option and automatically reset the password of Windows instance. You can check the reset password from instance access information.

### Sysprep Option Details


`\generalize`

Remove unique system information registered in Windows. This step resets the Security ID (SID), clears all system restore points, and deletes the event log. After going through these steps, reboot and the ‘Windows Configuration Step’ will appear.
> [Note]
This step deletes SID, user information, and unique information, so the operation of existing programs may be affected.


`\oobe`
Reboot Windows to enter startup mode and apply the pre-defined settings, such as regional settings, network location, language settings, and time zone.

`\shutdown`
Shut down Windows.

`\unattend`
Reinstall Windows, and then restore the user settings recorded in the previous steps. This step registers Windows user information, updates drivers and products, and installs additional software. In addition to the default settings, the user can specify the desired settings in the response files.

> [Note]
Response files for Windows images provided by NHN Cloud are located in C:\Program Files\Cloud Solutions\Cloudbase-Init\conf\Unattend.xml. All required settings are prepared, so there is no need to modify them except for special purposes.


## Guide to Creating Images from Running Windows Instances

When creating an image with a running Windows instance, the prerequisites below are required if the image version of the original instance is earlier than 2019.05.28.
If the image is a Windows image of 2019.05.28 distribution version or later, the process below is unnecessary.

1. Connect to the Windows instance and run **Task Scheduler** from the **Apps**.
![[Figure 3 Running Task Scheduler]](http://static.toastoven.net/prod_infrastructure/compute/windows/001_190604.png)

2. Click **Create Task** from the **Actions** tab on the right of **Task Scheduler**.
![[Figure 4 Starting Create Task]](http://static.toastoven.net/prod_infrastructure/compute/windows/002_190604.png)

3. In the **General** tab of the **Create Task** window, enter a name in **Name**. Select **Run with highest privileges** in **Security options** and then click **Change User or Group**.
![[Figure 5 Changing User and Setting Priority]](http://static.toastoven.net/prod_infrastructure/compute/windows/003_190604.png)

4. In the input box below **Enter the object name to select**, enter **SYSTEM** and click **OK**.
![[Figure 6 Setting SYSTEM User]](http://static.toastoven.net/prod_infrastructure/compute/windows/004_190604.png)

5. In the **Triggers** tab of the **Create Task** window, click **New** to create a new trigger.
![[Figure 7 Setting Trigger 1]](http://static.toastoven.net/prod_infrastructure/compute/windows/005_190604.png)

6. Select the trigger for **Begin the task** as **At startup** and click the **OK** button to complete the trigger creation.
![[Figure 8 Setting Trigger 2]](http://static.toastoven.net/prod_infrastructure/compute/windows/006_190604.png)

7. In the **Actions** tab of the **Create Task** window, click the **New** button to create a new action.
![[Figure 9 Setting Action 1]](http://static.toastoven.net/prod_infrastructure/compute/windows/007_190604.png)

8. In the **New Action** window, enter the values as follows.

```
	Program/script: C:\Windows\System32\ipconfig.exe
	Add arguments (optional): /renew
```

![[Figure 10 Setting Action 2]](http://static.toastoven.net/prod_infrastructure/compute/windows/008_190604.png)

9. Click the **OK** button to close the **New Action** window and the **Create Task** window to complete the scheduler registration.
