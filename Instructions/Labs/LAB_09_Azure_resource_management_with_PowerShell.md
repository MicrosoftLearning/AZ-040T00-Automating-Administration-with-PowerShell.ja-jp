---
lab:
  title: 'ラボ: PowerShell を使用した Azure リソース管理'
  module: 'Module 9: Managing Azure resources with PowerShell'
ms.openlocfilehash: c2b21bf0c29c091c47cf40002a0e65ab87ea9c38
ms.sourcegitcommit: 9c31a6ab628c30fac88ec9070c3d807f2a9bbfdb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2022
ms.locfileid: "146824972"
---
# <a name="lab-azure-resource-management-with-powershell"></a>ラボ: PowerShell を使用した Azure リソース管理

## <a name="scenario"></a>シナリオ

あなたは Adatum Corporation のロンドン支店のシステム管理者です。 会社の仮想マシン (VM) とその他のリソースを実行するため、Microsoft Azure プラットフォームを評価する必要があります。 評価の一環として、Azure ベースのリソースの PowerShell 管理もテストする必要があります。

# <a name="objectives"></a>目標

このラボを完了すると、次のことができるようになります。

- Az module for Windows PowerShell をインストールする。
- Azure Cloud Shell 環境を実行して使用する。
- PowerShell を使用して Azure VM とディスクを管理する。

## <a name="estimated-time-60-minutes"></a>予想所要時間: 60 分

## <a name="lab-setup"></a>ラボのセットアップ

仮想マシン:**LON-DC1**、**LON-CL1**

ユーザー名: **Adatum\\Administrator**

パスワード: **Pa55w.rd**

このラボでは、使用可能な仮想マシン環境を使います。 ラボを開始する前に、次の手順を行ってください。

1. **LON-DC1** を開き、パスワード **Pa55w.rd** を使って **Adatum\\Administrator** としてサインインします。
1. **LON-CL1** について手順 1 を繰り返します。

## <a name="exercise-1-activating-the-azure-subscription-and-installing-the-powershell-az-module"></a>演習 1: Azure サブスクリプションのアクティブ化と PowerShell Az モジュールのインストール

### <a name="scenario-1"></a>シナリオ 1

ご自分の試用版の Azure サブスクリプションがアクティブで、Az module for Windows PowerShell をインストールしていることを確認する必要があります。

この演習の主なタスクは次のとおりです。

1. Azure Pass のバウチャーを使用して Azure サブスクリプションをアクティブにする。
1. Azure Az module for PowerShell をインストールする。

### <a name="task-1-activate-your-azure-subscription-by-using-azure-pass-voucher"></a>タスク 1: Azure Pass のバウチャーを使用して Azure サブスクリプションをアクティブにする

1. **LON-CL1** で Microsoft Edge ブラウザーを開き、 **https://www.microsoftazurepass.com/** に移動します。
1. 試用版の Azure サブスクリプションに使用する Microsoft アカウントでサインインします。
1. インストラクターまたはラボ ホスティング プロバイダーによって提供される Azure Pass コードを使用します。
1. **[サブスクリプション]** ページに、 **[Azure Pass - スポンサーシップ]** が **[アクティブ]** の状態で表示され、50 米国ドルの残高があることを確認します。

### <a name="task-2-install-the-azure-az-module-for-powershell"></a>タスク 2: Azure Az module for PowerShell をインストールする

1. **LON-CL1** で、PowerShell 7.1 環境を開始します。
1. PowerShell のバージョンを確認するには、`$PSVersionTable.PSVersion` を使用します。
1. 現在のユーザーの実行ポリシーを **RemoteSigned** に設定します。
1. PowerShell ギャラリーから、**install-module** コマンドを使用して、現在のユーザーの Az モジュールをインストールします。
1. **Connect-AzAccount** を使用して Azure サブスクリプションにサインインします。

## <a name="exercise-2-using-azure-cloud-shell"></a>演習 2: Azure Cloud Shell の使用

### <a name="scenario-2"></a>シナリオ 2

Azure VM などの他の Azure リソースを操作するには、Azure リソースグループを作成する必要があります。 このタスクを実行するには、Azure Cloud Shell を使用することを決定します。

この演習の主なタスクは次のとおりです。

- Azure Cloud Shell を使って、リソース グループを作成する。

### <a name="task-1-use-azure-cloud-shell-to-create-a-resource-group"></a>タスク 1: Azure Cloud Shell を使って、リソース グループを作成する

