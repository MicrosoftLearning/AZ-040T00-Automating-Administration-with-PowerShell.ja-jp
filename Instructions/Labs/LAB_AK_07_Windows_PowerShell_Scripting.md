---
lab:
  title: 'ラボ: PowerShell でのスクリプトの使用'
  type: Answer Key
  module: 'Module 4: Windows PowerShell scripting'
ms.openlocfilehash: 68da74b22003a243fc1567c2bb07aec4b74d0346
ms.sourcegitcommit: a95a9bb3a7919b785df0574c3407f4b6c3bea9f5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2021
ms.locfileid: "132116744"
---
# <a name="lab-answer-key-using-scripts-with-powershell"></a>ラボの応答キー: PowerShell でのスクリプトの使用

## <a name="exercise-1-signing-a-script"></a>演習 1: スクリプトへの署名

### <a name="task-1-create-a-code-signing-certificate"></a>タスク 1: コード署名証明書を作成する

1. **LON-CL1** で **[スタート]** を選び、「**mmc.exe**」と入力し、**mmc.exe** を選びます。
1. **MMC** コンソールで **[ファイル]** を選び、 **[スナップインの追加と削除]** を選びます。
1. **[スナップインの追加と削除]** ウィンドウで **[証明書]** を選び、 **[追加]** を選びます。
1. **[証明書スナップイン]** ダイアログ ボックスで **[ユーザー アカウント]** を選択し、 **[完了]** をクリックします。
1. **[スナップインの追加と削除]** ウィンドウで **[OK]** を選びます。
1. **MMC** コンソールで **[証明書 - 現在のユーザー]** を展開し、 **[個人用]** を選びます。
1. **[個人用]** を右クリックするか、コンテキスト メニューを起動し、 **[すべてのタスク]** にカーソルを合わせて **[新しい証明書の要求]** を選びます。
1. **[証明書の登録]** ウィザードの **[開始する前に]** ページで **[次へ]** を選びます。
1. **[証明書の登録ポリシーの選択]** ページで、 **[Active Directory の登録ポリシー]** を選び、 **[次へ]** を選びます。
1. **[証明書の要求]** ページで、 **[Adatum Code Signing]** チェックボックスを選び、 **[登録]** を選びます。
1. **[証明書のインストール結果]** ページで **[完了]** を選びます。
1. **MMC** コンソールで **[個人用]** を展開し、 **[証明書]** を選び、新しいコード署名証明書が存在することを確認します。
1. **MMC** コンソールを閉じ、プロンプトで **[いいえ]** を選び、コンソールの設定を保存します。

### <a name="task-2-digitally-sign-a-script"></a>タスク 2: スクリプトにデジタル署名する

1. **[スタート]** ボタンを選び、「**Powersh**」と入力し、**Windows PowerShell** を選びます。
1. Windows PowerShell プロンプトで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-ChildItem Cert:\CurrentUser\My\ -CodeSigningCert
   ```

1. Windows PowerShell プロンプトで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $cert = Get-ChildItem Cert:\CurrentUser\My\ -CodeSigningCert
   ```

1. Windows PowerShell プロンプトで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Set-Location E:\Mod07\Labfiles
   ```

1. Windows PowerShell プロンプトで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Rename-Item HelloWorld.txt HelloWorld.ps1
   ```

