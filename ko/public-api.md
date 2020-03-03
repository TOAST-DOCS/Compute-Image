# Image API

## API 버전
### 버전 목록 보기
TOAST 기본 인프라 서비스 Image API에서 지원하는 버전 목록을 확인할 수 있습니다.

```
GET /
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

#### 응답
<details><summary>예시</summary>
<p>

```json
{
    "versions": [
        {
            "status": "CURRENT",
            "id": "v2.2",
            "links": [
                {
                    "href": "http://23.253.228.211:9292/v2/",
                    "rel": "self"
                }
            ]
        },
        {
            "status": "SUPPORTED",
            "id": "v2.1",
            "links": [
                {
                    "href": "http://23.253.228.211:9292/v2/",
                    "rel": "self"
                }
            ]
        },
        {
            "status": "SUPPORTED",
            "id": "v2.0",
            "links": [
                {
                    "href": "http://23.253.228.211:9292/v2/",
                    "rel": "self"
                }
            ]
        },
        {
            "status": "SUPPORTED",
            "id": "v1.1",
            "links": [
                {
                    "href": "http://23.253.228.211:9292/v1/",
                    "rel": "self"
                }
            ]
        },
        {
            "status": "SUPPORTED",
            "id": "v1.0",
            "links": [
                {
                    "href": "http://23.253.228.211:9292/v1/",
                    "rel": "self"
                }
            ]
        }
    ]
}
```

</p>
</details>

---

## 이미지
### 이미지 목록 조회
사용가능한 공용 또는 개인 이미지 목록을 반환합니다.

```
GET /v2/images
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| limit | Query | Integer | - | 반환할 이미지의 갯수<br>기본 값은 1000으로 설정 |
| marker | Query | UUID | - | 조회할 이미지 목록의 첫 번째 이미지 ID<br>정렬 방식에 따라 `marker`로 지정된 이미지부터<br> `limit` 만큼의 이미지 목록을 조회 |
| name | Query | String | - | 조회할 이미지 이름 |
| visibility | Query | Enum | - | 조회할 이미지의 보여주기 속성<br>`public`, `private`, `shared` 중 하나의 값만 선택 가능<br>생략하면 모든 종류의 이미지 목록을 반환 |
| owner | Query | String  | - | 조회할 이미지가 속한 Tenant ID |
| status | Query | Enum    | - | 조회할 이미지의 상태<br>`queued`: 이미지 컨버팅 중<br>`saving`: 이미지 업로드 중<br>`active`: 정상<br>`killed`: 시스템에 의해 이미지 삭제<br>`deleted`: 이미지 삭제<br>`pending_delete`: 이미지 삭제 대기 중 |
| size_min | Query | Integer | - | 조회할 이미지의 최소 크기<br>바이트 단위 |
| size_max | Query | Integer | - | 조회할 이미지의 최대 크기<br>바이트 단위 |
| sort_key | Query | String | - | 이미지 목록을 정렬할 때 사용할 속성<br>이미지의 모든 속성을 지정 가능<br>기본 값은 `created_at` |
| sort_dir | Query | Enum | - | 이미지 목록 정렬 방향<br>`asc` (오름차순), `desc` (내림차순) 중 하나의 값만 선택 가능<br>기본 값은 내림차순 |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| images | Body | Array | 반환된 이미지 목록 객체 |
| images.status | Body | String | 이미지의 상태<br>`queued`, `saving`, `active`, `killed`, `deleted`, `pending_delete` 중 하나 |
| images.name | Body | String | 이미지의 이름 |
| images.tag | Body | String | 이미지의 태그 |
| images.container_format | Body | String | 이미지의 컨테이너 포맷 |
| images.created_at | Body | Datetime | 이미지 생성 시각 |
| images.disk_format | Body | String | 이미지의 디스크 포맷 |
| images.updated_at | Body | Datetime | 이미지 수정 시각 |
| images.min_disk | Body | Integer | 이미지의 최소 디스크 요구량<br>`min_disk` 값보다 큰 볼륨에서만 사용할 수 있음 |
| images.protected | Body | Boolean | 이미지의 보호 여부<br>`protected=true`인 경우 수정 및 삭제가 불가 |
| images.id | Body | UUID | 이미지의 ID |
| images.min_ram | Body | Integer | 이미지의 최소 메모리 요구량<br>`min_disk` 값보다 큰 인스턴스에서만 사용할 수 있음 |
| images.checksum | Body | String | 이미지 내용의 해쉬 값<br>내부적으로 이미지 유효성 검증을 위해 사용 |
| images.owner | Body | String | 이미지 소유주 ID<br>TOAST 기본 인프라 서비스의 이미지는 테넌트 별로 관리됨<br>즉, 이미지 소유주 ID는 이미지가 속한 테넌트 ID |
| images.visibility | Body | Enum | 이미지의 보여주기 속성<br>`public`, `private`, `shared` 중 하나 |
| images.virtual_size | Body | Integer | 이미지의 가상 크기 |
| images.size | Body | Integer | 이미지의 실제 크기<br>바이트 단위 |
| images.properties | Body | Object | 이미지의 속성 객체<br>이미지 별 사용자 지정 속성을 키-값 쌍 형태로 기술 |
| images.self | Body | URI | 이미지의 경로 |
| images.file | Body | String | 이미지 파일 경로 |
| images.schema | Body | URI | 이미지 스키마 경로 |
| schema | Body | URI | 이미지 목록 스키마 경로 |
| first | Body | URI | 이미지 목록의 첫 번째 페이지에 해당하는 경로 |
| next| Body | URI | 이미지 목록의 다음 페이지에 해당하는 경로 |

