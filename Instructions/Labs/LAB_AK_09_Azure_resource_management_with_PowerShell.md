---
lab:
  title: 'ラボ: PowerShell を使った Azure リソースの管理'
  type: Answer Key
  module: 'Module 9: Managing Azure resources with PowerShell'
---

# PowerShell を使用した Azure リソース管理

## 演習 1:Azure サブスクリプションのアクティブ化と PowerShell Az モジュールのインストール

#### タスク 1: Azure portal を開く

1. タスク バーで、**Microsoft Edge** アイコンを選択します。

1. ブラウザー ウィンドウで、Azure portal (`https://portal.azure.com`) に移動してから、このラボで使用するアカウントでサインインします。

   > **注**: Azure portal に初めてサインインしている場合は、ポータルのツアーが表示されます。 このツアーをスキップする場合は、 **[後で行う]** を選択してポータルの使用を開始します。

### タスク 2: Azure Az module for PowerShell をインストールする

1. **[スタート]** メニューを選択してから、「**pwsh**」と入力します。

1. 結果リストで、**[PowerShell 7 (x64)]** を右クリックするか、そのコンテキスト メニューをアクティブにして、**[管理者として実行]** を選択します。

1. 
          **[管理者:PowerShell 7 (x64)]** ウィンドウで、次のコマンドを入力し、Enter キーを押して、PowerShell のバージョンを確認します。

   ```powershell
   $PSVersionTable.PSVersion
   ```

1. 次のコマンドを入力し、Enter キーを押して PowerShell 7 を更新します。
      
   ```powershell
   # Update PowerShell 7 to the latest version
   iex "& { $(irm https://aka.ms/install-powershell.ps1 -UseBasicP) }"

   # Open a new PowerShell 7 window as an administrator
   Start-Process -FilePath "C:\Users\Administrator.ADATUM\AppData\Local\Microsoft\powershell\pwsh.exe" -Verb RunAs

   # Close the current PowerShell window
   Stop-Process -Id $PID 
   
   ```  

1. 実行ポリシーを適切な値に設定して Az モジュールをインストールできるようにするには、次のコマンドを入力し、Enter キーを押します。 「**Y**」と入力して、選択した内容を確認します。

   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
   ```

1. Az モジュールをインストールするには、次のコマンドを入力します。 「**Y**」と入力して、選択した内容を確認します。

   ```powershell
   Install-Module -Name Az -Scope CurrentUser -Repository PSGallery -Force
   ```

1. モジュールがインストールされ、コマンド プロンプトが表示されるまで待ちます。

1. Az モジュールがインストールされたら、次のコマンドを入力します。

   ```powershell
   Connect-AzAccount
   ```

1. メッセージが表示されたら、前のタスクで使用したアカウントでサインインし、Azure サブスクリプションをプロビジョニングします。

1. サインイン後、アカウント名と Azure サブスクリプションの詳細が一覧に表示されていることを確認します。

## 演習 2:Azure Cloud Shell の使用

### タスク 1: Azure Cloud Shell を使って、リソース グループを作成する

1. **LON-CL1** のコンピューターで、Azure portal が表示されている Web ブラウザー ウィンドウに切り替えます。

1. Microsoft Azure portal のホームページで、**[仮想マシン]** を選択します。 仮想マシン (VM) が作成されていないことを確認します。 **[ホーム]** を選択します。

1. Microsoft Azure portal のホームページで、**[ストレージ アカウント]** を選択します。 ストレージ アカウントが作成されていないことを確認します。 **[ホーム]** を選択します。

1. Microsoft Azure portal のホームページで、**[Cloud Shell]** アイコンを選択します。

1. **[Azure Cloud Shell へようこそ]** ウィンドウで **[PowerShell]** を選択します。

1. **[ストレージがマウントされていません]** ページで、Cloud Shell を実行するために必要なストレージ アカウントがないことに関する注記を確認します。 **"サブスクリプション"** フィールドで、自分のサブスクリプションが選択されていることを確認し、**[ストレージの作成]** を選択します。 ストレージ アカウントが作成されるまで待ちます。

1. ストレージ アカウントが作成されると、**[Cloud Shell]** コンソールが開き、**PS /home/yourname>** の形式でプロンプトが表示されます。

1. PowerShell プロンプトで、「`Get-AzSubscription`」と入力し、Enter キーを押してサブスクリプションを確認します。

1. 「`Get-AzResourceGroup`」と入力してリソース グループ情報を確認します。

1. ドロップダウン リストを使用して、PowerShell から **Bash** シェルに切り替えて、選択内容を確認します。

1. Bash シェル プロンプトで、「`az account list`」と入力し、Enter キーを押してサブスクリプションに関する情報を確認します。 また、タブの補完を試してみてください。

1. 「`az resource list`」と入力してリソース グループ情報を確認します。

1. PowerShell インターフェイスに戻します。

1. **PowerShell** コンソールで次のコマンドを入力し、Enter キーを押して新しいリソース グループを作成します (`<resource-group-name>` プレースホルダーは、自分の名前に置き換えます)。

    ```powershell
    New-AzResourceGroup -Name <resource-group-name> -Location westeurope
    ```

1. 新しいリソース グループが作成されていることを確認します。 ご利用のリソース グループの名前を記録します。 これは、このラボの次の演習で必要となります。

## 演習 3:Azure PowerShell を使った Azure リソースの管理

### タスク 1: PowerShell を使用して Azure VM を作成する

1. PowerShell ウィンドウで次のコマンドを入力し、この演習で作成する Azure VM のオペレーティング システムの管理者資格情報を指定します。

   ```powershell
   $cred = Get-Credential -Message "Enter an admin username and password for the operating system"
   ```

1. プロンプトが表示されたら、新しい VM の管理者資格情報として使用する任意のユーザー名とパスワードを入力します。 ユーザー名として **Admin** または **Administrator** は使用しないでください。 小文字、大文字、数字、および少なくとも 1 つの特殊文字を含む、8 文字以上の複雑なパスワードを選択してください。

1. PowerShell ウィンドウで、次のコマンドを入力して VM パラメーターを定義し、Enter キーを押します (`<resource-group-name>` プレースホルダーは、前の演習で作成したリソース グループの名前に置き換えます)。

   ```powershell
   $vmParams = @{
     ResourceGroupName = '<resource-group-name>'
     Name = 'TestVM1'
     Location = 'westeurope'
     ImageName = 'Win2019Datacenter'
     PublicIpAddressName = 'TestPublicIp'
     Credential = $cred
     OpenPorts = 3389
   }
   ```

1. これらのパラメーターに基づいて新しい VM を作成し、それに対する参照を `newVM1` 変数内に格納するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $newVM1 = New-AzVM @vmParams
   ```

   >**注:** Azure VM が作成されるまでお待ちください。

