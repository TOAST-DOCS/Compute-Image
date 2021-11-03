## Compute > Image > API 가이드

API는 현재 한국 리전에서만 사용할 수 있습니다.

이미지 API에서는 이미지의 목록을 조회하는 API만 제공합니다. 이미지 생성 API는 [인스턴스 추가 기능 API](/Compute/Instance/ko/api-guide/#api_4)를 참고합니다.

## 사전 준비

이미지 API를 사용하려면 앱키(Appkey)와 토큰이 필요합니다. [API Endpoint URL](/Compute/Instance/ko/api-guide/#api-endpoint-url)과 [토큰 API](/Compute/Instance/ko/api-guide/#api)를 이용하여 앱키와 토큰을 준비합니다. 앱키는 API Endpoint URL에, 토큰은 Request Body에 포함하여 사용합니다.

예를 들어, 이미지 목록 조회는 다음 URL로 요청해야 합니다.

	GET https://api-compute.cloud.toast.com/compute/v1.0/appkeys/{appkey}/images

## 이미지 상태
이미지는 다음의 상탯값을 갖습니다.

| 상태 | 설명 |
| -- | -- |
| queued | 이미지 ID는 발급되었으나 아직 이미지 데이터가 업로드되지 못한 상태 |
| saving | 이미지 데이터를 저장 중인 상태 |
| active | 이미지를 사용할 수 있는 상태 |
| killed | 이미지 데이터 업로드 중 오류가 발생한 상태 |
| deleted | 이미지에 대한 정보는 남아있으나 더 이상 가용할 수 없는 상태 |
| pending_delete | deleted 상태와 유사, 이미지를 회복할 수 없는 상태 |
| deactivated | 이미지 데이터를 사용할 수 없는 상태 |

## 이미지 API

### 이미지 목록 조회

이미지의 목록 및 상세 정보를 조회합니다.

#### Method, URL
```
GET /v1.0/appkeys/{appkey}/images?limit={limit}&marker={markerId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |
| limit | Query | Integer | O | 조회할 이미지 갯수 |
| markerId | Query | UUID | O | 조회 시 기준이 되는 이미지 ID<br>이미지 목록은 생성 일자 순으로 정렬됩니다.<br>limit과 maker를 지정하는 경우 marker로 지정된 이미지부터 limit 갯수만큼 조회합니다 |

#### Request Body
이 API는 Request Body가 필요 없습니다.

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
| Created At | Body | String  | 이미지 생성 시간. yyyy-mm-ddTHH:MM:ssZ의 형태. 예) 2017-05-16T02:17:50.166563 |
| Disk Format | Body | String | 이미지의 디스크 형식. <br />"ami", "ari", "aki", "vhd", "vhdx", "vmdk", "raw", "qcow2", "vdi", "ploop", "iso" |
| Image ID | Body | String | 이미지 ID |
| Is Public | Body | Boolean | 퍼블릭 이미지 여부 |
| Min Disk | Body | Integer | 이 이미지로 만들 수 있는 인스턴스의 최소 디스크 크기(GB) |
| Min RAM | Body | Integer | 이 이미지로 만들 수 있는 인스턴스의 최소 RAM 크기(MB) |
| Image Name | Body | String | 이미지 이름 |
| Prop Key / Prop Value | Body | String | 이미지의 추가적인 속성 |
| Protected | Body | Boolean | 삭제 보호 설정 여부 |
| Image Size | Body | Integer | 이미지 데이터의 크기(byte) |
| Image Status | Body | String | 이미지의 상태 |
| Updated At | Body | String | 이미지가 업데이트된 시간. yyyy-mm-ddTHH:MM:ssZ의 형태. 예) 2017-05-16T02:17:50.166563 |

### 이미지 삭제

지정한 이미지를 삭제 합니다. 단, 사용자가 생성한 이미지들만 삭제할 수 있습니다.

#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/images?id={imageId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| imageId | Query | UUID | - | 이미지 ID |
| tokenId | Header | String | - | 토큰 ID |

#### Request Body
이 API는 Request Body가 필요 없습니다.

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
