## Compute > Image > API Guide

Image API provides List Images API only. For Create Image API, refer to [Add Instance API](/Compute/Instance/ko/api-guide/#_15).  

## Prerequisites

Using an image API requires an appkey and a token. Get an appkey and a token by using [API Endpoint URL](/Compute/Instance/ko/api-guide/#api-endpoint-url) and [Token API](/Compute/Instance/ko/api-guide/#api): include the appkey to Endpoint URL and the token to the Request Body. 

For example, List Images must be requested to the following URL:

	GET https://api-compute.cloud.toast.com/compute/v1.0/appkeys/{appkey}/images

## Image Status 
Images have the following status values:

| Status | Description |
| -- | -- |
| queued | ID has been issued but data has not been uploaded |
| saving | Saving image data |
| active | Image use is available |
| killed | Error occurred during data uploads |
| deleted | Data remains but not available any more |
| pending_delete | Similar to 'deleted' but image is irrevocable |
| deactivated | Data use is unavailable |

## Image API

### List Images 

List images with details. 

#### Method, URL
```
GET /v1.0/appkeys/{appkey}/images
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | Token ID |

#### Request Body
This API does not require a request body. 

#### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "images": [
        {
            "createdAt": "{Created At}",
            "diskFormat": "{Disk Format}",
            "id": "{Image ID}",
            "isPublic": "{Is Public}",
            "minDisk": "{Min Disk}",
            "minRam": "{Min RAM}",
            "name": "{Image Name}",
            "properties": {
            	"{Prop Key}" : "{Prop Value}"
            },
            "protected": "{Protected}",
            "size": "{Image Size}",
            "status": "{Image Status}",
            "updatedAt": "{Updated At}"
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Created At | Body | String  | Time when image is created: in the format of yyyy-mm-ddTHH:MM:ssZ. e.g) 2017-05-16T02:17:50.166563 |
| Disk Format | Body | String | Disk format of an image. <br />"ami", "ari", "aki", "vhd", "vhdx", "vmdk", "raw", "qcow2", "vdi", "ploop", "iso" |
| Image ID | Body | String | ID of an image |
| Is Public | Body | Boolean | If the image is public or not |
| Min Disk | Body | Integer | Minimum disk size of an instance to be made available with this image (GB) |
| Min RAM | Body | Integer | Minimum RAM size of an instance to be made available with this image (MB) |
| Image Name | Body | String | Name of an Image |
| Prop Key / Prop Value | Body | String | Additional attributes of an image |
| Protected | Body | Boolean | Whether to set protected from deletion |
| Image Size | Body | Integer | Size of image data  (byte) |
| Image Status | Body | String | Status of an image |
| Updated At | Body | String | Time when an image is updated: in the format of yyyy-mm-ddTHH:MM:ssZ. e.g) 2017-05-16T02:17:50.166563 |
