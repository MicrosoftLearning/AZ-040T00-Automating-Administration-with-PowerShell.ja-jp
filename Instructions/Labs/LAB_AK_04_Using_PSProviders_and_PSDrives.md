---
lab:
  title: 'ラボ: PowerShell での PSProvider と PSDrive の使用'
  type: Answer Key
  module: 'Module 4: Using PSProviders and PSDrives'
---

# PowerShell での PSProvider と PSDrive の使用

## 演習 1:リモート コンピューターでのファイルとフォルダーの作成

### タスク 1: リモート コンピューターに新しいフォルダーを作成する

1. **LON-CL1** で、**[スタート]** を選択して、「**powersh**」と入力します。
1. 結果リストで、**Windows PowerShell** を右クリックするか、コンテキスト メニューをアクティブにして、**[管理者として実行]** を選択します。
1. **New-Item** コマンドレットのヘルプを別のウィンドウで確認するには、**管理者: Windows PowerShell** コンソールで、以下のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-Help New-Item –ShowWindow
   ```

1. **Get-Help** の出力で、*–Name* と *–ItemType* パラメーターを確認してから、サンプル コマンドを確認して、**New-Item Help** ウィンドウを閉じます。
1. **\\\\Lon-Svr1\\C$\\** に新しい **ScriptShare** フォルダーを作成するには、コンソールで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   New-Item –Path \\Lon-Svr1\C$\ –Name ScriptShare –ItemType Directory
   ```

### タスク 2: リモート ファイル フォルダーへの新しい PSDrive マッピングを作成する

1. **New-PSDrive** コマンドレットのヘルプを表示するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-Help New-PSDrive –ShowWindow
   ```

1. 次の情報を確認してから、**New-PSDrive Help** ウィンドウを閉じます。
    - ヘルプ情報
    - *–Name*、*–Root*、および *–PSProvider* パラメーター
    - コマンドの例

1. 新しい PSDrive マッピングを作成するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   New-PSDrive –Name ScriptShare –Root \\Lon-Svr1\c$\ScriptShare –PSProvider FileSystem
   ```

### タスク 3: マップされたドライブにファイルを作成する

1. **Set-Location** コマンドレットのヘルプを確認するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-Help Set-Location –ShowWindow
   ```

1. ヘルプ情報を確認してから、**[Set-Location ヘルプ]** ウィンドウを閉じます。
1. ScriptShare: の場所を変更するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Set-Location ScriptShare:
   ```

1. 新しいファイルを作成するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   New-Item script.txt
   ```

1. ディレクトリのリストを確認するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-ChildItem
   ```

1. **script.txt** ファイルが一覧に表示されていることを確認します。

## 演習 2:後で使用するスクリプトのためのレジストリ キーの作成

### タスク 1: スクリプト構成を格納するレジストリ キーを作成する

1. **Software** レジストリ キーの内容を確認するには、**Windows PowerShell** コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-ChildItem -Path HKCU:\Software
   ```

1. 以下のコマンドを入力し、Enter キーを押します。

   ```powershell
   New-Item –Path HKCU:\Software –Name Scripts
   ```

### タスク 2: PSDrive の名前を格納する新しいレジストリ値を作成する

1. 場所を **HKCU:\Software\Scripts** に変更するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Set-Location HKCU:\Software\Scripts
   ```

1. **PSDriveName** レジストリ値を作成するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   New-ItemProperty -Path HKCU:\Software\Scripts -Name "PSDriveName" –Value "ScriptShare"
   ```

1. PSDriveName レジストリ値を確認するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-ItemProperty . -Name PSDriveName
   ```

## 演習 3:新しい Active Directory グループの作成

### タスク 1: AD DS の Users コンテナーにマップする PSDrive を作成する

1. **ActiveDirectory** モジュールを読み込むには、**Windows PowerShell** コンソールで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Import-Module ActiveDirectory
   ```

1. 新しい **AdatumUsers** PSDrive を作成するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   New-PSDrive -Name AdatumUsers -Root "CN=Users,DC=Adatum,DC=com" -PSProvider ActiveDirectory
   ```

1. 場所を **AdatumUsers** ドライブに変更するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Set-Location AdatumUsers:
   ```

### タスク 2: ロンドンの開発者グループを作成する

1. ロンドンの開発者グループを作成するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   New-Item -ItemType group -Path . -Name "CN=London Developers"
   ```

1. 現在のドライブの項目を一覧表示するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-ChildItem
   ```

1. **London Developers** グループが一覧表示されていることを確認します。
