## Compute > Image > API 가이드

이미지 API에서는 이미지의 목록을 조회하는 API만 제공합니다. 이미지 생성 API는 [Instance API 가이드](ko/Compute/Instance/ko/api-guide/)의 **인스턴스 추가 기능** 부분을 참조합니다.

이미지 API를 사용하려면 토큰 발급과 같은 사전 준비가 필요합니다. [API 사용 준비 가이드](/Infrastructure%20Common/ko/api-guide/)를 참조합니다.

### 이미지 상태
이미지는 다음의 상태 값을 갖습니다.

| 상태 | 설명 |
| -- | -- |
| queued | 이미지 ID는 발급되었으나 아직 이미지 데이터가 업로드 되지 못한 상태 |
| saving | 이미지 데이터를 저장 중인 상태 |
| active | 이미지 사용 가능 상태 |
| killed | 이미지 데이터 업로드 중 에러 발생 |
| deleted | 이미지에 대한 정보는 남아있으나 더 이상 가용하지 않은 상태 |
| pending_delete | deleted 상태와 유사, 이미지가 회복 불가능한 상태 |
| deactivated | 이미지 데이터가 사용 불가한 상태 |

### 이미지 목록 조회

이미지의 목록 및 상세 정보를 조회합니다.

#### Method, URL
```
GET /v1.0/appkeys/{appkey}/images
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |

#### Request Body
이 API는 request body를 필요로 하지 않습니다.

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
            "name": "Image Name",
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
| Min Disk | Body | Integer | 이 이미지로 만들 수 있는 인스턴스의 최소 디스크 크기 (GB) |
| Min RAM | Body | Integer | 이 이미지로 만들 수 있는 인스턴스의 최소 RAM 크기 (MB) |
| Image Name | Body | String | 이미지 이름 |
| Prop Key / Prop Value | Body | String | 이미지의 추가적인 속성 |
| Protected | Body | Boolean | 삭제 보호 설정 여부 |
| Image Size | Body | Integer | 이미지 데이터의 크기 (byte) |
| Image Status | Body | String | 이미지의 상태 |
| Updated At | Body | String | 이미지가 업데이트 된 시간. yyyy-mm-ddTHH:MM:ssZ의 형태. 예) 2017-05-16T02:17:50.166563 |

