## 计算 > 镜像 > API指南

API目前仅限韩国地区使用。

API只提供镜像列表查询API。有关镜像创建API，请参考[实例新增功能API](/Compute/Instance/zh/api-guide/#api_4)。

## 事前准备

如果要使用API需要设置Appkey和令牌。使用[API Endpoint URL](/Compute/Instance/zh/api-guide/#api-endpoint-url)和[令牌API](/Compute/Instance/zh/api-guide/#api)准备Appkey和令牌。使用时将Appkey包含在API Endpoint URL中，令牌包含在Request Body中。

例如，查询镜像列表应向以下URL请求。

	GET https://api-compute.cloud.toast.com/compute/v1.0/appkeys/{appkey}/images

## 镜像状态
镜像具备以下状态值。

| 状态 | 描述 |
| -- | -- |
| queued | 表示镜像ID已发放，但镜像数据尚未上传 |
| saving | 表示镜像数据正在保存 |
| active | 表示镜像可以使用 |
| killed | 表示上传镜像数据时发生错误 |
| deleted | 表示有关镜像的信息仍然存在，但不再可用 |
| pending_delete | 与deleted状态类似，表示镜像无法复原 |
| deactivated | 表示镜像数据不可用 |

## 镜像API

### 查看镜像列表

查看镜像列表及详情。

#### Method, URL
```
GET /v1.0/appkeys/{appkey}/images?limit={limit}&marker={markerId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 令牌ID |
| limit | Query | Integer | O | 要查询的镜像个数 |
| markerId | Query | UUID | O | 查询时作为标准的镜像ID<br>镜像列表按照创建日期排序。<br>若指定limit及maker，则从指定为marker的镜像开始按照limit个数查询> |


#### Request Body
此API不需要Request Body。

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
| Created At | Body | String  | 镜像创建时间。 yyyy-mm-ddTHH:MM:ssZ格式。例如) 2017-05-16T02:17:50.166563 |
| Disk Format | Body | String | 镜像的磁盘格式。 <br />"ami", "ari", "aki", "vhd", "vhdx", "vmdk", "raw", "qcow2", "vdi", "ploop", "iso" |
| Image ID | Body | String | 镜像ID |
| Is Public | Body | Boolean | 是否为公共镜像 |
| Min Disk | Body | Integer | 通过该镜像可以创建的实例的最小磁盘大小(GB) |
| Min RAM | Body | Integer | 通过该镜像可以创建的实例的最小RAM大小(MB) |
| Image Name | Body | String | 镜像名称 |
| Prop Key / Prop Value | Body | String | 镜像的其它属性 |
| Protected | Body | Boolean | 是否设置删除保护 |
| Image Size | Body | Integer | 镜像数据的大小(byte) |
| Image Status | Body | String | 镜像状态 |
| Updated At | Body | String | 镜像上传时间。 yyyy-mm-ddTHH:MM:ssZ格式。 例如) 2017-05-16T02:17:50.166563 |

### 删除镜像

删除指定的镜像。但仅可删除用户创建的镜像。

#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/images?id={imageId}
X-Auth-Token:{tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| imageId | Query | UUID | - | 镜像ID |
| tokenId | Header | String | - | 令牌ID |

#### Request Body
该API不需要Request Body。

#### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```
