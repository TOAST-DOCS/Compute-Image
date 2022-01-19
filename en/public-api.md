## Compute > Image > API v2 Guide

To enable APIs, API endpoint and token are required. Prepare information required to enable an API, in reference of  [Preparing for APIs](/Compute/Compute/ko/identity-api/)

Use `image`-type endpoint for Image API. For more details, see `serviceCatalog` from response of token issuance. 

| Type | Region | Endpoint |
|---|---|---|
| image | Korea (Pangyo)<br>Japan | https://kr1-api-image.infrastructure.cloud.toast.com<br>https://jp1-api-image.infrastructure.cloud.toast.com |

In API response, you may find fields that are not specified in the guide. Refrain from using them because such fields are only for the NHN Cloud internal usage and might be changed without previous notice. 

## Image
### List Images 

```
GET /v2/images
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| limit | Query | Integer | - | Image count to return (default is 20) |
| marker | Query | UUID | - | ID of the first image on the list to query <br>Query as much as the `limit` after image specified as the `marker` according to the sorting order |
| name | Query | String | - | Name of image to query |
| visibility | Query | Enum | - | Visibility attribute of the image to query<br>Select only one of`public`, `private`, and `shared` <br>If left blank, list of all types of images are returned. |
| owner | Query | String  | - | ID of tenant including image to query |
| status | Query | Enum    | - | Image status to query <br>`queued`: Converting image<br>`saving`: Uploading image<br>`active`: Normal<br>`killed`: Deleting image from system<br>`deleted`: Image deleted<br>`pending_delete`: Delete Image is pending |
| size_min | Query | Integer | - | Minimum size of image to query (bytes) |
| size_max | Query | Integer | - | Maximum size of image to query (bytes) |
| nhncloud_product | Query | Enum | - | Infrastructure service type of image to query<br>`compute`: Instance service image<br>`gpu`: GPU Instance service image |
| sort_key | Query | String | - | Attribute to sort image list <br>Available to specify all attributes of image; default is `created_at` |
| sort_dir | Query | Enum | - | Sorting direction of image on the list<br>Select only one of`asc`(ascending), or `desc`(descending) order |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| images | Body | Array | Image list object |
| images.status | Body | String | Image status <br>One of `queued`, `saving`, `active`, `killed`, `deleted`, and `pending_delete` |
| images.name | Body | String | Image name |
| images.tag | Body | String | Image tag<br>Take cautions for not deleting the`_AVAILABLE_` tag, resulting in the failed query on console |
| images.container_format | Body | String | Image container format |
| images.created_at | Body | Datetime | Creation time |
| images.disk_format | Body | String | Image disk format |
| images.updated_at | Body | Datetime | Modification time |
| images.min_disk | Body | Integer | Minimum required disk volume of image (GB)<br>Available for only such volumes that are larger than `min_disk` |
| images.protected | Body | Boolean | Protect image or not<br>Unable to modify or delete, for`protected=true` |
| images.id | Body | UUID | Image ID |
| images.min_ram | Body | Integer | Minimum required memory volume of image (MB)<br>Available for only such instances that are larger than `min_disk` |
| images.checksum | Body | String | Hash for image content <br>Applied to check image validity |
| images.owner | Body | String | ID of tenant including image |
| images.visibility | Body | Enum | Image visibility <br>One of `public`, `private`, and `shared` |
| images.virtual_size | Body | Integer | Virtual image size |
| images.size | Body | Integer | Actual image size (bytes) |
| images.properties | Body | Object | Image attribute object <br>Describe user-specified attribute for each image in the key-value pair format |
| images.self | Body | URI | Image path |
| images.file | Body | String | File path of image |
| images.schema | Body | URI | Schema path of image |
| schema | Body | URI | Schema path of image list |
| first | Body | URI | Path of the first page of image list |
| next| Body | URI | Path of the following page of image list |

<details><summary>Example</summary>
<p>


```json
{
  "images": [
    {
      "container_format": "bare",
      "min_ram": 0,
      "updated_at": "2018-12-11T01:01:35Z",
      "login_username": "centos",
      "file": "/v2/images/1c868787-6207-4ff2-a1e7-ae1331d6829b/file",
      "owner": "c289b99209ca4e189095cdecebbd092d",
      "id": "1c868787-6207-4ff2-a1e7-ae1331d6829b",
      "size": 1778843648,
      "os_distro": "CentOS",
      "self": "/v2/images/1c868787-6207-4ff2-a1e7-ae1331d6829b",
      "disk_format": "qcow2",
      "os_version": "6.10",
      "schema": "/v2/schemas/image",
      "status": "active",
      "description": "CentOS 6.10 (2018.10.23)",
      "tags": [],
      "visibility": "public",
      "os_architecture": "amd64",
      "min_disk": 20,
      "virtual_size": null,
      "name": "CentOS 6.10 (2018.10.23)",
      "hypervisor_type": "qemu",
      "created_at": "2018-10-23T02:17:43Z",
      "protected": true,
      "checksum": "f803c5c15bcf9a75935980a900a04584",
      "os_type": "linux"
    }
  ],
  "schema": "/v2/schemas/images",
  "first": "/v2/images",
  "next": "/v2/images?marker=057f9a69-4e4c-4025-8a69-fa248cd9db94"
}
```

</p>
</details>

---

### Get Image

```
GET /v2/images/{imageId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| imageId | URL | UUID | O | Image ID to query |
| tokenId | Header | String | O | Token ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| image.status | Body | String | Image status |
| image.name | Body | String | Image name |
| image.tag | Body | String | Image tags <br>Take cautions for not deleting the`_AVAILABLE_` tag, resulting in the failed query on console |
| image.container_format | Body | String | Image container format |
| image.created_at | Body | Datetime | Creation time |
| image.disk_format | Body | String | Image disk format |
| image.updated_at | Body | Datetime | Modification time |
| image.min_disk | Body | Integer | Minimum required disk volume of image (GB)<br>Available for only such instances that are larger than `min_disk |
| image.protected | Body | boolean | Protect image or not<br/>Unable to modify or delete, for `protected=true` |
| image.id | Body | UUID | Image ID |
| image.min_ram | Body | Integer | Minimum required memory volume of image (MB)<br/>Available for only such instances that are larger than `min_disk` |
| image.checksum | Body | String | Hash for image content <br/>Applied to check image validity |
| image.owner | Body | String | ID of tenant including image |
| image.visibility | Body | Enum | Image visibility<br>One of`public`, `private`, and `shared` |
| image.virtual_size | Body | Integer | Virtual image size |
| image.size | Body | Integer | Actual image size (bytes) |
| image.properties | Body | Object | Image attribute object <br/>Describe user-specified attribute for each image in the key-value pair format |
| image.self | Body | URI | Image path |
| image.file | Body | String | File path of image |
| image.schema | Body | URI| Schema path of image |

<details><summary>Example</summary>
<p>


```json
{
  "container_format": "bare",
  "min_ram": 0,
  "updated_at": "2018-12-11T01:01:35Z",
  "login_username": "centos",
  "file": "/v2/images/1c868787-6207-4ff2-a1e7-ae1331d6829b/file",
  "owner": "c289b99209ca4e189095cdecebbd092d",
  "id": "1c868787-6207-4ff2-a1e7-ae1331d6829b",
  "size": 1778843648,
  "os_distro": "CentOS",
  "self": "/v2/images/1c868787-6207-4ff2-a1e7-ae1331d6829b",
  "disk_format": "qcow2",
  "os_version": "6.10",
  "schema": "/v2/schemas/image",
  "status": "active",
  "description": "CentOS 6.10 (2018.10.23)",
  "tags": [],
  "visibility": "public",
  "os_architecture": "amd64",
  "min_disk": 20,
  "virtual_size": null,
  "name": "CentOS 6.10 (2018.10.23)",
  "hypervisor_type": "qemu",
  "created_at": "2018-10-23T02:17:43Z",
  "protected": true,
  "checksum": "f803c5c15bcf9a75935980a900a04584",
  "os_type": "linux"
}
```

</p>
</details>

---

### Delete Image

Unable to delete images with `public` visibility. 

```
DELETE /v2/images/{imageId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| imageId | URL | String | O | Image ID to delete |
| tokenId | Header | String | O | Token ID |

#### Response
This API does not return response body. 

---

## Image Tag
### Add Tag
Add tags to a specified image. 

```
PUT /v2/images/{imageId}/tags/{tag}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| imageId | URL | UUID | O | ID of image to add tags |
| tag | URL | String | O | Tag name to add (no more than 255 letters in English)<br><font color='red'>**(Caution) Unable to use tags starting with`_`**</font> |
| tokenId | Header | String | O | Token ID |

#### Response
This API does not return response body. 

---

### Remove Tag
Remove tags from a specified image. 


```
DELETE /v2/images/{imageId}/tags/{tag}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| imageId | URL | UUID | O | ID of image to remove tags |
| tag | URL | String | O | Name of tag to remove |
| tokenId | Header | String | O | Token ID |

#### Response
This API does not return a response body. 

---

## Image Sharing 

With Image Sharing, you may share your images of your own tenant with another tenant. 

1. Change image visibility to  `shared`.
2. Register tenant to be shared as member of the image. 

Shared images are readily available by shared tenant but not displayed on the List Images. By changing member status to  `active` for **Shared Tenant**, shared images can be queried. 

### Change Visibility 

```
PATCH /v2/images/{imageId}
X-Auth-Token: {tokenId}
Content-Type: application/openstack-images-v2.1-json-patch
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| imageId | URL | UUID | O | Image ID to share |
| tokenId | Header | String | O | Token ID |
| op | Body | String | O | Specified with `replace` |
| path | Body | String | O | Specified with `/visibility` |
| value | Body | String | O | Visibility value to be changed:  `private` or `shared` |

<details><summary>Example</summary>
<p>


```json
[
    {
        "op" : "replace",
        "path" : "/visibility",
        "value" : "shared"
    }
]
```

</p>
</details>

#### Response

Return the same response as Get Image.  

---

### Add Member
Register tenant to be shared as member of the specified image. 

```
POST /v2/images/{imageId}/members
X-Auth-Token: {tokenId}
```

> Each image is allowed to have up to 127 members. 

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| imageId | URL | UUID | O | ID of image to share |
| tokenId | Header | String | O | Token ID |
| member | Body | String | O | Tenant ID to be shared |

<details><summary>Example</summary>
<p>


```
{
    "member": "8989447062e04a818baf9e073fd04fa7"
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| created_at | Body | Datetime | Member creation time<br>In the `YYYY-MM-DDThh:mm:ssZ` format |
| image_id | Body | UUID | Shared image ID |
| member_id | Body | String | Tenant ID shared with image |
| schema | Body | URI | Schema path for image member |
| status | Body | Enum | Image member status<br>Either`pending` , or `accepted` |

<details><summary>Example</summary>
<p>


```json
{
    "created_at": "2013-09-20T19:22:19Z",
    "image_id": "a96be11e-8536-4910-92cb-de50aa19dfe6",
    "member_id": "8989447062e04a818baf9e073fd04fa7",
    "schema": "/v2/schemas/member",
    "status": "pending",
    "updated_at": "2013-09-20T19:25:31Z"
}
```

</p>
</details>

---

### List Members 
List tenants shared with a specified image. Must request with a token of tenant to which the image is included or shared.  

```
GET /v2/images/{imageId}/members
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| imageId | URL | UUID | O | Image ID |
| tokenId | Header | String | O | Token ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| members | Body | Object | List of member objects |
| members.created_at | Body | Datetime | Member creation time, in the `YYYY-MM-DDThh:mm:ssZ` format |
| members.image_id | Body | UUID | ID of shared ID |
| members.member_id | Body | String | Tenant ID shared with image |
| members.schema | Body | URI | Schema path of image member |
| members.status | Body | Enum | Image member status<br/>Either`pending`, or `accepted` |
| schema | Body | URI | Schema path for the list of image members |

<details><summary>Example</summary>
<p>


```json
{
    "members": [
        {
            "created_at": "2013-10-07T17:58:03Z",
            "image_id": "dbc999e3-c52f-4200-bedd-3b18fe7f87fe",
            "member_id": "123456789",
            "schema": "/v2/schemas/member",
            "status": "pending",
            "updated_at": "2013-10-07T17:58:03Z"
        },
        {
            "created_at": "2013-10-07T17:58:55Z",
            "image_id": "dbc999e3-c52f-4200-bedd-3b18fe7f87fe",
            "member_id": "987654321",
            "schema": "/v2/schemas/member",
            "status": "accepted",
            "updated_at": "2013-10-08T12:08:55Z"
        }
    ],
    "schema": "/v2/schemas/members"
}
```

</p>
</details>

---

### Get Member Details 

Return details of a particular member of specified image. Must request with a token of tenant to which the image is included or shared.  

```
GET /v2/images/{imageId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| imageId | URL | UUID | O | Image ID |
| memberId | URL | String | O | Member ID |
| tokenId | Header | String | O | Token ID |

#### Response

| Name       | Type | Format   | Description                                               |
| ---------- | ---- | -------- | --------------------------------------------------------- |
| created_at | Body | Datetime | Member creation time In the `YYYY-MM-DDThh:mm:ssZ` format |
| image_id   | Body | UUID     | Shared image ID                                           |
| member_id  | Body | String   | Tenant ID shared with image                               |
| schema     | Body | URI      | Schema path for image member                              |
| status     | Body | Enum     | Image member status Either`pending` , or `accepted`       |

<details><summary>Example</summary>
<p>

```json
{
    "status": "pending",
    "created_at": "2013-11-26T07:21:21Z",
    "updated_at": "2013-11-26T07:21:21Z",
    "image_id": "0ae74cc5-5147-4239-9ce2-b0c580f7067e",
    "member_id": "8989447062e04a818baf9e073fd04fa7",
    "schema": "/v2/schemas/member"
}
```

</p>
</details>

---

### Change Member Status 

Approve shared images from shared tenant. Once approved, the images can be queried by List Images. Must request by token of shared tenant. 

```
PUT /v2/images/{imageId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| imageId | URL | UUID | O | Image ID |
| memberId | URL | String | O | Member ID |
| tokenId | Header | String | O | Token ID |
| status  | Body | Enum | O | One of `accepted`, `pending`, and `rejected` |

<details><summary>Example</summary>
<p>


```json
{
    "status": "accepted"
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| created_at | Body | Datetime | Member creation time<br>In the format of`YYYY-MM-DDThh:mm:ssZ` |
| image_id | Body | UUID | ID of shared image |
| member_id | Body | String | Tenant ID shared with image |
| schema | Body | URI | Schema path of image member |
| status | Body | Enum | Image member status <br>One of`accpeted`,`pending`, and `rejected` |
| updated_at | Body | Datetime | Member status modification time<br>In the `YYYY-MM-DDThh:mm:ssZ` format |

<details><summary>Example</summary>
<p>



```json
{
    "created_at": "2013-09-20T19:22:19Z",
    "image_id": "a96be11e-8536-4910-92cb-de50aa19dfe6",
    "member_id": "8989447062e04a818baf9e073fd04fa7",
    "schema": "/v2/schemas/member",
    "status": "accepted",
    "updated_at": "2013-09-20T20:15:31Z"
}
```

</p>
</details>

---

### Delete Member

Delete member of specified image: the purpose is to unshare. Must request with the token of tenant to which specified image is included.  

```
DELETE /v2/images/{imageId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| imageId | URL | UUID | O | Image ID |
| memberId | URL | String | O | Member ID |
| tokenId | Header | String | O | Token ID |

#### Response
This API does not return a response body. 
