## Compute > Image > API v2 Guide

To use the API, API endpoint and token are required. Refer to [API usage preparations](/Compute/Compute/en/identity-api/) to prepare the information required to use the API.

Image API uses the `image` type endpoint. Refer to the `serviceCatalog` in the token issuance response for the valid endpoint.

| Type | Region | Endpoint |
|---|---|---|
| image | Korea (Pangyo) Region<br>Korea (Pyeongchon) Region<br>Japan Region | https://kr1-api-image-infrastructure.nhncloudservice.com<br>https://kr2-api-image-infrastructure.nhncloudservice.com<br>https://jp1-api-image-infrastructure.nhncloudservice.com |

In API response, you may find fields that are not specified in the guide. These fields are only for the internal use by NHN Cloud and are subject to change without prior notice, so we advise you not to use them.

## Image
### List Images

```
GET /v2/images
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description                                                                                                                                                                                                           |
|---|---|---|---|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tokenId | Header | String | O | Token ID                                                                                                                                                                                                              |
| limit | Query | Integer | - | Image count to return (default is 25)                                                                                                                                                                                 |
| marker | Query | UUID | - | ID of the first image on the list to query<br>Query as much as the `limit` after image specified as the `marker` according to the sorting order                                                                       |
| name | Query | String | - | Name of image to query                                                                                                                                                                                                |
| visibility | Query | Enum | - | Visibility attribute of the image to query<br>Select only one of `public`, `private`, and `shared`<br>If left blank, list of all types of images are returned.                                                        |
| owner | Query | String  | - | ID of the tenant to which the image to query belongs                                                                                                                                                                  |
| status | Query | Enum    | - | Image status to query<br>`queued`: Converting image<br>`saving`: Uploading image<br>`active`: Normal<br>`killed`: Deleting image from system<br>`deleted`: Image deleted<br>`pending_delete`: Delete Image is pending |
| size_min | Query | Integer | - | Minimum size of image to query (bytes)                                                                                                                                                                                |
| size_max | Query | Integer | - | Maximum size of image to query (bytes)                                                                                                                                                                                |
| nhncloud_product | Query | Enum | - | Infrastructure service type of image to query<br>`compute`: Instance service image<br>`gpu`: GPU Instance service image                                                                                               |
| sort_key | Query | String | - | Attribute to use when sorting the image list<br>All attributes of image can be specified, default is `created_at`                                                                                                     |
| sort_dir | Query | Enum | - | Sorting direction of the image list<br>Select only one of `asc` (ascending order) or `desc` (descending order)                                                                                                        |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| images | Body | Array | Image list object |
| images.status | Body | String | Image status<br>One of `queued`, `saving`, `active`, `killed`, `deleted`, and `pending_delete` |
| images.name | Body | String | Image name |
| images.tags | Body | Array | Image tag list<br>If you delete the `_AVAILABLE_` tag, it will not be queried in the console, so be careful not to delete the tag. |
| images.container_format | Body | String | Image container format |
| images.created_at | Body | Datetime | Creation time |
| images.disk_format | Body | String | Image disk format |
| images.updated_at | Body | Datetime | Modification time |
| images.min_disk | Body | Integer | Minimum required disk size of image (GB)<br>Available only for block storage that are larger than `min_disk` |
| images.protected | Body | Boolean | Protect image or not<br>Cannot be modified or deleted when `protected=true` |
| images.id | Body | UUID | Image ID |
| images.min_ram | Body | Integer | Minimum required memory size of image (MB)<br>Available only for instances that are larger than `min_disk` |
| images.checksum | Body | String | Hash for image content<br>Used internally for image validation |
| images.owner | Body | String | ID of the tenant to which the image belongs |
| images.visibility | Body | Enum | Image visibility<br>One of `public`, `private`, and `shared` |
| images.virtual_size | Body | Integer | Virtual size of the image |
| images.size | Body | Integer | Real size of the image (bytes) |
| images.properties | Body | Object | Image properties object<br>Describes user-specified properties for each image in the key-value pair format |
| images.self | Body | URI | Image path |
| images.file | Body | String | File path of image |
| images.schema | Body | URI | Schema path of image |
| schema | Body | URI | Schema path of image list |
| first | Body | URI | Path of the first page of image list |
| next| Body | URI | Path of the next page of image list |

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
| tokenId | Header | String | O | Token ID|

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| status | Body | String | Image status |
| name | Body | String | Image name |
| tags | Body | String | Image tag list<br>If you delete the `_AVAILABLE_` tag, it will not be queried in the console, so be careful not to delete the tag. |
| container_format | Body | String | Image container format |
| created_at | Body | Datetime | Creation time |
| disk_format | Body | String | Image disk format |
| updated_at | Body | Datetime | Modification time |
| min_disk | Body | Integer | Minimum required disk size of image (GB)<br>Available only for block storage that are larger than `min_disk` |
| protected | Body | Boolean | Protect image or not<br>Cannot be modified or deleted when `protected=true` |
| id | Body | UUID | Image ID |
| min_ram | Body | Integer | Minimum required memory size of image (MB)<br>Available only for instances that are larger than `min_disk` |
| checksum | Body | String | Hash for image content<br>Used internally for image validation |
| owner | Body | String | ID of the tenant to which the image belongs |
| visibility | Body | Enum | Image visibility<br>One of `public`, `private`, and `shared` |
| virtual_size | Body | Integer | Virtual size of the image |
| size | Body | Integer | Real size of the image (bytes) |
| properties | Body | Object | Image properties object<br>Describes user-specified properties for each image in the key-value pair format |
| self | Body | URI | Image path |
| file | Body | String | File path of image |
| schema | Body | URI| Schema path of image |

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

### Create Image

```
POST /v2/images
X-Auth-Token: {tokenId}
```

#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| container_format | Body | String | - | Image container format |
| disk_format | Body | String | - | Image disk format |
| min_disk | Body | Integer | - | Minimum required disk size of image (GB) |
| min_ram | Body | Integer | - | Minimum required memory size of image (MB) |
| protected | Body | Boolean | - | Whether to protect image, true or false |
| tags | Body | Array | - | Image tag list<br>If you delete the `_AVAILABLE_` tag, it will not be queried in the console, so be careful not to delete the tag. |
| visibility | Body | String | - | Image visibility<br>One of `public`, `private`, and `shared` |

<details><summary>Example</summary>
<p>

```json
{
    "container_format": "bare",
    "disk_format": "raw",
    "name": "Ubuntu",
}
```

<p>
</details>

#### Response
| Name | Type | Format | Description |
|---|---|---|---|
| status | Body | String | Image status<br>One of `queued`, `saving`, `active`, `killed`, `deleted`, and `pending_delete` |
| name | Body | String | Image name |
| tags | Body | String | Image tag list<br>If you delete the `_AVAILABLE_` tag, it will not be queried in the console, so be careful not to delete the tag. |
| container_format | Body | String | Image container format |
| created_at | Body | Datetime | Creation time |
| disk_format | Body | String | Image disk format |
| updated_at | Body | Datetime | Modification time |
| min_disk | Body | Integer | Minimum required disk size of image (GB)<br>Available only for block storage that are larger than `min_disk` |
| protected | Body | Boolean | Protect image or not<br>Cannot be modified or deleted when `protected=true` |
| id | Body | UUID | Image ID |
| min_ram | Body | Integer | Minimum required memory size of image (MB)<br>Available only for instances that are larger than `min_disk` |
| checksum | Body | String | Hash for image content<br>Used internally for image validation |
| owner | Body | String | ID of the tenant to which the image belongs |
| visibility | Body | Enum | Image visibility<br>One of `public`, `private`, and `shared` |
| virtual_size | Body | Integer | Virtual size of the image |
| size | Body | Integer | Real size of the image (bytes) |
| properties | Body | Object | Image properties object<br>Describes user-specified properties for each image in the key-value pair format |
| self | Body | URI | Image path |
| file | Body | String | File path of image |
| schema | Body | URI| Schema path of image |

<details><summary>Example</summary>
<p>

```json
{
    "status": "queued",
    "name": "Ubuntu",
    "tags": [],
    "container_format": "bare",
    "created_at": "2015-11-29T22:21:42Z",
    "size": null,
    "disk_format": "raw",
    "updated_at": "2015-11-29T22:21:42Z",
    "visibility": "private",
    "locations": [],
    "self": "/v2/images/b2173dd3-7ad6-4362-baa6-a68bce3565cb",
    "min_disk": 0,
    "protected": false,
    "id": "b2173dd3-7ad6-4362-baa6-a68bce3565cb",
    "file": "/v2/images/b2173dd3-7ad6-4362-baa6-a68bce3565cb/file",
    "checksum": null,
    "os_hash_algo": null,
    "os_hash_value": null,
    "os_hidden": false,
    "owner": "bab7d5c60cd041a0a36f7c4b6e1dd978",
    "virtual_size": null,
    "min_ram": 0,
    "schema": "/v2/schemas/image"
}
```

<p>
</details>

---

### Upload Image

Uploads the actual image file to the specified image.

```
PUT /v2/images/{imageId}/file
X-Auth-Token: {tokenId}
Content-Type: application/octet-stream
```

#### Request
For a request, the content type for the header must be set to application/octet-stream.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| imageId | URL | UUID | O | Image ID |
| tokenId | Header | String | O | Token ID |
| -       | Body | Binary | O | The binary data of the image file to be uploaded |

#### Response
This API does not return request body. When the request is appropriate, return status code 204.

---

### Download Image

Downloads the binary data of the specified image.

```
GET /v2/images/{imageId}/file
X-Auth-Token: {tokenId}
```

#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| imageId | URL | UUID | O | Image ID |
| tokenId | Header | String | O | Token ID |

#### Response
The binary data for the image is returned. For a valid request, return status code 200.

---

### Modify Image

Modifies image properties through modification.

```
PATCH /v2/images/{imageId}
X-Auth-Token: {tokenId}
Content-Type: application/openstack-images-v2.1-json-patch
```

#### Request
For a request, the content type for the header must be set to application/openstack-images-v2.1-json-patch.

| Name | Type | Format     | Required | Description                                                                           |
|---|---|--------|----|------------------------------------------------------------------------------|
| imageId | URL | UUID   | O  | Image ID to modify                                                                   |
| tokenId | Header | String | O  | Token ID                                                                        |
| op | Body | Enum   | O  | Type of task to modify</br>`add`: Add a property</br>`replace`: Modify a property value</br>`remove`: Delete a property |
| path | Body | String | O  | Property to modify</br>`/{path}` format                                                      |
| value | Body | String | -  | The value of the property to modify                                                                    |

<details><summary>Example</summary>
<p>

```json
// Add a property
[
    {
        "op": "add",
        "path": "/metadata1",
        "value": "value1"
    },
    {
        "op": "add",
        "path": "/metadata2",
        "value": "1"
    }
]

