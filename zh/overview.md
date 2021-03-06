## 计算 > 镜像 > 概述

镜像是搭载操作系统和应用程序的虚拟磁盘，作为实例的系统盘使用。NHN Cloud提供了多种操作系统和应用程序的内置镜像，用户可通过修改镜像以满足其需求。

实例的系统盘基于镜像创建，可根据用户设置进行伸缩。但，系统盘大小必须大于镜像大小。

NHN Cloud镜像特征如下：

- 适合在虚拟硬件上运行。
- 已通过基本安全检查，安全性较高。

镜像大体分为3类：

* 公共镜像
* 自定义镜像
* 共享镜像

### 公共镜像

NHN Cloud提供公共镜像。这些镜像安装了操作系统，可以实现虚拟硬件应用最优。并且，已做好基本的安全设置，用户可以放心使用。

目前NHN Cloud支持以下操作系统：

| 操作系统 | 版本 |
|------- | ---- |
| CentOS | 6.5<br>7.1|
| Debian | 8.2.0 |
| Ubuntu | 14.04<br>16.04 |
| window | 2008R2 standard<br>2012R2 standard |

部分镜像安装了应用程序，可帮助用户快速有效地构建服务。目前，CentOS操作系统中提供的镜像已安装MySQL数据库。

未来，我们将根据用户需求，提供多种搭载应用程序的镜像。

### 自定义镜像

自定义镜像是指用户基于公共镜像进行修改之后的镜像。用户可以根据服务需要安装新的应用程序，或更改各种操作系统的设置。

自定义镜像多用于服务扩展。当您需要扩展服务时，若使用公共镜像创建实例，再去安装服务可能需要消耗较长时间。但，如果您事先将服务所需的镜像作成自定义镜像，可以免去每次重复的繁琐设置，快速创建实例。同时，对于服务负荷激增情况也可以较快作出反应。

自定义镜像可以通过Image服务和Compute服务中的**添加功能**，轻松创建。详细方法请参考[镜像控制台使用指南](/Compute/Image/zh/console-guide/)或[实例控制台使用指南](/Compute/Instance/zh/console-guide/)。

### 共享镜像

您可以将您的镜像设置为与其它项目共享。但，共享镜像只能用于同一所有者的项目之间。

### 计费

镜像创建后即按照创建磁盘的大小计费。