1. Azure portal で Azure テナントを確認して、Azure VM またはストレージ アカウントが存在しないことを確認します。
1. Azure Cloud Shell 環境を開始します。
1. **Get-AzSubscription** コマンドを使用して、サブスクリプションを確認します。
1. **Get AzResourceGroup** コマンドを使用して、リソース グループが存在しないことを確認します。
1. Azure Cloud Shell で **Bash** 環境に切り替え、**az account list** と **az resource list** を使用して、サブスクリプションとリソース グループを確認します。 Azure Cloud Shell で PowerShell 環境に切り替えます。
1. **New-AzResourceGroup** コマンドを使用して、西ヨーロッパ リージョンに新しいリソース グループを作成します。 リソース グループに **YourNameM9** という名前を付け、*YourName* をご自分の名前に置き換えます。

## <a name="exercise-3-managing-azure-resources-with-azure-powershell"></a>演習 3: Azure PowerShell を使った Azure リソースの管理

Azure サブスクリプションとリソース グループを作成したら、PowerShell を使用して、Windows Server 2019 イメージに基づいて Azure VM を作成します。

この演習の主なタスクは次のとおりです。

1. PowerShell を使用して Azure VM を作成する。
1. PowerShell を使用して Azure VM にディスクを追加する。
1. Azure リソースを削除する。

### <a name="task-1-create-an-azure-vm-by-using-powershell"></a>タスク 1: PowerShell を使用して Azure VM を作成する

1. PowerShell 7.1 ウィンドウで、**Get-Credential** コマンドを使用して、新しい Azure VM の管理者資格情報を `$cred` 変数に格納します。 ユーザー名として "**Admin**" や "**Administrator**" を使用せず、小文字、大文字、数字、および少なくとも 1 つの特殊文字を含む、8 文字以上の複雑なパスワードを選択してください。
1. PowerShell 7.1 ウィンドウで、次のコマンドを使用して VM パラメーターを定義します (`<resource-group-name>` プレースホルダーは、前の演習で作成したリソース グループの名前に置き換えます)。

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

1. **New-AzVM** コマンドを使用して、前の手順で定義したパラメーターで新しい VM を作成します。 VM への参照を `$NewVM1` 変数に保存します。
1. 新しい VM の構成設定を識別するには、次のコマンドを実行します。

   ```powershell
   $NewVM1
   $newVM1.OSProfile | Select-Object ComputerName,AdminUserName
   $newVM1 | Get-AzNetworkInterface | Select-Object -ExpandProperty IpConfigurations | Select-Object Name,PrivateIpAddress
   ```

1. Azure VM をデプロイしたリソース グループの名前を取得して変数内に保存するには、次のコマンドを実行します。

   ```powershell
   $rgName = $NewVM1.ResourceGroupName
   ```


1. Azure VM のネットワーク インターフェイスに割り当てられているパブリック IP アドレスを特定して接続できるようにするには、次のコマンドを実行します。

   ```powershell
   $publicIp = Get-AzPublicIpAddress -Name TestPublicIp -ResourceGroupName $rgName
   
   $publicIp | Select-Object Name,IpAddress,@{label='FQDN';expression={$_.DnsSettings.Fqdn}}
   ```

1. テーブル出力の IPAddress の値をメモします。
1. "**mstsc.exe**" ツールを使用してリモート デスクトップを使用し、作成した Azure VM に接続します。 Azure VM のプロビジョニング中に指定した管理者資格情報でサインインします。 接続したら、オペレーティング システムをシャットダウンします。

### <a name="task-2-add-a-disk-to-the-azure-vm-by-using-powershell"></a>タスク 2: PowerShell を使用して Azure VM にディスクを追加する

1. Azure portal を使用して、**TestVM1** Azure VM のページに移動します。
1. **TestVM1** に接続されているディスクが 1 つだけであることを確実にします (オペレーティング システムをホストしています)。
1. 次のコマンドを実行して、既存の VM のデータ ディスクを作成します。

   ```powershell
   $VirtualMachine = Get-AzVM -ResourceGroupName $rgName -Name "TestVM1"
   Add-AzVMDataDisk -VM $VirtualMachine -Name "disk1" -LUN 0 -Caching ReadOnly -DiskSizeinGB 1 -CreateOption Empty
   Update-AzVM -ResourceGroupName $rgName -VM $VirtualMachine
   ```

1. Azure portal に切り替えて、 **[Disks]** ページを更新します。 **[データ ディスク]** セクションに、**disk1** という名前の新しいディスクがあることを確認します。

### <a name="task-3-delete-the-azure-resources"></a>タスク 3:Azure リソースを削除する

1. **LON-CL1** コンピューターで、[PowerShell] ウィンドウに戻ります。
1. PowerShell コンソールで、次のコマンドを実行して、このラボで前に作成したリソース グループとそのすべてのリソースを削除します。

    ```powershell
    Remove-AzResourceGroup -Name $rgName -Force
    ```

1. コマンドが完了するまで待機します。 通常は 5 分もかかりません。
