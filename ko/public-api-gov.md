## Compute > Image > API v2 가이드

API를 사용하려면 API 엔드포인트와 토큰 등이 필요합니다. [API 사용 준비](/Compute/Compute/ko/identity-api-gov/)를 참고하여 API 사용에 필요한 정보를 준비합니다.

이미지 API는 `image` 타입 엔드포인트를 이용합니다. 정확한 엔드포인트는 토큰 발급 응답의 `serviceCatalog`를 참조합니다.

| 타입 | 리전 | 엔드포인트 |
|---|---|---|
| image | 한국(판교) 리전 | https://gov-api-image.infrastructure.cloud.toast.com |

API 응답에 가이드에 명시되지 않은 필드가 나타날 수 있습니다. 이런 필드는 NHN Cloud 내부 용도로 사용되며 사전 공지 없이 변경될 수 있으므로 사용하지 않습니다.

## 이미지
### 이미지 목록 조회

```
GET /v2/images
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| limit | Query | Integer | - | 반환할 이미지 개수(기본값은 20) |
| marker | Query | UUID | - | 조회할 이미지 목록의 첫 번째 이미지 ID<br>정렬 방식에 따라 `marker`로 지정된 이미지부터 `limit`만큼의 이미지 목록을 조회 |
| name | Query | String | - | 조회할 이미지 이름 |
| visibility | Query | Enum | - | 조회할 이미지의 보여 주기 속성<br>`public`, `private`, `shared` 중 하나의 값만 선택 가능<br>생략하면 모든 종류의 이미지 목록 반환 |
| owner | Query | String  | - | 조회할 이미지가 속한 테넌트 ID |
| status | Query | Enum    | - | 조회할 이미지 상태<br>`queued`: 이미지 변환 중<br>`saving`: 이미지 업로드 중<br>`active`: 정상<br>`killed`: 시스템에서 이미지 삭제<br>`deleted`: 삭제된 이미지<br>`pending_delete`: 이미지 삭제 대기 중 |
| size_min | Query | Integer | - | 조회할 이미지의 최소 크기(바이트) |
| size_max | Query | Integer | - | 조회할 이미지의 최대 크기(바이트) |
| nhncloud_product | Query | String | - | 조회할 이미지의 인프라 상품 종류<br>`compute`: compute 상품 이미지<br>`gpu`: gpu 상품 이미지 |
| sort_key | Query | String | - | 이미지 목록을 정렬할 때 사용할 속성<br>이미지의 모든 속성을 지정 가능, 기본값은 `created_at` |
| sort_dir | Query | Enum | - | 이미지 목록 정렬 방향<br>`asc`(오름차순), `desc`(내림차순) 중 하나의 값만 선택 가능, 기본값은 내림차순 |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| images | Body | Array | 이미지 목록 객체 |
| images.status | Body | String | 이미지 상태<br>`queued`, `saving`, `active`, `killed`, `deleted`, `pending_delete` 중 하나 |
| images.name | Body | String | 이미지 이름 |
| images.tag | Body | String | 이미지 태그<br>`_AVAILABLE_` 태그를 삭제하면 콘솔에서는 조회되지 않으므로, 태그를 삭제하지 않도록 주의 |
| images.container_format | Body | String | 이미지 컨테이너 포맷 |
| images.created_at | Body | Datetime | 생성 시각 |
| images.disk_format | Body | String | 이미지 디스크 포맷 |
| images.updated_at | Body | Datetime | 수정 시각 |
| images.min_disk | Body | Integer | 이미지 최소 디스크 요구량(GB)<br>`min_disk`값보다 큰 볼륨에서만 사용할 수 있음 |
| images.protected | Body | Boolean | 이미지 보호 여부<br>`protected=true`인 경우 수정 및 삭제 불가 |
| images.id | Body | UUID | 이미지 ID |
| images.min_ram | Body | Integer | 이미지 최소 메모리 요구량(MB)<br>`min_disk`값보다 큰 인스턴스에서만 사용할 수 있음 |
| images.checksum | Body | String | 이미지 내용 해시값<br>내부적으로 이미지 유효성 검증을 위해 사용 |
| images.owner | Body | String | 이미지가 속한 테넌트 ID |
| images.visibility | Body | Enum | 이미지 가시성<br>`public`, `private`, `shared` 중 하나 |
| images.virtual_size | Body | Integer | 이미지 가상 크기 |
| images.size | Body | Integer | 이미지 실제 크기(바이트) |
| images.properties | Body | Object | 이미지 속성 객체<br>이미지별 사용자 지정 속성을 키-값 쌍 형태로 기술 |
| images.self | Body | URI | 이미지 경로 |
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
  ],
  "schema": "/v2/schemas/images",
  "first": "/v2/images",
  "next": "/v2/images?marker=057f9a69-4e4c-4025-8a69-fa248cd9db94"
}
```