// Modify property value.
[
    {
        "op": "replace",
        "path": "/metadata1",
        "value": "value2"
    }
]

// delete property
[
    {
        "op": "remove",
        "path": "/metadata1"
    }
]
```

<p>
</details>

#### Response

Returns the same response as Get Image.

---

### Delete Image

Images with `public` visibility cannot be deleted.

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
This API does not return a response body.

---

## Image Tag
### Add Tag
Adds a tag to the specified image.

```
PUT /v2/images/{imageId}/tags/{tag}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| imageId | URL | UUID | O | ID of the image to add a tag |
| tag | URL | String | O | Name of the tag to add (no more than 255 letters in English)<br><font color='red'>**(Caution) Tags starting with `_` are not allowed**</font> |
| tokenId | Header | String | O | Token ID |

#### Response
This API does not return a response body.

---

### Remove Tag
Removes a tag from the specified image.


```
DELETE /v2/images/{imageId}/tags/{tag}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| imageId | URL | UUID | O | ID of the image to remove the tag |
| tag | URL | String | O | Name of the tag to remove |
| tokenId | Header | String | O | Token ID |

#### Response
This API does not return a response body.

---

## Image Sharing

Image sharing allows you to share images belonging to your tenant with other tenants. You can share your images as follows:

1. Change the image visibility to `shared`.
2. Register the tenant to share the image with as a member of the image.

The shared image becomes immediately available to the target tenant, but is not displayed in the image list query. If you change the member status to `active` in **Shared Tenant**, the shared image can be queried.

### Change Visibility

```
PATCH /v2/images/{imageId}
X-Auth-Token: {tokenId}
Content-Type: application/openstack-images-v2.1-json-patch
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| imageId | URL | UUID | O | ID of the image to share |
| tokenId | Header | String | O | Token ID |
| op | Body | String | O | Specify as `replace` |
| path | Body | String | O | Specify as `/visibility` |
| value | Body | String | O | Visibility value to change, `private` or `shared` |

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

