## 计算 > 镜像 > 控制台使用指南

## 创建镜像

仅可在实例的默认磁盘中创建镜像。除u2类型的实例外，t2、m2、c2、r2、x1类型的实例支持在运行中创建镜像，但无法保障数据整合性。u2类型的实例，仅终止状态的实例可创建镜像。

若欲创建Windows实例的镜像，建议使用Sysprep完成创建镜像准备后终止实例。详细的sysprep使用方法参考[Windows Sysprep指南](#windows-sysprep)。

将运行中的Windows实例创建为镜像时，若是创建为低于2019.05.28发行版本的镜像的Windows实例，为保证运行正确，需要先行操作。创建实例的镜像的Windows版本可在**实例详细信息**的**镜像名**中确认。详细内容参考[运行中的Windows实例镜像创建指南](#windows)。

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

当Sysprep运行时，Windows实例会自动终止。在NHN Cloud控制台确认Windows实例确实终止后，便可以使用[创建镜像](./console-guide/#_1)功能创建自定义Windows镜像。

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
NHN Cloud提供的Windows镜像应答文件位于C:\Program Files\Cloud Solutions\Cloudbase-Init\conf\Unattend.xml中。所有必要的设置都已准备就绪，除特殊用途外，无需用户进行修改。

## 运行中的Windows实例镜像创建指南

利用运行中的Windows实例创建镜像时，若原实例的镜像版本低于2019.05.28，则需要进行如下先行操作。
若为高于2019.05.28发行版本的Windows镜像，则不需要如下过程。

1.连接Windows实例后，在**应用程序**中运行**Task Scheduler**。
![[图3 运行Task Scheudler]](http://static.toastoven.net/prod_infrastructure/compute/windows/001_190604.png)

2.单击**Task Scheduler**右侧**Actions**标签的**Create Task**。
![[图4 开始Create Task]](http://static.toastoven.net/prod_infrastructure/compute/windows/002_190604.png)

3.在**Create Task**窗口的**General**标签中，在**Name**中输入名称，选择**Security options**的**Run with highest privileges**后单击**Change User or Group**。
![[图5 更改User并设置优先顺序]](http://static.toastoven.net/prod_infrastructure/compute/windows/003_190604.png)

4.在**Enter the object name to select**下端的输入框中输入**SYSTEM**并单击**OK**按钮。
![[图6 设置SYSTEM用户]](http://static.toastoven.net/prod_infrastructure/compute/windows/004_190604.png)

5.在**Create Task**窗口的**Triggers**标签中单击**New**按钮，创建新的触发器(trigger)。
![[图7 设置Trigger 1]](http://static.toastoven.net/prod_infrastructure/compute/windows/005_190604.png)

6.将**Begin the task**的触发器选择为**At startup**并单击**OK**按钮，完成创建触发器。
![[图8 设置Trigger 2]](http://static.toastoven.net/prod_infrastructure/compute/windows/006_190604.png)

7.在**Create Task**窗口的**Actions**标签中单击**New**按钮，创建新的动作(action)。
![[图9 设置Action 1]](http://static.toastoven.net/prod_infrastructure/compute/windows/007_190604.png)

8.在**创建Action**窗口中输入如下值。

```
	Program/script:C:\Windows\System32\ipconfig.exe
	Add arguments(optional): /renew
```

![[图10 设置Action 2]](http://static.toastoven.net/prod_infrastructure/compute/windows/008_190604.png)

9.单击**OK**按钮，关闭**New Action**窗口与**Create Task**窗口，完成日程登记。
