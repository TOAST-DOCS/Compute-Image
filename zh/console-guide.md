## 计算 > 镜像 > 控制台使用指南

## 创建镜像

镜像是在实例系统盘中创建。为确保数据一致性，必须选择处于关闭状态的实例创建镜像。如果没有关闭状态下的实例，则无法创建镜像。

如果要创建Windows实例镜像，必须使用Sysprep为创建镜像做好准备，然后关闭实例。更多sysprep使用方法请参考[Windows Sysprep指南](#sysprep)。

如果要创建Linux实例镜像，则必须关闭实例或从TOAST控制台退出后创建。 

## 编辑镜像

更改镜像名称。

选择**Protected**属性并更新镜像可防止镜像被意外删除。如果要删除Protected 属性开启状态下的镜像，需要在镜像编辑页面中取消已选中的**Protected**属性，然后更新镜像。

## 共享至其它项目

选择要共享的项目，共享镜像。


## Windows Sysprep指南

如果要创建Windows镜像，需要先删除硬件信息和用户所属信息对镜像进行初始化，才可将其用于实例创建。上述过程可借助Sysprep完成，Sysprep是由Microsoft提供，用于部署Windows操作系统的系统准备工具。

首先，连接到您的Windows实例，进入**App**，右键单击**命令提示符**，选择**以管理员身份运行**。
![[图1 命令提示符 
运行]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/001_170524_800px.PNG)

在弹出的命令提示符窗口运行以下命令：

	cd C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf
	C:\Windows\System32\Sysprep\sysprep.exe /generalize /oobe /shutdown /unattend:Unattend.xml

![[图2 Sysprep 运行]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/002_170524_800px.PNG)

当Sysprep运行时，Windows实例会自动关闭。在TOAST控制台确认Windows实例确实关闭后，便可以使用[创建镜像](./console-guide/#_1)功能创建自定义Windows镜像。

### Sysprep选项详情


`\generalize`

删除在Windows中注册的固有系统信息。在此过程，会重置SID(Security ID)，并清除所有系统还原点和删除事件日志。当完成上述步骤后重启程序，将会显示`Windows配置步骤`。
> [参考]
在此阶段，您可以通过删除SID及用户信息和唯一信息，影响现有程序的执行。


`\oobe`
重启Windows进入启动模式后可以应用用户事先指定的设置。这些设置包括国家/地区设置、网络位置、语言和时区等。

`\shutdown`
关闭Windows。

`\unattend`
重新安装Windows之后，复原上一步骤中的用户设置。在此过程中，注册Windows用户信息，安装驱动程序、更新产品以及安装其它软件。除默认设置以外，用户可以在应答文件中进行自定义设置。

> [参考]
TOAST提供的Windows镜像应答文件位于C:\Program Files\Cloud Solutions\Cloudbase-Init\conf\Unattend.xml中。所有必要的设置都已准备就绪，除特殊用途外，无需用户进行修改。
