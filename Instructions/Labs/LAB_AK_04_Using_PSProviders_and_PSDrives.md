---
lab:
  title: 'ラボ: PowerShell での PSProviders と PSDrives の使用'
  type: Answer Key
  module: 'Module 4: Using PSProviders and PSDrives'
ms.openlocfilehash: 817aa59897b79eb49b59a8b7c35817d7231ee410
ms.sourcegitcommit: a95a9bb3a7919b785df0574c3407f4b6c3bea9f5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2021
ms.locfileid: "132116711"
---
# <a name="lab-answer-key-using-psproviders-and-psdrives-with-powershell"></a>ラボの回答キー: PowerShell での PSProviders と PSDrives の使用

## <a name="exercise-1-creating-files-and-folders-on-a-remote-computer"></a>演習 1: リモート コンピューターでのファイルとフォルダーの作成

### <a name="task-1-create-a-new-folder-on-a-remote-computer"></a>タスク 1: リモート コンピューターに新しいフォルダーを作成する

1. **LON-CL1** で、 **[スタート]** を選択して、「**powersh**」と入力します。
1. 結果リストで、 **[Windows PowerShell]** を右クリックするか、そのコンテキスト メニューをアクティブにしてから、 **[管理者として実行]** を選択します。
1. **New-Item** コマンドレットのヘルプを別のウィンドウで確認するには、**Administrator: Windows PowerShell** コンソールで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-Help New-Item –ShowWindow
   ```

1. **Get-Help** の出力で、 *–Name* と *–ItemType* パラメーターを確認してから、サンプル コマンドを確認します。
1. * *\\\Lon-Svr1\C$\** に新しい **ScriptShare** フォルダーを作成するには、コンソールで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   New-Item –Path \\Lon-Svr1\C$\ –Name ScriptShare –ItemType Directory
   ```

### <a name="task-2-create-a-new-psdrive-mapping-to-the-remote-file-folder"></a>タスク 2: リモート ファイル フォルダーへの新しい PSDrive マッピングを作成する

1. **New-PSDrive** コマンドレットにヘルプを表示するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-Help New-PSDrive –ShowWindow
   ```

1. 次の情報を確認してから、ヘルプ ウィンドウを閉じます。
    - ヘルプ情報
    - *–Name*、 *–Root*、および *–PSProvider* パラメーター
    - コマンドの例

1. 新しい PSDrive マッピングを作成するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   New-PSDrive –Name ScriptShare –Root \\Lon-Svr1\c$\ScriptShare –PSProvider FileSystem
   ```

### <a name="task-3-create-a-file-on-the-mapped-drive"></a>タスク 3: マップされたドライブにファイルを作成する

1. **Set-Location** コマンドレットのヘルプを確認するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-Help Set-Location –ShowWindow
   ```

1. ヘルプ情報を確認してから、ヘルプ ウィンドウを閉じます。
1. ScriptShare: の場所を変更するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Set-Location ScriptShare:
   ```

1. 新しいファイルを作成するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   New-Item script.txt
   ```

1. ディレクトリの一覧を確認するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-ChildItem
   ```

1. **script.txt** ファイルが一覧に表示されていることを確認します。

## <a name="exercise-2-creating-a-registry-key-for-your-future-scripts"></a>演習 2: 今後のスクリプトのためのレジストリ キーの作成

### <a name="task-1-create-the-registry-key-to-store-script-configurations"></a>タスク 1: スクリプト構成を格納するレジストリ キーを作成する

1. **Software** レジストリ キーの内容を確認するには、**Windows PowerShell** コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-ChildItem -Path HKCU:\Software
   ```

1. 次のコマンドを入力して、Enter キーを押します。

   ```powershell
   New-Item –Path HKCU:\Software –Name Scripts
   ```

### <a name="task-2-create-a-new-registry-setting-to-store-the-name-of-the-psdrive"></a>タスク 2: PSDrive の名前を格納する新しいレジストリ設定を作成する

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

## <a name="exercise-3-creating-a-new-active-directory-group"></a>演習 3: 新しい Active Directory グループの作成

### <a name="task-1-create-a-psdrive-that-maps-to-the-users-container-in-ad-ds"></a>タスク 1: AD DS の Users コンテナーにマップする PSDrive を作成する

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

### <a name="task-2-create-the-london-developers-group"></a>タスク 2: ロンドンの開発者グループを作成する

1. ロンドンの開発者グループを作成するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   New-Item -ItemType group -Path . -Name "CN=London Developers"
   ```

1. 現在のドライブの項目を一覧表示するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-ChildItem
   ```

1. **London Developers** グループが一覧表示されていることを確認します。
