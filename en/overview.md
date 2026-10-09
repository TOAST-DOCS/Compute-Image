<!-- machine_translated: true -->

<!-- pre-align:aligned sig=ab849ab21c5b -->

{% set svc_domain = {"public":"toast.com","gov":"gov.toast.com","ncgn":"gncloud.go.kr","ninc":"ninc.go.kr","ngsc":"ngsc.go.kr","ngovc":"ngovc.com","ngoic":"ngoic.com"} %}

<a id="compute-image-overview"></a>
## Compute > Image > Overview { #compute-image-overview }

An image contains the operating system and applications and is used to create an instance’s root block storage. NHN Cloud provides images with various operating systems and applications installed by default, and the images can be modified and used according to the user's requirements.

An instance's root block storage is created from an image and can be resized according to user settings. However, the default disk size can only be adjusted beyond the size of the image.

The characteristics of NHN Cloud images are as follows.

- The images are configured to run optimally on virtual hardware.
- The images are provided with basic security checks completed, so they are safe from security threats.

Images are classified into the following {% if "public" in build_flags %}3{% else %}2{% endif %} main categories.

* Public images
* User images
{% if "public" in build_flags %}
* Shared images
{% endif %}

**NHN Cloud does not support uploading private images.**

<a id="public-images"></a>
### Public Images { #public-images }

Public images are images provided by NHN Cloud. These images have an operating system installed for optimal use of virtual hardware. In addition, basic security settings are in place so that you can use it with confidence in your services and applications.

NHN Cloud currently offers {% if "public" not in build_flags and "gov" not in build_flags %}Rocky Linux, {% endif %}Debian, Ubuntu, and Windows. For more details on operating system versions offered, refer to [NHN Cloud Service Introduction](https://$[ svc_domain[(build_flags | select("in", ["public","gov","ncgn","ninc","ngsc","ngovc","ngoic"]) | list | first)] ]$/service/compute/instance).

Some images have applications installed, so you can build services faster. In the future, we plan to provide images in which more applications are installed according to various user needs.

<a id="user-images"></a>
### User Images { #user-images }

User images are images modified by the user based on public images. You can install new applications or change various operating system settings according to the service or application you are using.

User images are useful for service scale-out. It takes a lot of time to create new instances with a public image and install the service for service scale-out. You can respond to a surge in service load more quickly by creating a user image in advance and using it for instance creation, rather than repeating the cumbersome installation work every time.

User images can be easily created by using **Additional Functions** of the Image service or the Compute service. For details on how to create an image, see the [Image Console User Guide](./console-guide/) or the [Instance Console User Guide](/Compute/Instance/en/console-guide/).

{% if "public" in build_flags %}

<a id="shared-images"></a>
### Shared Images { #shared-images }

Shared images are images that are being shared with other projects. You can set images from your projects to be shared with other projects you belong to.

But, images shared from other projects can't be re-shared.

{% endif %}

<a id="pricing"></a>
### Pricing { #pricing }

Users are charged for an image by the size of the created block storage from the moment the image is created.