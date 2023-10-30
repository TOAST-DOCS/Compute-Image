## Compute > Image > API v2ガイド

APIを使用するには、APIエンドポイントとトークンなどが必要です。 [API使用準備](/Compute/Compute/ko/identity-api/)を参照してAPIを使用するために必要な情報を準備します。

イメージAPIは、`image`タイプエンドポイントを利用します。正確なエンドポイントはトークン発行レスポンスの`serviceCatalog`を参照します。

| タイプ | リージョン | エンドポイント |
|---|---|---|
| image | 韓国(パンギョ)リージョン<br>韓国(ピョンチョン)リージョン<br>日本リージョン | https://kr1-api-image-infrastructure.nhncloudservice.com<br>https://kr2-api-image-infrastructure.nhncloudservice.com<br>https://jp1-api-image-infrastructure.nhncloudservice.com |

APIレスポンスにガイドに明示されていないフィールドが表示される場合があります。それらのフィールドは、NHN Cloud内部用途で使用され、事前に告知せずに変更する場合があるため使用しないでください。

## イメージ
### イメージリスト照会

```
GET /v2/images
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明                                                                                                                                                             |
|---|---|---|---|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tokenId | Header | String | O | トークンID                                                                                                                                                         |
| limit | Query | Integer | - | 返すイメージの個数。(基本値は25)                                                                                                                                             |
| marker | Query | UUID | - | 照会するイメージリストの最初のイメージID<br>ソート方式に従って`marker`に指定されたイメージから`limit`分のイメージリストを照会                                                                                      |
| name | Query | String | - | 照会するイメージ名                                                                                                                                                      |
| visibility | Query | Enum | - | 照会するイメージの表示プロパティ<br>`public`, `private`、`shared`の中から1つの値のみ選択可能<br>省略するとすべての種類のイメージリストを返す                                                                       |
| owner | Query | String  | - | 照会するイメージが属しているテナントID                                                                                                                                           |
| status | Query | Enum    | - | 照会するイメージの状態<br>`queued`：イメージをコンバーティング中<br>`saving`：イメージをアップロード中<br>`active`：正常<br>`killed`：システムによってイメージ削除<br>`deleted`：削除されたイメージ<br>`pending_delete`：イメージ削除待機中 |
| size_min | Query | Integer | - | 照会するイメージの最小サイズ(Byte)                                                                                                                                           |
| size_max | Query | Integer | - | 照会するイメージの最大サイズ(Byte)                                                                                                                                           |
| nhncloud_product | Query | Enum | - | 照会するイメージのインフラサービスの種類<br>`compute`: Instanceサービスイメージ<br>`gpu`: GPU Instanceサービスイメージ                                                                             |
| sort_key | Query | String | - | イメージリストをソートする時に使用するプロパティ<br>イメージのすべてのプロパティを指定可能。基本値は`created_at`                                                                                               |
| sort_dir | Query | Enum | - | イメージリストのソート方向<br>`asc` (昇順)、`desc` (降順)のうち、1つの値のみ選択可能。基本値は降順                                                                                                   |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| images | Body | Array | イメージリストオブジェクト |
| images.status | Body | String | イメージの状態<br>`queued`、`saving`、`active`、`killed`、`deleted`、`pending_delete`のいずれか1つ。 |
| images.name | Body | String | イメージの名前 |
| images.tags | Body | Array | イメージタグリスト<br>`_AVAILABLE_`タグを削除すると、コンソールでは照会できないため、タグを削除しないでください。 |
| images.container_format | Body | String | イメージコンテナフォーマット |
| images.created_at | Body | Datetime | 作成時刻 |
| images.disk_format | Body | String | イメージディスクフォーマット |
| images.updated_at | Body | Datetime | 修正時刻 |
| images.min_disk | Body | Integer | イメージの最小ディスク要求量(GB)<br>`min_disk`の値より大きいブロックストレージでのみ使用できる。 |
| images.protected | Body | Boolean | イメージの保護有無<br>`protected=true`の場合、修正および削除不可 |
| images.id | Body | UUID | イメージID |
| images.min_ram | Body | Integer | イメージ最小メモリ要求量(MB)<br>`min_disk`の値より大きいインスタンスでのみ使用できる |
| images.checksum | Body | String | イメージ内容ハッシュ値<br>内部的にイメージの有効性を検証するために使用 |
| images.owner | Body | String | イメージが属しているテナントID |
| images.visibility | Body | Enum | イメージの可視性<br>`public`、`private`、`shared`のいずれか1つ。 |
| images.virtual_size | Body | Integer | イメージの仮想サイズ |
| images.size | Body | Integer | イメージの実際のサイズ(Byte) |
| images.properties | Body | Object | イメージプロパティオブジェクト<br>イメージごとにユーザー指定プロパティをキーと値のペアで記述 |
| images.self | Body | URI | イメージのパス |
| images.file | Body | String | イメージファイルのパス |
| images.schema | Body | URI | イメージスキーマのパス |
| schema | Body | URI | イメージリストスキーマのパス |
| first | Body | URI | イメージリストの最初のページに該当するパス |
| next| Body | URI | イメージリストの次のページに該当するパス |

<details><summary>例</summary>
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

### イメージ表示

```
GET /v2/images/{imageId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| imageId | URL | UUID | O | 照会するイメージID |
| tokenId | Header | String | O | トークンID|

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| status | Body | String | イメージの状態 |
| name | Body | String | イメージの名前 |
| tag | Body | String | イメージタグ<br>`_AVAILABLE_`タグを削除すると、コンソールでは照会されないため、タグを削除しないでください。 |
| container_format | Body | String | イメージコンテナフォーマット |
| created_at | Body | Datetime | 作成時刻 |
| disk_format | Body | String | イメージディスクフォーマット |
| updated_at | Body | Datetime | 修正時刻 |
| min_disk | Body | Integer | イメージ最小ディスク要求量(GB)<br>`min_disk`の値より大きいブロックストレージでのみ使用できる |
| protected | Body | boolean | イメージ保護有無<br>`protected=true`の場合、修正および削除不可 |
| id | Body | UUID | イメージID |
| min_ram | Body | Integer | イメージ最小メモリ要求量(MB)<br>`min_disk`の値より大きいインスタンスでのみ使用できる |
| checksum | Body | String | イメージ内容のハッシュ値<br>内部的にイメージの有効性を検証するために使用 |
| owner | Body | String | イメージが属しているテナントID |
| visibility | Body | Enum | イメージの可視性<br>`public`、`private`、`shared`のいずれか1つ。 |
| virtual_size | Body | Integer | イメージの仮想サイズ |
| size | Body | Integer | イメージの実際のサイズ(Byte) |
| properties | Body | Object | イメージプロパティオブジェクト<br>イメージごとにユーザー指定プロパティをキーと値のペアで記述 |
| self | Body | URI | イメージのパス |
| file | Body | String | イメージファイルのパス |
| schema | Body | URI | イメージスキーマのパス |

<details><summary>例</summary>
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

### イメージ作成

```
POST /v2/images
X-Auth-Token: {tokenId}
```

#### リクエスト
| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| container_format | Body | String | - | イメージコンテナフォーマット |
| disk_format | Body | String | - | イメージディスクフォーマット |
| min_disk | Body | Integer | - | イメージ最小ディスク要求量(GB) |
| min_ram | Body | Integer | - | イメージ最小メモリ要求量(MB) |
| protected | Body | Boolean | - | イメージ保護有無、trueまたはfalse |
| tags | Body | Array | - | イメージタグリスト<br>`_AVAILABLE_`タグを削除するとコンソールでは照会できないため、タグを削除しないように注意 |
| visibility | Body | String | - | イメージ可視性<br>`public`、`private`、`shared`のいずれか |

<details><summary>例</summary>
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

#### レスポンス
| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| status | Body | String | イメージ状態<br>`queued`, `saving`, `active`, `killed`, `deleted`, `pending_delete`のいずれか |
| name | Body | String | イメージの名前 |
| tag | Body | String | イメージタグ<br>`_AVAILABLE_`タグを削除すると、コンソールでは照会されないため、タグを削除しないでください。 |
| container_format | Body | String | イメージコンテナフォーマット |
| created_at | Body | Datetime | 作成時刻 |
| disk_format | Body | String | イメージディスクフォーマット |
| updated_at | Body | Datetime | 修正時刻 |
| min_disk | Body | Integer | イメージ最小ディスク要求量(GB)<br>`min_disk`の値より大きいブロックストレージでのみ使用できる |
| protected | Body | boolean | イメージ保護有無<br>`protected=true`の場合、修正および削除不可 |
| id | Body | UUID | イメージID |
| min_ram | Body | Integer | イメージ最小メモリ要求量(MB)<br>`min_disk`の値より大きいインスタンスでのみ使用できる |
| checksum | Body | String | イメージ内容のハッシュ値<br>内部的にイメージの有効性を検証するために使用 |
| owner | Body | String | イメージが属しているテナントID |
| visibility | Body | Enum | イメージの可視性<br>`public`、`private`、`shared`のいずれか1つ。 |
| virtual_size | Body | Integer | イメージの仮想サイズ |
| size | Body | Integer | イメージの実際のサイズ(Byte) |
| properties | Body | Object | イメージプロパティオブジェクト<br>イメージごとにユーザー指定プロパティをキーと値のペアで記述 |
| self | Body | URI | イメージのパス |
| file | Body | String | イメージファイルのパス |
| schema | Body | URI | イメージスキーマのパス |

<details><summary>例</summary>
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

### イメージアップロード

指定したイメージに実際のイメージファイルをアップロードします。

```
PUT /v2/images/{imageId}/file
X-Auth-Token: {tokenId}
Content-Type: application/octet-stream
```

#### リクエスト
リクエスト時、HeaderのContent-Typeをapplication/octet-streamに設定する必要があります。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| imageId | URL | UUID | O | イメージID |
| tokenId | Header | String | O | トークンID |
| -       | Body | Binary | O | アップロードするイメージファイルのバイナリデータ |

#### レスポンス
このAPIはレスポンス本文を返しません。リクエストが正しい場合はステータスコード204を返します。

---

### イメージダウンロード

指定したイメージのバイナリデータをダウンロードします。

```
GET /v2/images/{imageId}/file
X-Auth-Token: {tokenId}
```

#### リクエスト
| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| imageId | URL | UUID | O | イメージID |
| tokenId | Header | String | O | トークンID |

#### レスポンス
イメージのバイナリデータが返されます。リクエストが正しい場合、ステータスコード200を返します。

---

### イメージ修正

イメージ修正によりイメージプロパティを変更できます。

```
PATCH /v2/images/{imageId}
X-Auth-Token: {tokenId}
Content-Type: application/openstack-images-v2.1-json-patch
```

#### リクエスト
リクエスト時、HeaderのContent-Typeをapplication/openstack-images-v2.1-json-patchに設定する必要があります。

| 名前 | 種類 | 形式    | 必須 | 説明                                                                          |
|---|---|--------|----|------------------------------------------------------------------------------|
| imageId | URL | UUID   | O  | 修正するイメージID                                                                   |
| tokenId | Header | String | O  | トークンID                                                                        |
| op | Body | Enum   | O  | 修正する作業タイプ</br>`add`:プロパティの追加</br>`replace`:プロパティ値の修正</br>`remove`:プロパティの削除 |
| path | Body | String | O  | 修正するプロパティ</br>`/{path}`形式                                                     |
| value | Body | String | -  | 修正するプロパティの値                                                                   |

<details><summary>例</summary>
<p>

```json
// プロパティの追加
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

// プロパティ値の修正
[
    {
        "op": "replace",
        "path": "/metadata1",
        "value": "value2"
    }
]

// プロパティの削除
[
    {
        "op": "remove",
        "path": "/metadata1"
    }
]
```

<p>
</details>

#### レスポンス

イメージ表示と同じレスポンスを返します。

---

### イメージ削除

可視性が`public`のイメージは削除できません。

```
DELETE /v2/images/{imageId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| imageId | URL | String | O | 削除するイメージID |
| tokenId | Header | String | O | トークンID |

#### レスポンス
このAPIはレスポンス本文を返しません。


---

## イメージタグ
### タグを追加する
指定したイメージにタグを追加します。

```
PUT /v2/images/{imageId}/tags/{tag}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| imageId | URL | UUID | O | タグを追加するイメージID |
| tag | URL | String | O | 追加するタグ名(英字基準最大255文字)<br><font color='red'>**(注意) `_`で始まるタグは使用できません。**</font> |
| tokenId | Header | String | O | トークンID |

#### レスポンス
このAPIはレスポンス本文を返しません。

---

### タグを削除する
指定したイメージからタグを削除します。


```
DELETE /v2/images/{imageId}/tags/{tag}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| imageId | URL | UUID | O | タグを削除するイメージID |
| tag | URL | String | O | 削除するタグ名 |
| tokenId | Header | String | O | トークンID |

#### レスポンス
このAPIはレスポンス本文を返しません。

---

## イメージ共有

イメージ共有を通して、自分のテナントに属しているイメージを他のテナントに共有できます。次の2段階によりイメージを共有します。

1. イメージの可視性を`shared`に変更
2. 共有を受けるテナントをイメージのメンバーに登録

共有したイメージは共有されたテナントですぐに使用できますが、イメージリスト照会では表示されません。**共有されたテナント**でメンバーの状態を`active`に変更すると、共有されたイメージが照会されます。

### 可視性の変更

```
PATCH /v2/images/{imageId}
X-Auth-Token: {tokenId}
Content-Type: application/openstack-images-v2.1-json-patch
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| imageId | URL | UUID | O | 共有するイメージID |
| tokenId | Header | String | O | トークンID |
| op | Body | String | O | `replace`に指定 |
| path | Body | String | O | `/visibility`に指定 |
| value | Body | String | O | 変更する可視性の値。 `private`または`shared`。 |

<details><summary>例</summary>
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

#### レスポンス

イメージ表示と同じレスポンスを返します。

---

### メンバー追加
共有を受けるテナントを、指定したイメージのメンバーに登録します。

```
POST /v2/images/{imageId}/members
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| imageId | URL | UUID | O | 共有するイメージID |
| tokenId | Header | String | O | トークンID |
| member | Body | String | O | 共有を受けるテナントID |

<details><summary>例</summary>
<p>

```
{
    "member": "8989447062e04a818baf9e073fd04fa7"
}
```

</p>
</details>

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| created_at | Body | Datetime | メンバー作成日時<br>`YYYY-MM-DDThh:mm:ssZ`の形式 |
| image_id | Body | UUID | 共有したイメージID |
| member_id | Body | String | イメージを共有されたテナントID |
| schema | Body | URI | イメージメンバーのスキーマパス |
| status | Body | Enum | イメージメンバーの状態<br>`pending`、`accepted`のいずれか |

<details><summary>例</summary>
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

### メンバーリスト表示
指定したイメージを共有されたテナントリストを照会します。必ず該当イメージが属しているテナントや共有されたテナントのトークンでリクエストします。

```
GET /v2/images/{imageId}/members
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| imageId | URL | UUID | O | イメージID |
| tokenId | Header | String | O | トークンID |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| members | Body | Object | メンバーオブジェクトリスト |
| members.created_at | Body | Datetime | メンバー作成日時`YYYY-MM-DDThh:mm:ssZ`の形式 |
| members.image_id | Body | UUID | 共有したイメージID |
| members.member_id | Body | String | イメージを共有されたテナントID |
| members.schema | Body | URI | イメージメンバースキーマのパス |
| members.status | Body | Enum | イメージメンバーの状態。 `pending`、`accepted`のいずれか。 |
| schema | Body | URI | イメージメンバーリストのスキーマパス |

<details><summary>例</summary>
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

### メンバー詳細表示

指定したイメージの特定メンバーについての詳細情報を返します。必ず該当イメージが属しているテナントや、共有を受けたテナントのトークンでリクエストします。

```
GET /v2/images/{imageId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| imageId | URL | UUID | O | イメージID |
| memberId | URL | String | O | メンバーID |
| tokenId | Header | String | O | トークンID |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| created_at | Body | Datetime | メンバー作成日時`YYYY-MM-DDThh:mm:ssZ`の形式 |
| image_id | Body | UUID | 共有したイメージID |
| member_id | Body | String | イメージを共有されたテナントID |
| schema | Body | URI | イメージメンバースキーマのパス |
| status | Body | Enum | イメージメンバーの状態。 `pending`、`accepted`のいずれか。 |

<details><summary>例</summary>
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

### メンバーの状態変更

共有を受けたテナントで、共有されたイメージを承認します。イメージの共有を承認すると、イメージリスト照会でも該当イメージが照会されます。必ず共有を受けたテナントのトークンでリクエストします。

```
PUT /v2/images/{imageId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| imageId | URL | UUID | O | イメージID |
| memberId | URL | String | O | メンバーID |
| tokenId | Header | String | O | トークンID |
| status  | Body | Enum | O | `accepted`, `pending`、`rejected`のいずれか。 |

<details><summary>例</summary>
<p>

```json
{
    "status": "accepted"
}
```

</p>
</details>

#### レスポンス

| 名前 | 種類 | タイプ | 説明 |
|---|---|---|---|
| created_at | Body | Datetime | メンバー作成日時<br>`YYYY-MM-DDThh:mm:ssZ`形式 |
| image_id | Body | UUID | 共有したイメージID |
| member_id | Body | String | イメージの共有を受けたテナントID |
| schema | Body | URI | イメージメンバースキーマのパス |
| status | Body | Enum | イメージメンバーの状態<br>`accpeted`、`pending`、`rejected`のうち、いずれか1つ。 |
| updated_at | Body | Datetime | メンバー状態の修正日時<br>`YYYY-MM-DDThh:mm:ssZ`形式 |

<details><summary>例</summary>
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

### メンバーの削除

指定したイメージのメンバーを削除します。共有をキャンセルする時に使用します。必ず指定したイメージが属しているテナントのトークンでリクエストする必要があります。

```
DELETE /v2/images/{imageId}/members/{memberId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| imageId | URL | UUID | O | イメージID |
| memberId | URL | String | O | メンバーID |
| tokenId | Header | String | O | トークンID |

#### レスポンス
このAPIはレスポンス本文を返しません。