Returns the same response as Get Image.

---

### Add Member
Registers the tenant to share the image with as a member of the specified image.

```
POST /v2/images/{imageId}/members
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| imageId | URL | UUID | O | ID of the image to share |
| tokenId | Header | String | O | Token ID |
| member | Body | String | O | Tenant ID to share the image with |

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
| created_at | Body | Datetime | Member creation time<br>In `YYYY-MM-DDThh:mm:ssZ` format |
| image_id | Body | UUID | ID of shared image |
| member_id | Body | String | ID of the target tenant for image sharing |
| schema | Body | URI | Schema path of the image member |
| status | Body | Enum | Status of the image member<br>Either `pending` or `accepted` |

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
Retrieves the list of tenants that the specified image has been shared with. The request must be made with the token of the tenant to which the image belongs or the target tenant of the image sharing.

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
| members.created_at | Body | Datetime | Member creation time, in `YYYY-MM-DDThh:mm:ssZ` format       |
| members.image_id | Body | UUID | ID of shared image |
| members.member_id | Body | String | ID of the target tenant for image sharing |
| members.schema | Body | URI | Schema path of the image member |
| members.status | Body | Enum | Status of the image member<br/>Either `pending` or `accepted` |
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

Returns details of a particular member of specified image. The request must be made with the token of the tenant to which the image belongs or the target tenant of the image sharing.

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

| Name | Type | Format | Description |
|---|---|---|---|
| created_at | Body | Datetime | Member creation time, in `YYYY-MM-DDThh:mm:ssZ` format |
| image_id | Body | UUID | ID of shared image |
| member_id | Body | String | ID of the target tenant for image sharing |
| schema | Body | URI | Schema path of the image member |
| status | Body | Enum | Status of the image member<br/>Either `pending` or `accepted` |

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

The shared image is approved by the sharing target tenant. If the image sharing is approved, the image can be queried in the image list query. The request must be made with the sharing target tenant's token.

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
| status  | Body | Enum | O | One of `accepted`, `pending`, or `rejected` |

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
| created_at | Body | Datetime | Member creation time<br>In `YYYY-MM-DDThh:mm:ssZ` format |
| image_id | Body | UUID | ID of the shared image |
| member_id | Body | String | ID of the target tenant for image sharing |
| schema | Body | URI | Schema path of the image member |
| status | Body | Enum | Status of the image member<br>One of `accepted`,`pending`, or `rejected` |
| updated_at | Body | Datetime | Member status modification time<br>In `YYYY-MM-DDThh:mm:ssZ` format |

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

Deletes a member of the specified image. This is used to cancel sharing. The request must be made with the token of the tenant to which the specified image belongs.

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