<details><summary>예시</summary>
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
  ]
}
```

</p>
</details>

---

### 이미지 상세 조회
지정한 이미지에 대한 상세 정보만을 조회합니다.

```
GET /v2/images/{imageId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID|
| imageId | URI | UUID | O | 조회할 이미지 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| image | Body | Object | 반환된 이미지 목록 객체 |
| images.status | Body | String | 이미지의 상태 |
| images.name | Body | String | 이미지의 이름 |
| images.tag | Body | String | 이미지의 태그 |
| images.container_format | Body | String | 이미지의 컨테이너 포맷 |
| images.created_at | Body | Datetime | 이미지 생성 시각 |
| images.disk_format | Body | String | 이미지의 디스크 포맷 |
| images.updated_at | Body | Datetime | 이미지 수정 시각 |
| images.min_disk | Body | Integer | 이미지의 최소 디스크 요구량<br>`min_disk` 값보다 큰 볼륨에서만 사용할 수 있음 |
| images.protected | Body | boolean | 이미지의 보호 여부<br>`protected=true`인 경우 수정 및 삭제가 불가 |
| images.id | Body | UUID | 이미지의 ID |
| images.min_ram | Body | Integer | 이미지의 최소 메모리 요구량<br>`min_disk` 값보다 큰 인스턴스에서만 사용할 수 있음 |
| images.checksum | Body | String | 이미지 내용의 해쉬 값<br>내부적으로 이미지 유효성 검증을 위해 사용 |
| images.owner | Body | String | 이미지 소유주 ID<br>TOAST 기본 인프라 서비스의 이미지는 테넌트 별로 관리됨<br>즉, 이미지 소유주 ID는 이미지가 속한 테넌트 ID |
| images.visibility | Body | Enum | 이미지의 보여주기 속성<br>`public`, `private`, `shared` 중 하나 |
| images.virtual_size | Body | Integer | 이미지의 가상 크기 |
| images.size | Body | Integer | 이미지의 실제 크기<br>바이트 단위 |
| images.properties | Body | Object | 이미지의 속성 객체<br>이미지 별 사용자 지정 속성을 키-값 쌍 형태로 기술 |
| images.self | Body | URI | 이미지의 경로 |
| images.file | Body | String | 이미지 파일 경로 |
| images.schema | Body | URI| 이미지 스키마 경로 |

<details><summary>예시</summary>
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

### 이미지 삭제
```
DELETE /v2/images/{imageId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| imageId | URI | String | O | 조회할 이미지 ID |

#### 응답
이 API는 응답 본문을 반환하지 않습니다.

> 오직 `private` 이미지만 삭제할 수 있습니다.

---

## 이미지 태그
### 태그 추가하기
지정한 이미지에 태그를 추가합니다.

```
PUT /v2/images/{imageId}/tags/{tag}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| imageId | URI | UUID | O | 태그를 추가할 이미지 ID |
| tag | URI | String | O | 추가할 태그 이름<br>영문 기준 최대 255자 |

#### 응답
이 API는 응답 본문을 반환하지 않습니다.

---

### 태그 제거하기
지정한 이미지에서 태그를 제거합니다.

```
DELETE /v2/images/{imageId}/tags/{tag}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| imageId | URI | UUID | O | 태그를 제거할 이미지 ID |
| tag | URI | String | O | 제거할 태그 이름 |

#### 응답
이 API는 응답 본문을 반환하지 않습니다.

---

## 이미지 공유
TOAST 기본 인프라 서비스의 이미지 API는 자신의 이미지를 다른 테넌트에게 공유할 수 있는 기능을 제공합니다. 이미지 공유를 위해서는 공유할 이미지에 공유할 테넌트를 **member** 로 등록하면 됩니다. 그리고 **member** 로 등록된 테턴트에서 공유받은 이미지에 대해서 승인하면, 자신의 이미지처럼 조회 및 사용하는 것이 가능해집니다.

> 오직 `visibility` 속성이 `private`인 이미지만 공유할 수 있습니다.
>
> 반드시 공유하려는 이미지를 소유한 테넌트에서 **member** 를 추가해야 합니다.

### Member 추가
새로운 테넌트를 지정한 이미지의 **member** 로 등록합니다.

```
POST /v2/images/{imageId}/members
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| imageId | URI | UUID | O | 공유할 이미지 ID |
| member | Body | String | O | 공유받을 테넌트 ID |

<details><summary>예시</summary>
<p>

```
{
    "member": "8989447062e04a818baf9e073fd04fa7"
}
```

</p>
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| created_at | Body | Datetime | **Member** 생성 시각<br>yyyy-mm-ddTHH:MM:SSZ 형식 |
| image_id | Body | UUID | 공유한 이미지 ID |
| member_id | Body | String | 이미지를 공유받은 테넌트 ID |
| schema | Body | URI | 이미지 **Member**에 대한 스키마 경로 |
| status | Body | Enum | 이미지 **Member** 상태<br>`pending`, `accepted` 중 하나 |

<details><summary>예시</summary>
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

### Member 목록 보기
지정한 이미지를 공유 받은 테넌트 목록을 조회합니다.
반드시 지정한 이미지의 소유주 또는 **member** 로 등록된 테넌트에 속한 토큰으로 요청해야 합니다.

```
GET /v2/images/{imageId}/members
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| imageId | URI | UUID | O | 이미지 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| members | Body | Object | **Member** 목록 |
| members.created_at | Body | Datetime | **Member** 생성 시각 yyyy-mm-ddTHH:MM:SSZ 형식       |
| members.image_id | Body | UUID | 공유한 이미지 ID |
| members.member_id | Body | String | 이미지를 공유받은 테넌트 ID |
| members.schema | Body | URI | 이미지 **member**에 대한 스키마 경로 |
| members.status | Body | Enum | 이미지 **member** 상태 `pending`, `accepted` 중 하나 |
| schema | Body | URI | 이미지 **member** 목록에 대한 스키마 경로 |

<details><summary>예시</summary>
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

### Member 상세 보기
지정한 이미지의 특정 **member** 에 대한 상세 정보만을 반환합니다.
반드시 지정한 이미지의 소유주 또는 **member** 로 등록된 테넌트에 속한 토큰으로 요청해야 합니다.

```
GET /v2/images/{imageId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| imageId | URI | UUID | O | 이미지 ID |
| memberId | URI | String | O | **Member** ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| created_at | Body | Datetime | **Member** 생성 시각 yyyy-mm-ddTHH:MM:SSZ 형식 |
| image_id | Body | UUID | 공유한 이미지 ID |
| member_id | Body | String | 이미지를 공유받은 테넌트 ID |
| schema | Body | URI | 이미지 **Member**에 대한 스키마 경로 |
| status | Body | Enum | 이미지 **Member** 상태 `pending`, `accepted` 중 하나 |

<details><summary>예시</summary>
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

### Member 상태 변경

**Member** 로 등록된 테넌트에서 공유 받은 이미지를 승인합니다.
반드시 지정한 이미지의 **Member** 로 등록된 테넌트에 속한 토큰으로 요청해야 합니다.

```
PUT /v2/images/{imageId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| imageId | URI | UUID | O | 이미지 ID |
| status  | Body | Enum | O | `accpeted`, `pending`, `rejected` 중 하나 |

<details><summary>예시</summary>
<p>

```json
{
    "status": "accepted"
}
```

</p>
</details>

#### 응답

| 이름 | 종류 | 유형 | 설명 |
|---|---|---|---|
| created_at | Body | Datetime | **Member** 생성 시각<br> yyyy-mm-ddTHH:MM:SSZ 형식 |
| image_id | Body | UUID | 공유한 이미지 ID |
| member_id | Body | String | 이미지를 공유받은 테넌트 ID |
| schema | Body | URI | 이미지 **member**에 대한 스키마 경로 |
| status | Body | Enum | 이미지 **member** 상태 <br> `accpeted`,`pending`,`rejected` 중 하나 |
| updated_at | Body | Datetime | 이미지 **member** 상태 수정 시각<br>yyyy-mm-ddTHH:MM:SSZ 형식 |

<details><summary>예시</summary>
<p>

json
```
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

### Member 삭제

지정한 이미지의 **Member** 를 제거합니다. 공유를 취소할 때 사용합니다.
반드시 지정한 이미지의 소유주로 등록된 테넌트에 속한 토큰으로 요청해야 합니다.

```
DELETE /v2/images/{imageId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| imageId | URI | UUID | O | 이미지 ID |
| memberId | URI | String | O | **Member** ID |

#### 응답
이 API는 응답 본문을 반환하지 않습니다.
