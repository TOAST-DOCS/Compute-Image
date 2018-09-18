## Compute > Image > コンソール使用ガイド

## イメージ作成

イメージはインスタンスの基本ディスクから作成できます。データ整合性を保障するために、インスタンスを終了した状態でのみイメージを作成できます。終了状態のインスタンスがない場合はイメージを作成できません。

Windowsインスタンスのイメージを作成するには、Sysprepを利用してイメージ作成を準備した後、インスタンスを終了する必要があります。詳細なsysprep使用方法は[Windows Sysprepガイド](#windows-sysprep)を参照してください。

Linuxインスタンスイメージを作成するには、インスタンス内で終了するか、TOASTコンソールで終了してイメージを作成します。

## イメージ修正

イメージの名前を修正します。

**Protected** 属性を選択してイメージをアップデートすると、誤ってイメージを削除することを防げます。Protected属性が選択されたイメージを削除するには、イメージ修正から**Protected** 属性を選択解除してイメージをアップデートする必要があります。

## 他のプロジェクトに共有

イメージを共有するプロジェクトを選択して共有します。


## Windows Sysprepガイド

Windowsイメージを作成するには、ハードウェアとユーザーに従属された情報を削除して、インスタンス作成に使用できるようにイメージ初期化作業が必要です。イメージ初期化はMicrosoftからWindows OSを配布するために提供するシステム準備ユーティリティであるSysprepで実行できます。

まずWindowsインスタンスに接続した後、 **アプリ**で**コマンドプロンプト**を右クリックして**管理者権限で実行**をクリックします。
![[図1コマンドプロンプト実行]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/001_170524_800px.PNG)

コマンドプロンプトが起動したら、下記コマンドを実行します。

	cd C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf
	C:\Windows\System32\Sysprep\sysprep.exe /generalize /oobe /shutdown /unattend:Unattend.xml

![[図2 Sysprep実行]](http://static.toastoven.net/prod_infrastructure/compute/sysprep/002_170524_800px.PNG)

Sysprepが実行されるとWindowsインスタンスは自動的に終了します。TOASTコンソールでWindowsインスタンスの終了を確認して、 [イメージ作成](./console-guide/#_1)機能でユーザーWindowsイメージを作成します。

### Sysprepオプション詳細情報


`\generalize`

Windowsに登録された固有システム情報を削除します。この段階からSID(Security ID)が再設定され、システム復元ポイントが全て消え、イベントログが削除されます。この段階を経た後に再起動すると`Windows構成段階`が現れます。
> [参考]
この段階でSID 、ユーザー情報、固有情報を削除して、既存プログラムの動作に影響を与えることができます。


`\oobe`
Windowsを再起動して開始モードに進入した後、ユーザーがあらかじめ指定した設定を適用します。この時適用できる代表的な設定には、国別設定、ネットワーク位置、言語設定、時間帯などがあります。

`\shutdown`
Windowsを終了します。

`\unattend`
Windowsを再インストールした後、前段階で記録したユーザー設定を復元します。この段階ではWindowsユーザー情報を登録し、ドライバーや製品アップデート、追加ソフトウェアをインストールします。また基本的に提供する設定以外にユーザーが希望する設定を応答ファイルから指定することができます。

> [参考]
TOASTで提供するWindowsイメージの応答ファイルはC:\Program Files\Cloud Solutions\Cloudbase-Init\conf\Unattend.xmlにあります。必要な設定は全て準備されているので、特別な用途を除けば修正しなくてもよいです。
