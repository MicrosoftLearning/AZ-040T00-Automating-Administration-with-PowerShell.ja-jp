---
lab:
  title: 'ラボ: PowerShell を使用した Azure リソース管理'
  type: Answer Key
  module: 'Module 9: Managing Azure resources with PowerShell'
ms.openlocfilehash: 65af5195515f51040fb474553f3c7907eefeed14
ms.sourcegitcommit: a95a9bb3a7919b785df0574c3407f4b6c3bea9f5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2021
ms.locfileid: "132116728"
---
# <a name="lab-answer-key-azure-resource-management-with-powershell"></a>ラボの回答キー: PowerShell を使用した Azure リソース管理

## <a name="exercise-1-activating-the-azure-subscription-and-installing-the-powershell-az-module"></a>演習 1: Azure サブスクリプションのアクティブ化と PowerShell Az モジュールのインストール

### <a name="task-1-activate-your-azure-subscription-by-using-azure-pass-voucher"></a>タスク 1: Azure Pass のバウチャーを使用して Azure サブスクリプションをアクティブにする

1. **LON-CL1** で Microsoft Edge ブラウザーを開き、 **https://www.microsoftazurepass.com/** に移動します。
1. **[Try Microsoft Azure Pass]\(Microsoft Azure Pass を試す\)** Web ページで、 **[始める]** を選択します。
1. **[サインイン]** ページで、Azure Pass に使用する Microsoft アカウントを入力します。 任意の Microsoft アカウントにすることも、新しいアカウントを作成することもできます。 後で必要になるため、ここでサインインに使用するアカウントを必ずメモしてください。 **[次へ]** を選択します。
1. **[パスワードの入力]** ページで、このラボで使用する Microsoft アカウントのパスワードを入力します。 **[サインイン]** をクリックします。
1. 確認ページで、指定したデータを確認し、 **[Microsoft アカウントの確認]** を選択します。
1. **[プロモーション コードの入力]** ボックスに、インストラクターまたはラボ ホスティング プロバイダーから提供された Azure Pass コードを入力します。 **[プロモーション コードの要求]** を選択します。 要求が処理されるまで待ちます。
1. **[プロファイル]** ページで、自分のデータを確認し、 **[サブスクリプション契約、プランの詳細、およびプライバシーに関する声明に同意します]** をオンにして、 **[サインアップ]** を選択します。
1. **[アカウントの設定中]** ページで、サインアップ エクスペリエンスに関するフィードバックを入力し、 **[送信]** を選択します。
1. Azure portal が開きます。 **[Microsoft Azure へようこそ]** ページで、 **[後で]** を選択してツアーをスキップします。
1. **[Microsoft Azure]** ページで、 **[サブスクリプション]** を選択します。
1. **[サブスクリプション]** ページで、 **[Azure Pass - スポンサー プラン]** の状態が **[アクティブ]** であることを確認します。
1. 同じ Microsoft Edge ウィンドウで新しいタブを開き、 **https://www.microsoftazuresponsorships.com/** に移動します。
1. **[Azure スポンサー プラン]** ページで、 **[残高の確認]** を選択します。
1. **[Azure スポンサー プラン]** ページで、使用可能なクレジットが 50 米国ドルであることを確認します。 **[Azure スポンサー プラン]** タブを閉じます。Microsoft Azure portal タブは開いたままにしておきます。

### <a name="task-2-install-the-azure-az-module-for-powershell"></a>タスク 2: Azure Az module for PowerShell をインストールする

1. **LON-CL1** で、 **[開始]** を選択します。 管理者として PowerShell 7.1 を実行します。
1. **[管理者: Windows PowerShell]** ウィンドウで次のコマンドを入力し、Enter キーを押して PowerShell のバージョンを確認します。

   ```powershell
   $PSVersionTable.PSVersion
   ```

