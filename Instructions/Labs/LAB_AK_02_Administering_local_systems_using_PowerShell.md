---
lab:
  title: 'ラボ: PowerShell を使用したローカル システム管理の実行'
  type: Answer Key
  module: 'Module 2: Windows PowerShell for local systems administration'
ms.openlocfilehash: 8aa8b8ea7a0d61b94428f9cabf91c945bf259546
ms.sourcegitcommit: a95a9bb3a7919b785df0574c3407f4b6c3bea9f5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2021
ms.locfileid: "132116726"
---
# <a name="lab-performing-local-system-administration-with-powershell"></a>ラボ: PowerShell を使用したローカル システム管理の実行

## <a name="exercise-1-creating-and-managing-active-directory-objects"></a>演習 1: Active Directory オブジェクトの作成と管理

### <a name="task-1-create-a-new-organizational-unit-ou-for-a-branch-office"></a>タスク 1: ブランチ オフィス用の新しい組織単位 (OU) を作成する

1. **LON-CL1** で、 **[開始]** を選択します。

1. 「**powershell**」と入力して、Windows PowerShell アイコンを表示します。 アイコン名が **Windows PowerShell (x86)** ではなく **Windows PowerShell** と表示されていることを確認します。

1. **[Windows PowerShell]** を右クリックするか、そのコンテキスト メニューをアクティブにしてから、 **[管理者として実行]** を選択します。

1. **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   
   New-ADOrganizationalUnit -Name London
   ```

### <a name="task-2-create-group-for-branch-office-administrators"></a>タスク 2: ブランチ オフィスの管理者のグループを作成する

- Windows PowerShell コンソールで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   New-ADGroup "London Admins" -GroupScope Global
   ```

### <a name="task-3-create-a-user-and-computer-account-for-the-branch-office"></a>タスク 3: ブランチ オフィスのユーザーとコンピューター アカウントを作成する

1. コンソールで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   New-ADUser -Name Ty -DisplayName "Ty Carlson" 
   ```

1. 次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Add-ADGroupMember "London Admins" -Members Ty
   ```

1. 次のコマンドを入力して、Enter キーを押します。

   ```powershell
   New-ADComputer LON-CL2
   ```

### <a name="task-4-move-the-group-user-and-computer-accounts-to-the-branch-office-ou"></a>タスク 4: グループ、ユーザー、コンピューター アカウントをブランチ オフィス OU に移動する

1. コンソールで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Move-ADObject -Identity "CN=London Admins,CN=Users,DC=Adatum,DC=com" -TargetPath "OU=London,DC=Adatum,DC=com"
   ```

2. 次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Move-ADObject -Identity "CN=Ty,CN=Users,DC=Adatum,DC=com" -TargetPath "OU=London,DC=Adatum,DC=com"
   ```

3. 次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Move-ADObject -Identity "CN=LON-CL2,CN=Computers,DC=Adatum,DC=com" -TargetPath "OU=London,DC=Adatum,DC=com"
   ```

### <a name="exercise-1-results"></a>演習 1 の結果

この演習を完了すると、Windows PowerShell コマンドライン インターフェイスで Active Directory オブジェクトを管理するためのコマンドを正しく識別して使用できるようになります。

## <a name="exercise-2-configuring-network-settings-on-windows-server"></a>演習 2: Windows Server でのネットワーク設定の構成

### <a name="task-1-test-the-network-connection-and-review-the-configuration"></a>タスク 1: ネットワーク接続をテストし、構成を確認する

1. **LON-SVR1** に切り替えます。
1. **[スタート]** ボタンを右クリックするか、そのコンテキスト メニューをアクティブにし、 **[Windows PowerShell (Admin)]** を選択します。
1. **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Test-Connection LON-DC1
   ```

> **注:** 接続の速度をメモし、変更後の速度と比較できるようにします。

1. コンソールで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-NetIPConfiguration
   ```

> **注:** **LON-SVR1** の IP アドレス、既定のゲートウェイ、ドメイン ネーム システム (DNS) サーバーをメモします。

### <a name="task-2-change-the-server-ip-address"></a>タスク 2: サーバーの IP アドレスを変更する

1. **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   New-NetIPAddress -InterfaceAlias Ethernet -IPAddress 172.16.0.15 -PrefixLength 16
   ```

1. コンソールで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Remove-NetIPAddress -InterfaceAlias Ethernet -IPAddress 172.16.0.11
   ```

1. **「Y」** と入力し、両方の確認プロンプトに対して Enter キーを押します。

### <a name="task-3-change-the-dns-settings-and-default-gateway-for-the-server"></a>タスク 3: サーバーの DNS 設定と既定のゲートウェイを変更する

1. **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Set-DnsClientServerAddress -InterfaceAlias Ethernet -ServerAddress 172.16.0.12
   ```

2. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Remove-NetRoute -InterfaceAlias Ethernet -DestinationPrefix 0.0.0.0/0 -Confirm:$false
   ```

3. コンソール ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   New-NetRoute -InterfaceAlias Ethernet -DestinationPrefix 0.0.0.0/0 -NextHop 172.16.0.2
   ```

### <a name="task-4-verify-and-test-the-changes"></a>タスク 4: 変更を確認してテストする

1. **LON-SVR1** の **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-NetIPConfiguration
   ```

> **注:** IP アドレス、既定のゲートウェイ、DNS サーバーをメモします。

1. コンソールで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Test-Connection LON-DC1
   ```

> **注:** ここでは、**LON-DC1** から応答を受信するのに、今までより時間が長くかかります。 実際の所要時間は異なる場合があります。 変更は気が付く程度のものであるはずですが、違いに気付かない場合もあります。

### <a name="exercise-2-results"></a>演習 2 の結果

この演習を完了すると、ネットワーク構成を管理するための Windows PowerShell コマンドを正しく識別して使用できます。

## <a name="exercise-3-creating-a-website"></a>演習 3: Web サイトの作成

### <a name="task-1-install-the-web-server-iis-role-on-the-server"></a>タスク 1: サーバーに Web サーバー (IIS) の役割をインストールする

1. **LON-SVR1** の **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Install-WindowsFeature Web-Server
   ```

> **注:** インターネット インフォメーション サービス (IIS) がインストールされるのを待ちます。

### <a name="task-2-create-a-folder-on-the-server-for-the-website-files"></a>タスク 2: Web サイトのファイル用のフォルダーをサーバーに作成する

1. **LON-SVR1** の **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   New-Item C:\inetpub\wwwroot\London -Type directory
   ```

### <a name="task-3-create-the-iis-website"></a>タスク 3: IIS Web サイトを作成する

1. **LON-SVR1** の **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   New-IISSite London -PhysicalPath C:\inetpub\wwwroot\london -BindingInformation "172.16.0.15:8080:"
   ```

2. タスクバーで、 **[Internet Explorer]** のアイコンを選択します。

3. [アドレス バー] に「`http://172.16.0.15:8080`」と入力し、Enter キーを押します。

> **注:** Internet Explorer により、このディレクトリの内容を一覧表示しないように Web サーバーが構成されている、というエラー メッセージが表示されます。 エラー メッセージの詳細に、サイトの物理パスが表示されます。これは、**C:\\inetpub\\wwwroot\\london** であるはずです。

### <a name="exercise-3-results"></a>演習 3 の結果

この演習を完了すると、標準化された Web サーバー構成の一部として使用される Windows PowerShell コマンドを正しく識別して使用できます。