1. Windows PowerShell プロンプトで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Set-AuthenticodeSignature -FilePath HelloWorld.ps1 -Certificate $cert
   ```

### <a name="task-3-set-the-execution-policy"></a>タスク 3: 実行ポリシーを設定する

1. Windows PowerShell プロンプトで次のコマンドを入力し、Enter キーを押します。 プロンプトで「**Y**」と入力し、Enter キーを押します。

   ```powershell
   Set-ExecutionPolicy AllSigned
   ```

2. Windows PowerShell プロンプトで次のコマンドを入力し、Enter キーを押します。 信頼されていない発行元のソフトウェアを実行するかどうかを尋ねられることがあります。 「**A**」と入力し、Enter キーを押します。

   ```powershell
   .\HelloWorld.ps1
   ```

3. Windows PowerShell プロンプトで次のコマンドを入力し、Enter キーを押します。 プロンプトで「**Y**」と入力し、Enter キーを押します。

   ```powershell
   Set-ExecutionPolicy Unrestricted
   ```

4. Windows PowerShell プロンプトを閉じます。

## <a name="exercise-2-processing-an-array-with-a-foreach-loop"></a>演習 2: ForEach ループを使用する配列の処理

### <a name="task-1-create-a-test-group"></a>タスク 1: テスト グループを作成する

1. **LON-CL1** で **[スタート]** を選び、「**Powersh**」と入力し、**Windows PowerShell** を選びます。

1. Windows PowerShell プロンプトで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   New-ADGroup -Name IPPhoneTest -GroupScope Universal -GroupCategory Security
   ```

1. Windows PowerShell プロンプトで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Move-ADObject "CN=IPPhoneTest,CN=Users,DC=Adatum,DC=com" -TargetPath "OU=IT,DC=Adatum,DC=com"
   ```

1. Windows PowerShell プロンプトで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Add-ADGroupMember IPPhoneTest -Members Abbi,Ida,Parsa,Tonia
   ```

### <a name="task-2-create-a-script-to-configure-the-ipphone-attribute"></a>タスク 2: ipPhone 属性を構成するスクリプトを作成する

- このタスクを実行するスクリプトは、**E:\\Mod07\\Labfiles\\AZ-040_Mod07_Ex2_LAK.txt** にあります。

## <a name="exercise-3-processing-items-by-using-if-statements"></a>演習 3: If ステートメントを使用した項目の処理

### <a name="task-1-create-servicestxt-with-service-names"></a>タスク 1: サービス名を含む services.txt を作成する

1. **[スタート]** を選び、「**powersh**」と入力し、**Windows PowerShell** を選びます。
1. Windows PowerShell プロンプトで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Set-Location E:\Mod07\Labfiles
   ```

1. Windows PowerShell プロンプトで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   New-Item services.txt -ItemType File
   ```

1. Windows PowerShell プロンプトで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-Service "Print Spooler" | Select -ExpandProperty Name | Out-File services.txt -Append
   ```

1. Windows PowerShell プロンプトで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-Service "Windows Time" | Select -ExpandProperty Name | Out-File services.txt -Append
   ```

### <a name="task-2-create-a-script-that-starts-stopped-services"></a>タスク 2: 停止されたサービスを開始するスクリプトを作成する

- このタスクを実行するスクリプトは、**E:\\Mod07\\Labfiles\\AZ-040_Mod07_Ex3_LAK.txt** にあります。

## <a name="exercise-4-creating-users-based-on-a-csv-file"></a>演習 4: CSV ファイルに基づくユーザーの作成

### <a name="task-1-create-ad-ds-users-from-a-csv-file"></a>タスク 1: CSV ファイルから AD DS ユーザーを作成する

- このタスクを実行するスクリプトは、**E:\\Mod07\\Labfiles\\AZ-040_Mod07_Ex4_LAK.txt** にあります。

## <a name="exercise-5-querying-disk-information-from-remote-computers"></a>演習 5: リモート コンピューターからのディスク情報のクエリの実行

### <a name="task-1-create-a-script-that-queries-disk-information-with-current-credentials"></a>タスク 1: 現在の資格情報でディスク情報のクエリを実行するスクリプトを作成する

1. **LON-CL1** を使ってすべての手順を実行します。
1. このタスクを実行するスクリプトは、**E:\\Mod07\\Labfiles\\AZ-040_Mod07_Ex5_LAK.txt** にあります。

## <a name="exercise-6-updating-the-script-to-use-alternate-credentials"></a>演習 6: 別の資格情報を使うスクリプトの更新

### <a name="task-1-update-the-script-to-use-alternate-credentials"></a>タスク 1: 代替の資格情報を使うようにスクリプトを更新する

- このタスクを実行するスクリプトは、**E:\\Mod07\\Labfiles\\AZ-040_Mod07_Ex6_LAK.txt** にあります。