1. 新しい VM の構成設定を特定するには、次のコマンドを入力し、それぞれの後に Enter キーを押します。

   ```powershell
   $NewVM1

   $newVM1.OSProfile | Select-Object ComputerName,AdminUserName

   $newVM1 | Get-AzNetworkInterface | Select-Object -ExpandProperty IpConfigurations | Select-Object Name,PrivateIpAddress
   ```

1. Azure VM をデプロイしたリソース グループの名前を取得して変数内に格納するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $rgName = $NewVM1.ResourceGroupName
   ```

1. Azure VM のネットワーク インターフェイスに割り当てられているパブリック IP アドレスを特定して接続できるようにするには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $publicIp = Get-AzPublicIpAddress -Name TestPublicIp -ResourceGroupName $rgName
   
   $publicIp | Select-Object Name,IpAddress,@{label='FQDN';expression={$_.DnsSettings.Fqdn}}
   ```

1. テーブル出力の **IPAddress** の値をメモします。

1. 次のコマンドを入力し、Enter キーを押して、リモート デスクトップを使用して VM に接続します。

   ```powershell
   mstsc.exe /v $publicIp.IpAddress
   ```

1. プロンプトが表示されたら、Azure VM のプロビジョニング中に指定した管理者資格情報でサインインします。 Windows Server 2019 VM に接続していることを確認したら、オペレーティング システムをシャットダウンします。 これにより、リモート デスクトップ セッションが自動的に終了します。

### タスク 2: PowerShell を使用して Azure VM にディスクを追加する

1. **LON-CL1** コンピューターで、Azure portal が表示されている Web ブラウザー ウィンドウに切り替え、**[Virtual Machines]** ページに移動します。

1. **[Virtual Machines]** ページで、**[TestVM1]** エントリを選択します。

1. **[TestVM1]** VM の**概要**ページで、そのパラメーターを確認し、ナビゲーション メニューの **[設定]** セクションで **[ディスク]** を選択します。 

1. ディスクの一覧を確認し、1 つのディスク (OS ディスク) のみが表示されていることを確認します。

1. 既存の VM 用のデータ ディスクを作成するには、PowerShell ウィンドウで次のコマンドを入力し、それぞれの後に Enter キーを押します。

   ```powershell
   $VirtualMachine = Get-AzVM -ResourceGroupName "<resource-group-name>" -Name "TestVM1"

   Add-AzVMDataDisk -VM $VirtualMachine -Name "disk1" -LUN 0 -Caching ReadOnly -DiskSizeinGB 1 -CreateOption Empty

   Update-AzVM -ResourceGroupName "<resource-group-name>" -VM $VirtualMachine
   ```

1. Azure portal に切り替えて、**[TestVM1 \| ディスク]** ページを更新します。 ディスクの一覧の **[データ ディスク]** セクションに **disk1** という名前の新しいディスクが含まれているかどうかを確認します。

### タスク 3:Azure リソースを削除する

1. **LON-CL1** コンピューターで、**[PowerShell]** ウィンドウに戻ります。

1. **[PowerShell]** コンソールで次のコマンドを入力し、Enter キーを押して、このラボで前に作成したリソース グループとそのすべてのリソースを削除します。

    ```powershell
    Remove-AzResourceGroup -Name $rgName -Force
    ```

1. コマンドが完了するまで待機します。 通常は 5 分もかかりません。
