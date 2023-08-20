## Compute > Image > コンソール使用ガイド

## イメージ作成

イメージは、インスタンスのルートブロックストレージから作成できます。u2タイプのインスタンスを除く、t2、m2、c2、r2、x1タイプのインスタンスでは実行中にもイメージを作成できますが、データの整合性は保障しません。u2タイプのインスタンスでは停止状態の時のみイメージを作成できます。

Linuxインスタンスのイメージを作成する前にmachine-idを初期化して重複を予防することを推奨します。machine-idの初期化方法については[Linux machine-id初期化ガイド](#Linux-machineid)を参照します。

Windowsインスタンスのイメージを作成するには、Sysprepを利用してイメージ作成を準備し、インスタンスを停止することを推奨します。Sysprepの詳細な使用方法は、[Windows Sysprepガイド](#windows-sysprep)を参照してください。

起動中のWindowsインスタンスからイメージを作成する場合、2019. 05.28.配布バージョン以前のイメージで作成したWindowsインスタンスの場合は、正常に動作させるために先行作業が必要です。インスタンスを作成したイメージのWindowsバージョンは、**インスタンス詳細情報**の**イメージ名**で確認できます。詳細は[起動中のWindowsインスタンスからのイメージ作成ガイド](#windows)を参照してください。

## イメージ修正

イメージの名前を修正します。

**Protected** 属性を選択してイメージをアップデートすると、誤ってイメージを削除することを防げます。Protected属性が選択されたイメージを削除するには、イメージ修正から**Protected** 属性を選択解除してイメージをアップデートする必要があります。

## 別のリージョンにコピー

イメージをコピーする対象リージョンを選択し、新しいイメージの名前と説明を入力してコピーします。

## 他のプロジェクトに共有

イメージを共有するプロジェクトを選択して共有します。

Linuxユーザーイメージを作成して、そのイメージでインスタンスを作成する場合、machine-idが重複して予期しない問題が発生する可能性があります。
ユーザーイメージを作成する前にmachine-idを初期化して重複を予防できます。

	$ sudo sh -c "echo -n > /etc/machine-id"
	$ sudo rm /var/lib/dbus/machine-id
	$ sudo ln -s /etc/machine-id /var/lib/dbus/machine-id

## Windows Sysprepガイド

Windowsイメージを作成するには、ハードウェアとユーザーに従属された情報を削除して、インスタンス作成に使用できるようにイメージ初期化作業が必要です。イメージ初期化はMicrosoftからWindows OSを配布するために提供するシステム準備ユーティリティであるSysprepで実行できます。

### 原本インスタンスのイメージバージョンが2020. 08. 18. 以降の場合
Windowsインスタンスに接続した後、**PowerShell**を管理者権限で実行します。
![[図1 Powershell実行]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/win_sysprep1.png)

PowerShellウィンドウが表示されたら下記のコマンドを実行します。

    ToastSysprep

![[図2 ToastSysprep実行]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/win_sysprep2.png)
> [参考]
ToastSysprepはNHN Cloudで提供するSysprepを簡単に使用できるコマンドです。

**Y**キーを押すと作業を進行します。
![[図3 ToastSysprep進行]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/win_sysprep3.png)

Sysprepが実行されるとWindowsインスタンスは自動的に停止します。NHN CloudコンソールでWindowsインスタンスの停止を確認し、[イメージ作成](./console-guide/#_1)機能でユーザーWindowsイメージを作成します。

Sysprepを利用してWindowsインスタンスを初期化すると、パスワードが空白に変更されてログインできません。イメージ作成機能を利用する時、**イメージに作成されるWindowsパスワードを初期化します。**オプションを選択してWindowsインスタンスのパスワードを自動的に初期化することを推奨します。初期化されたパスワードはインスタンス接続情報で確認します。

### 原本インスタンスのイメージバージョンが2020. 08. 18. 以前の場合

まずWindowsインスタンスに接続した後、 **アプリ**で**コマンドプロンプト**を右クリックして**管理者権限で実行**をクリックします。
![[図1コマンドプロンプト実行]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/001_170524_800px.PNG)

コマンドプロンプトが起動したら、下記コマンドを実行します。

	cd C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf
	C:\Windows\System32\Sysprep\sysprep.exe /generalize /oobe /shutdown /unattend:Unattend.xml

![[図2 Sysprep実行]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/002_170524_800px.PNG)

Sysprepが実行されるとWindowsインスタンスは自動的に停止します。NHN CloudコンソールでWindowsインスタンスの停止を確認して、 [イメージ作成](./console-guide/#_1)機能でユーザーWindowsイメージを作成します。

Sysprepを利用してWindowsインスタンスを初期化すると、パスワードが空白に変更されてログインできません。イメージ作成機能を利用する時、**イメージに作成されるWindowsパスワードを初期化します。**オプションを選択してWindowsインスタンスのパスワードを自動的に初期化することを推奨します。初期化されたパスワードはインスタンス接続情報で確認します。

### Sysprepオプション詳細情報


`\generalize`

Windowsに登録された固有システム情報を削除します。この段階からSID(Security ID)がリセットされ、システム復元ポイントが全て消え、イベントログが削除されます。この段階を経た後に再起動すると`Windows構成パス`が現れます。
> [参考]
この段階でSID 、ユーザー情報、固有情報を削除して、既存プログラムの動作に影響が発生します。


`\oobe`
Windowsを再起動して開始モードに進んだ後、ユーザーがあらかじめ指定した設定を適用します。この時適用できる代表的な設定には、国別設定、ネットワーク位置、言語設定、時間帯などがあります。

`\shutdown`
Windowsを終了します。

`\unattend`
Windowsを再インストールした後、前段階で記録したユーザー設定を復元します。この段階ではWindowsユーザー情報を登録し、ドライバーや製品アップデート、追加ソフトウェアをインストールします。また基本的に提供する設定以外にユーザーが希望する設定を応答ファイルから指定することができます。

> [参考]
NHN Cloudで提供するWindowsイメージの応答ファイルはC:\Program Files\Cloud Solutions\Cloudbase-Init\conf\Unattend.xmlにあります。必要な設定は全て準備されているので、特別な用途を除けば修正しなくてもよいです。


## 起動中のWindowsインスタンスからのイメージ作成ガイド

起動中のWindowsインスタンスからイメージを作成する時、原本インスタンスのイメージのバージョンが2019. 05. 28.以前の場合は、下記の先行作業が必要です。
2019. 05. 28.配布バージョン以降のWindowsイメージの場合は、下記のプロセスは不要です。

1. Windowsインスタンスに接続した後、**アプリ**から**Task Scheduler**を実行します。
![[図3 Task Scheudler実行]](http://static.toastoven.net/prod_infrastructure/compute/windows/001_190604.png)

2. **Task Scheduler**右側の**Actions**タブの**Create Task**をクリックします。
![[図4 Create Task開始]](http://static.toastoven.net/prod_infrastructure/compute/windows/002_190604.png)

3. **Create Task**ウィンドウの**General**タブで**Name**に名前を入力し、**Security options**の**Run with highest privileges**を選択した後、**Change User or Group**をクリックします。
![[図5 User変更および優先順位設定]](http://static.toastoven.net/prod_infrastructure/compute/windows/003_190604.png)

4. **Enter the object name to select**下にある入力ボックスに**SYSTEM**を入力し、**OK**ボタンをクリックします。
![[図6 SYSTEMユーザー設定]](http://static.toastoven.net/prod_infrastructure/compute/windows/004_190604.png)

5. **Create Task**ウィンドウの**Triggers**タブで**New**ボタンをクリックし、新しいトリガー(trigger)を作成します。
![[図7 Trigger設定1]](http://static.toastoven.net/prod_infrastructure/compute/windows/005_190604.png)

6. **Begin the task**のトリガーを**At startup**に選択し、**OK**ボタンをクリックしてトリガーの作成を完了します。
![[図8 Trigger設定2]](http://static.toastoven.net/prod_infrastructure/compute/windows/006_190604.png)

7. **Create Task**ウィンドウの**Actions**タブで**New**ボタンをクリックし、新しいアクション(action)を作成します。
![[図9 Action設定1]](http://static.toastoven.net/prod_infrastructure/compute/windows/007_190604.png)

8. **Action作成**ウィンドウで、下記の値を入力します。

```
	Program/script: C:\Windows\System32\ipconfig.exe
	Add arguments(optional): /renew
```

![[図10 Action設定2]](http://static.toastoven.net/prod_infrastructure/compute/windows/008_190604.png)

9. **OK**ボタンをクリックし、**New Action**ウィンドウと**Create Task**ウィンドウを閉じ、スケジューラーの登録を完了します。
