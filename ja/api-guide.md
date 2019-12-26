## Compute > Image > APIガイド

APIは現在、韓国リージョンでのみ使用できます。

イメージAPIではイメージのリストを照会するAPIのみ提供します。イメージ作成APIは[インスタンス追加機能API](/Compute/Instance/ja/api-guide/#api_4)を参照してください。

## 事前準備

イメージAPIを使用するにはアプリケーションキー(Appkey)とトークンが必要です。 [API Endpoint URL](/Compute/Instance/ja/api-guide/#api-endpoint-url)と[トークンAPI](/Compute/Instance/ja/api-guide/#api)を利用してアプリケーションキーとトークンを準備します。アプリケーションキーはAPI Endpoint URLに、トークンはRequest Bodyに含めて使用します。

例えば、イメージリスト照会は次のURLでリクエストする必要があります。

	GET https://api-compute.cloud.toast.com/compute/v1.0/appkeys/{appkey}/images

## イメージ状態
イメージは次の状態値を持ちます。

| 状態 | 説明 |
| -- | -- |
| queued | イメージIDは発行されているが、まだイメージデータがアップロードされていない状態 |
| saving | イメージデータを保存中の状態 |
| active | イメージを使用できる状態 |
| killed | イメージデータアップロード中にエラーが発生した状態 |
| deleted | イメージの情報は残っているが、これ以上利用できない状態 |
| pending_delete | deleted状態と類似、イメージを回復できない状態 |
| deactivated | イメージデータを使用できない状態 |

## イメージAPI

### イメージリスト照会

イメージのリスト、詳細情報を照会します。

#### Method、 URL
```
GET /v1.0/appkeys/{appkey}/images?limit={limit}&marker={markerId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | トークンID |
| limit | Query | Integer | O | 照会するイメージの数 |
| markerId | Query | UUID | O | 照会時に基準になるイメージID<br>イメージリストは作成日時順にソートされます。<br>limitとmakerを指定する場合、markerで指定されたイメージからlimitの数を照会します。 |

#### Request Body
このAPIはRequest Bodyが必要ありません。

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
| Created At | Body | String  | イメージ生成時間。 yyyy-mm-ddTHH:MM:ssZの形式。例) 2017-05-16T02:17:50.166563 |
| Disk Format | Body | String | イメージのディスク形式。 <br />"ami"、 "ari"、 "aki"、 "vhd"、 "vhdx"、 "vmdk"、 "raw"、 "qcow2"、 "vdi"、 "ploop"、 "iso" |
| Image ID | Body | String | イメージID |
| Is Public | Body | Boolean | パブリックイメージ可否 |
| Min Disk | Body | Integer | このイメージで生成できるインスタンスの最小ディスクサイズ (GB) |
| Min RAM | Body | Integer | このイメージで生成できるインスタンスの最小RAMサイズ(MB) |
| Image Name | Body | String | イメージ名前 |
| Prop Key / Prop Value | Body | String | イメージの追加的な属性 |
| Protected | Body | Boolean | 削除保護設定の可否 |
| Image Size | Body | Integer | イメージデータのサイズ(byte) |
| Image Status | Body | String | イメージの状態 |
| Updated At | Body | String | イメージがアップデートされた時間。 yyyy-mm-ddTHH:MM:ssZの形式。例) 2017-05-16T02:17:50.166563 |

### イメージの削除

指定したイメージを削除します。ただし、ユーザーが作成したイメージのみ削除できます。

#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/images?id={imageId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| imageId | Query | UUID | - | イメージID |
| tokenId | Header | String | - | トークンID |

#### Request Body
このAPIはRequest Bodyが不要です。

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