</p>
</details>

---

### 이미지 보기

```
GET /v2/images/{imageId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| imageId | URL | UUID | O | 조회할 이미지 ID |
| tokenId | Header | String | O | 토큰 ID|

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| image.status | Body | String | 이미지 상태 |
| image.name | Body | String | 이미지 이름 |
| image.tag | Body | String | 이미지 태그<br>`_AVAILABLE_` 태그를 삭제하면 콘솔에서는 조회되지 않으므로, 태그를 삭제하지 않도록 주의 |
| image.container_format | Body | String | 이미지 컨테이너 포맷 |
| image.created_at | Body | Datetime | 생성 시각 |
| image.disk_format | Body | String | 이미지 디스크 포맷 |
| image.updated_at | Body | Datetime | 수정 시각 |
| image.min_disk | Body | Integer | 이미지 최소 디스크 요구량(GB)<br>`min_disk`값보다 큰 볼륨에서만 사용할 수 있음 |
| image.protected | Body | boolean | 이미지 보호 여부<br>`protected=true`인 경우 수정 및 삭제가 불가 |
| image.id | Body | UUID | 이미지 ID |
| image.min_ram | Body | Integer | 이미지 최소 메모리 요구량(MB)<br>`min_disk`값보다 큰 인스턴스에서만 사용할 수 있음 |
| image.checksum | Body | String | 이미지 내용의 해시값<br>내부적으로 이미지 유효성 검증을 위해 사용 |
| image.owner | Body | String | 이미지가 속한 테넌트 ID |
| image.visibility | Body | Enum | 이미지 가시성<br>`public`, `private`, `shared` 중 하나 |
| image.virtual_size | Body | Integer | 이미지 가상 크기 |
| image.size | Body | Integer | 이미지 실제 크기(바이트) |
| image.properties | Body | Object | 이미지 속성 객체<br>이미지별 사용자 지정 속성을 키-값 쌍 형태로 기술 |
| image.self | Body | URI | 이미지 경로 |
| image.file | Body | String | 이미지 파일 경로 |
| image.schema | Body | URI| 이미지 스키마 경로 |

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

가시성이 `public`인 이미지는 삭제할 수 없습니다.

```
DELETE /v2/images/{imageId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| imageId | URL | String | O | 삭제할 이미지 ID |
| tokenId | Header | String | O | 토큰 ID |

#### 응답
이 API는 응답 본문을 반환하지 않습니다.

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
| imageId | URL | UUID | O | 태그를 추가할 이미지 ID |
| tag | URL | String | O | 추가할 태그 이름(영문 기준 최대 255자)<br><font color='red'>**(주의) `_`로 시작하는 태그는 사용할 수 없습니다**</font> |
| tokenId | Header | String | O | 토큰 ID |

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
| imageId | URL | UUID | O | 태그를 제거할 이미지 ID |
| tag | URL | String | O | 제거할 태그 이름 |
| tokenId | Header | String | O | 토큰 ID |

#### 응답
이 API는 응답 본문을 반환하지 않습니다.