1. 実行ポリシーを適切な値に設定して Az モジュールをインストールできるようにするには、次のコマンドを入力し、Enter キーを押します。 「**Y**」と入力して、選択した内容を確認します。

   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
   ```

1. Az モジュールをインストールするには、次のコマンドを入力します。

   ```powershell
   Install-Module -Name Az -Scope CurrentUser -Repository PSGallery -Force
   ```

1. モジュールがインストールされ、コマンド プロンプトが表示されるまで待ちます。
1. Az モジュールがインストールされたら、次のコマンドを入力します。

   ```powershell
   Connect-AzAccount
   ```

1. メッセージが表示されたら、前のタスクで使用したアカウントでサインインし、Azure サブスクリプションをプロビジョニングします。
1. サインイン後、アカウント名と **Azure Pass - スポンサー プラン** のサブスクリプションが一覧に表示されていることを確認します。

## <a name="exercise-2-using-azure-cloud-shell"></a>演習 2: Azure Cloud Shell の使用

### <a name="task-1-use-azure-cloud-shell-to-create-a-resource-group"></a>タスク 1: Azure Cloud Shell を使って、リソース グループを作成する

1. **LON-CL1** コンピューターで、Azure portal が開いた状態のブラウザー ウィンドウを復元します。
1. Microsoft Azure portal のホームページで、 **[仮想マシン]** を選択します。 仮想マシン (VM) が作成されていないことを確認します。 **[ホーム]** を選択します。
1. Microsoft Azure portal のホームページで、 **[ストレージ アカウント]** を選択します。 ストレージ アカウントが作成されていないことを確認します。 **[ホーム]** を選択します。
1. Microsoft Azure portal のホームページで、 **[Cloud Shell]** アイコンを選択します。
1. **[Azure Cloud Shell へようこそ]** ウィンドウで **[PowerShell]** を選択します。
1. **[ストレージがマウントされていません]** ページで、Cloud Shell を実行するために必要なストレージ アカウントがないことに関する注記を確認します。 **[サブスクリプション]** フィールドで **Azure Pass - スポンサー プラン** のサブスクリプションが選択されていることを確認し、 **[ストレージの作成]** を選択します。 ストレージ アカウントが作成されるまで待ちます。
1. ストレージ アカウントが作成されると、Cloud Shell コンソールが開き、**PS /home/yourname>** の形式でプロンプトが表示されます。
1. PowerShell プロンプトで、「**Get-AzSubscription**」と入力し、Enter キーを押してサブスクリプションを確認します。
1. 「**Get-AzResourceGroup**」と入力し、リソース グループの情報を確認します。
1. ドロップダウン リストを使用して、PowerShell から **Bash** シェルに切り替えて、選択内容を確認します。
1. Bash シェル プロンプトで、「**az account list**」と入力し、Enter キーを押してサブスクリプションを確認します。 また、タブの補完を試してみてください。
1. 「**az resource list**」と入力し、リソース グループの情報を確認します。
1. PowerShell インターフェイスに戻します。
1. PowerShell コンソールで次のコマンドを入力し、Enter キーを押して新しいリソース グループを作成します。

    ```powershell
    New-AzResourceGroup -Name YournameM9 -Location westeurope
    ```

1. 新しいリソース グループが作成されていることを確認します。 リソース グループの名前を憶えておいてください。

## <a name="exercise-3-managing-azure-resources-with-azure-powershell"></a>演習 3: Azure PowerShell を使った Azure リソースの管理

### <a name="task-1-create-an-azure-vm-by-using-powershell"></a>タスク 1: PowerShell を使用して Azure VM を作成する

1. PowerShell 7.1 ウィンドウで次のコマンドを入力して、Azure で作成する VM の管理者資格情報を定義します。

   ```powershell
   $cred = Get-Credential -Message "Enter a username and password for the virtual machine."
   ```

1. メッセージが表示されたら、新しい VM の管理者資格情報として使用するユーザー名とパスワードを選択します。 管理者の名前として Admin を使用しないでください。
1. PowerShell 7.1 ウィンドウで次のコマンドを入力して VM のパラメーターを定義し、Enter キーを押します。 *yourname* を自分の名前に置き換えます。

   ```powershell
   $vmParams = @{
     ResourceGroupName = 'yournameM9'
     Name = 'TestVM1'
     Location = 'westeurope'
     ImageName = 'Win2016Datacenter'
     PublicIpAddressName = 'TestPublicIp'
     Credential = $cred
     OpenPorts = 3389
   }
   ```

1. これらのパラメーターに基づいて新しい VM を作成し、それを `newVM1` 変数に格納するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $newVM1 = New-AzVM @vmParams
   ```

   >**注:** Azure VM が作成されるまでお待ちください。

1. 新しい VM のパラメーターを確認するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $NewVM1
   $newVM1.OSProfile | Select-Object ComputerName,AdminUserName
   $newVM1 | Get-AzNetworkInterface | Select-Object -ExpandProperty IpConfigurations | Select-Object Name,PrivateIpAddress
   ```

1. 作成した Azure VM に接続できるように、そのパブリック IP アドレスを見つけるには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $publicIp = Get-AzPublicIpAddress -Name TestPublicIp -ResourceGroupName yournameM9
   
   $publicIp | Select-Object Name,IpAddress,@{label='FQDN';expression={$_.DnsSettings.Fqdn}}
   ```

1. テーブル出力の **IPAddress** の値をメモします。
1. 次のコマンドを入力し、Enter キーを押して VM に接続します。

   ```powershell
   mstsc.exe /v <PUBLIC_IP_ADDRESS>
   ```

1. メッセージが表示されたら、この VM 用に指定した資格情報を使用してサインインします。 Windows Server 2016 VM に接続されたことを確認します。 VM の機能を確認し、シャットダウンします。

### <a name="task-2-add-a-disk-to-the-azure-vm-by-using-powershell"></a>タスク 2: PowerShell を使用して Azure VM にディスクを追加する

1. Azure portal が開いた状態の Microsoft Edge ウィンドウに切り替えます。 **[仮想マシン]** を選択します。
1. **TestVM1** が一覧に表示されていることを確認します。 それを選択します。
1. **[概要]** ページで、作成した VM のパラメーターを確認します。 **[ディスク]** を選択します。 作成済みのディスクが 1 つ (OS ディスク) のみであることを確認します。
1. 既存の VM 用のデータ ディスクを作成するには、PowerShell 7.1 ウィンドウで次のコマンドを入力し、それぞれの後に Enter キーを押します。

   ```powershell
   $VirtualMachine = Get-AzVM -ResourceGroupName "yournameM9" -Name "TestVM1"
   Add-AzVMDataDisk -VM $VirtualMachine -Name "disk1" -LUN 0 -Caching ReadOnly -DiskSizeinGB 1 -CreateOption Empty
   Update-AzVM -ResourceGroupName "yournameM9" -VM $VirtualMachine
   ```

1. Azure portal に切り替えて、 **[ディスク]** ページを更新します。 **[データ ディスク]** セクションで **disk1** という名前の新しいディスクを確認できます。
