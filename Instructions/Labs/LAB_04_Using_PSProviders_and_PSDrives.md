---
lab:
  title: 'ラボ: PowerShell での PSProvider と PSDrive の使用'
  module: 'Module 4: Using PSProviders and PSDrives'
---

# ラボ: PowerShell での PSProvider と PSDrive の使用

## シナリオ

あなたは Adatum Corporation のロンドン ブランチ オフィスのシステム管理者です。 環境内のいくつかの設定を再構成する必要があります。 あなたは最近、PSProviders と PSDrives について学習し、それらを使用してデータ ストアにアクセスできるようになりました。 あなたは PSDrives を使用して、これらの設定を再構成することにしました。

## 目標

このラボを完了すると、次のことができるようになります。

- PSDrive を使用してファイルとフォルダーを作成する。
- レジストリ キーと値を作成する。
- PSDrive を使用して Active Directory オブジェクトを作成して表示する。

## 予想所要時間: 30 分

## ラボのセットアップ

仮想マシン:

- **LON-DC1**
- **LON-SVR1**
- **LON-CL1**

ユーザー名: **Adatum\\Administrator**

パスワード: **Pa55w.rd**

このラボでは、提供されている仮想マシン環境を使用します。 ラボを開始する前に、次の手順を行ってください。

1. **LON-DC1** を開いて、**Adatum\\Administrator** として、パスワード **Pa55w.rd** を使用してサインインします。
1. **LON-SVR1** と **LON-CL1** に対して手順 1 を繰り返し行います。

## 演習 1:リモート コンピューターでのファイルとフォルダーの作成

### 演習のシナリオ 1

後で自動化スクリプトをコンピューターにコピーできるように、リモート コンピューターにフォルダーとファイルを作成できることを確認する必要があります。 PSProviders と PSDrives に関連付けられているコマンドレットを使用できるように、**MkDir** コマンドレットまたはそのエイリアスを使用しないことを決定しました。

この演習の主なタスクは次のとおりです。

1. リモート コンピューターに新しいフォルダーを作成する。
1. リモート ファイル フォルダーへの新しい PSDrive マッピングを作成する。
1. マップされたドライブにファイルを作成する。

### タスク 1: リモート コンピューターに新しいフォルダーを作成する

1. **LON-CL1** で、管理者として Windows PowerShell を開きます。
1. **New-Item** コマンドレットの完全なヘルプを確認します。 *–Name* と *–ItemType* パラメーターを確認してから、サンプル コマンドを確認します。
1. **New-Item** を使用して、**\\\LON-SVR1\C$** に **ScriptShare** という名前の新しいフォルダー (ディレクトリ) を作成します。

### タスク 2: リモート ファイル フォルダーへの新しい PSDrive マッピングを作成する

1. Windows PowerShell コンソールで、**New-PSDrive** コマンドの完全なヘルプを確認します。
1. **\\\LON-SVR1\C$\ScriptShare** にマップされる **ScriptShare** という名前の新しい PSDrive を作成します。

### タスク 3: マップされたドライブにファイルを作成する

1. Windows PowerShell コンソールで、**Set-Location** コマンドレットの完全なヘルプを確認します。
1. 現在の作業フォルダーの場所を、マップされたドライブの **ScriptShare** に設定します。
1. **ScriptShare** ドライブで、**New-Item** コマンドレットを使用して、**script.txt** という名前のテキスト ファイルを作成します。
1. **ScriptShare** ドライブ内の項目を一覧表示し、それに **script.txt** ファイルが含まれていることを確認します。

## 演習 2:後で使用するスクリプトのためのレジストリ キーの作成

### シナリオ 2

この演習では、将来開発するスクリプトの構成データを格納するための新しいレジストリ キーを作成します。 また、このキーにレジストリ値を作成して、使用するスクリプトの PSDrive の名前を保存します。 後で作成するスクリプトでレジストリから値を取得できることを確認します。

この演習の主なタスクは次のとおりです。

1. スクリプト構成を格納するレジストリ キーを作成する。
1. PSDrive の名前を格納する新しいレジストリ値を作成する。

### タスク 1: スクリプト構成を格納するレジストリ キーを作成する

1. **Windows PowerShell** コンソールで、コマンドを入力して、レジストリ キー **HKEY_CURRENT_USER\Software** に **Scripts** という名前のサブキーがないことを確認します。
1. コンソールで、コマンドを実行して、**HKEY_CURRENT_USER\Software** に **Scripts** という名前のレジストリ キーを作成します。

### タスク 2: PSDrive の名前を格納する新しいレジストリ値を作成する

1. **Windows PowerShell** コンソールで、コマンドを実行して、現在の作業場所を、作成したレジストリ キーのパスに設定します。
1. 次の構成で PSDrive 名を格納するレジストリ値を作成します。

   - 名前: **PSDriveName**
   - 値: **ScriptShare**

1. **HKey_Current_User\Software\Scripts** キーから **PSDriveName** 設定を取得できることを確認します。

## 演習 3:新しい Active Directory グループの作成

Active Directory のユーザー組織単位にマップされている PSDrive を使用して、すべての AD DS オブジェクトの種類を管理する機能が必要です。 この PSDrive では、ActiveDirectory プロバイダーを使用します。 新しい PSDrive がマップされたら、ロンドン オフィスの開発者にアクセス許可を割り当てるために使用できる、新しいロンドンの開発者グループを作成します。

この演習の主なタスクは次のとおりです。

1. AD DS の Users コンテナーにマップする PSDrive を作成する。
1. ロンドンの開発者グループを作成する。

### タスク 1: AD DS の Users コンテナーにマップする PSDrive を作成する

1. **Windows PowerShell** コンソールで、**ActiveDirectory** モジュールを読み込みます。
1. **New-PSDrive** コマンドレットを使用して、次の設定で新しい PSDrive を作成します。

   - 名前: **AdatumUsers**
   - ルート: **CN=Users,DC=Adatum,DC=com**
   - PSProvider: **ActiveDirectory**

1. 現在の作業場所を新しい PSDrive に設定します。

### タスク 2: ロンドンの開発者グループを作成する

1. **Windows PowerShell** コンソールで、**New-Item** コマンドレットを使用して **AdatumUsers** PSDrive に **London Developers** という名前のグループを作成します。
1. 新しいグループが作成されたことを確認するには、**Get-ChildItem** コマンドレットを使用します。
