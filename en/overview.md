## Compute > Image > Overview

An image, as a virtual disk, contains an operating system and application, and is used as a default disk of an instance. A default disk of an instance is created upon images and the size can be adjusted as the user configures. However, the default disk size cannot be smaller than the size of an image.

NHN Cloud provides images installed upon various operating systems and applications. Such images are: 

- Configured for optimal execution under virtual hardware; 
- Safe from security threats as they are provided after basic security check is done. 

User can modify default images to fit his needs. 

Images are classified into three types in general: 

* Public Images
* User Images
* Shared Images

### Public Images

Provided by NHN Cloud, public images are installed with operating systems to make the best use of virtual hardware. Security setting is also provided by default for safe use in customer services and applications. 

NHN Cloud currently supports CentOS, Debian, Ubuntu, and Windows. For more information about the versions we support, refer to [NHN Cloud Service](https://toast.com/service/compute/instance).

Some images even have application programs to enable faster service implementation. Currently available images are based on CentOS, with MySQL database installed.   

In the near future, more application programs will be available as user demands. 

### User Images

User images are modified images by user based upon public images. New application may be installed to suit for services or applications that are in use, or OS setting can be modified. 

User images are useful for service scale-ups. To scale out services with public images, it takes a lot of time to create new instances and install services. However, with user images, once created in advance, speedier response to load hikes is available, without having to repeat the burden of installations.   

User images can be easily created by using **Additional Functions** of Image Service or Compute Service.  For more details, refer to [Guide to Use Image Console](/Compute/Image/en/console-guide/) or [User Guide to Instance Console](/Compute/Instance/en/console-guide/).

### Shared Images

User images can be shared with other projects by setting. Nevertheless, shared images are available between projects of a same owner only. 

### Pricing

An image is charged by the size of a created disk from the moment it is created.  
