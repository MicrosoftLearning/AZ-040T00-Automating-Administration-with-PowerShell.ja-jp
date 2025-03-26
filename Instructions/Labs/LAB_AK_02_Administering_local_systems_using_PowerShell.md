---
lab:
  title: 'ラボ: PowerShell を使用したローカル システム管理の実行'
  type: Answer Key
  module: 'Module 2: Windows PowerShell for local systems administration'
---

# PowerShell を使用したローカル システム管理の実行

## 演習 1:Active Directory オブジェクトの作成と管理

### タスク 1: ブランチ オフィス用の新しい組織単位 (OU) を作成する

1. **LON-CL1** で、**[開始]** を選択します。

1. 「**powershell**」と入力して、Windows PowerShell アイコンを表示します。 アイコン名が **Windows PowerShell (x86)** ではなく **Windows PowerShell** と表示されていることを確認します。

1. **[Windows PowerShell]** を右クリックするか、そのコンテキスト メニューをアクティブにしてから、**[管理者として実行]** を選びます。

1. 
          **[管理者:Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   
   New-ADOrganizationalUnit -Name London
   ```

### タスク 2: ブランチ オフィスの管理者のグループを作成する

- Windows PowerShell コンソールで、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   New-ADGroup "London Admins" -GroupScope Global
   ```

### タスク 3: ブランチ オフィスのユーザーとコンピューター アカウントを作成する

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   New-ADUser -Name Ty -DisplayName "Ty Carlson" 
   ```

1. 以下のコマンドを入力し、Enter キーを押します。

   ```powershell
   Add-ADGroupMember "London Admins" -Members Ty
   ```

1. 以下のコマンドを入力し、Enter キーを押します。

   ```powershell
   New-ADComputer LON-CL2
   ```

### タスク 4: グループ、ユーザー、コンピューター アカウントをブランチ オフィス OU に移動する

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Move-ADObject -Identity "CN=London Admins,CN=Users,DC=Adatum,DC=com" -TargetPath "OU=London,DC=Adatum,DC=com"
   ```

2. 以下のコマンドを入力し、Enter キーを押します。

   ```powershell
   Move-ADObject -Identity "CN=Ty,CN=Users,DC=Adatum,DC=com" -TargetPath "OU=London,DC=Adatum,DC=com"
   ```

3. 以下のコマンドを入力し、Enter キーを押します。

   ```powershell
   Move-ADObject -Identity "CN=LON-CL2,CN=Computers,DC=Adatum,DC=com" -TargetPath "OU=London,DC=Adatum,DC=com"
   ```

### 演習 1 の結果

この演習を完了すると、Windows PowerShell コマンドライン インターフェイスで Active Directory オブジェクトを管理するためのコマンドを正しく識別して使用できるようになります。

## 演習 2:Windows Server でのネットワーク設定の構成

### タスク 1: ネットワーク接続をテストし、構成を確認する

1. **LON-SVR1** に切り替えます。
1. **[スタート]** ボタンを右クリックするか、そのコンテキスト メニューをアクティブにし、**[Windows PowerShell (Admin)]** を選択します。
1. 
          **[管理者:Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Test-Connection LON-DC1
   ```

> **注:** テスト結果は、顕著な遅延なしに返される必要があります。 この演習で、この応答時間と、ネットワーク構成を変更した後のそれを比較します。

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-NetIPConfiguration
   ```

> **注:****LON-SVR1** の IP アドレス、既定のゲートウェイ、ドメイン ネーム システム (DNS) サーバーをメモします。

### タスク 2: サーバーの IP アドレスを変更する

1. 
          **[管理者:Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   New-NetIPAddress -InterfaceAlias Ethernet -IPAddress 172.16.0.15 -PrefixLength 16
   ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Remove-NetIPAddress -InterfaceAlias Ethernet -IPAddress 172.16.0.11
   ```

1. 両方の確認プロンプトで、「**Y**」を入力して Enter キーを押します。

### タスク 3: サーバーの DNS 設定と既定のゲートウェイを変更する

1. 
          **[管理者:Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Set-DnsClientServerAddress -InterfaceAlias Ethernet -ServerAddress 172.16.0.12
   ```

2. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Remove-NetRoute -InterfaceAlias Ethernet -DestinationPrefix 0.0.0.0/0 -Confirm:$false
   ```

3. コンソール ウィンドウで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   New-NetRoute -InterfaceAlias Ethernet -DestinationPrefix 0.0.0.0/0 -NextHop 172.16.0.2
   ```

### タスク 4: 変更を確認してテストする

1. **LON-SVR1** の **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-NetIPConfiguration
   ```

> **注:** IP アドレス、既定のゲートウェイ、DNS サーバーをメモします。

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Test-Connection LON-DC1
   ```

> **注:** 変更後は、**LON-DC1** からの応答を受信するまでに時間がかかるはずです。 実際の遅延は異なる場合があります。 変更は気が付く程度のものであるはずですが、違いに気付かない場合もあります。

### 演習 2 の結果

この演習を完了すると、ネットワーク構成を管理するための Windows PowerShell コマンドを正しく識別して使用できます。

## 演習 3:Web サイトを作成する

### タスク 1: サーバーに Web サーバー (IIS) の役割をインストールする

1. **LON-SVR1** の **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Install-WindowsFeature Web-Server
   ```

> **注:** インターネット インフォメーション サービス (IIS) がインストールされるのを待ちます。 これには 2 分ほどかかります。

### タスク 2: Web サイトのファイル用のフォルダーをサーバーに作成する

1. **LON-SVR1** の **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   New-Item C:\inetpub\wwwroot\London -Type directory
   ```

### タスク 3: IIS Web サイトを作成する

1. **LON-SVR1** の **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   New-IISSite London -PhysicalPath C:\inetpub\wwwroot\London -BindingInformation "172.16.0.15:8080:"
   ```

2. タスクバーで、**[Internet Explorer]** のアイコンを選択します。

3. アドレス バーに、「`http://172.16.0.15:8080`」と入力して、Enter キーを押します。

> **注:** Internet Explorer により、このディレクトリの内容を一覧表示しないように Web サーバーが構成されている、というエラー メッセージが表示されます。 エラー メッセージの詳細に、サイトの物理パスが表示されます。これは、**C:\\inetpub\\wwwroot\\London** になります。

### 演習 3 の結果

この演習を完了すると、標準化された Web サーバー構成の一部として使用される Windows PowerShell コマンドを正しく識別して使用できます。
