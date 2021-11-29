---
lab:
  title: 'ラボ: PowerShell を使用した Azure リソース管理'
  module: 'Module 9: Managing Azure resources with PowerShell'
ms.openlocfilehash: 75f55607cb249d38e331ba13b7c17781a1307e7c
ms.sourcegitcommit: a95a9bb3a7919b785df0574c3407f4b6c3bea9f5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2021
ms.locfileid: "132116727"
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

このラボでは、提供されている仮想マシン環境を使用します。 ラボを開始する前に、次の手順を完了します。

1. **LON-DC1** を開いて、**Adatum\\Administrator** として、パスワード **Pa55w.rd** を使ってサインインします。
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
1. **New-AzResourceGroup** コマンドを使用して、西ヨーロッパ リージョンに新しいリソース グループを作成します。 リソース グループに *YourNameM9* という名前を付け、*YourName* をご自分の名前に置き換えます。

## <a name="exercise-3-managing-azure-resources-with-azure-powershell"></a>演習 3: Azure PowerShell を使った Azure リソースの管理

Azure サブスクリプションとリソース グループを作成したら、PowerShell を使用して、Windows Server 2016 イメージに基づいて Azure VM を作成します。

この演習の主なタスクは次のとおりです。

1. PowerShell を使用して Azure VM を作成する。
1. PowerShell を使用して Azure VM にディスクを追加する。

### <a name="task-1-create-an-azure-vm-by-using-powershell"></a>タスク 1: PowerShell を使用して Azure VM を作成する

1. PowerShell 7.1 ウィンドウで、**Get-Credential** コマンドを使用して、新しい Azure VM の管理者資格情報を `$cred` 変数に格納します。 管理者には Admin という名前を使用しないでください。
1. PowerShell 7.1 ウィンドウで、次のコマンドを使用して VM のパラメーターを定義します (*yourname* をご自分の実際の名前に置き換えてください)。

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

1. **New-AzVM** コマンドを使用して、前の手順で定義したパラメーターで新しい VM を作成します。 これを `$NewVM1` 変数に格納します。
1. 新しい VM のパラメーターを確認するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $NewVM1
   $newVM1.OSProfile | Select-Object ComputerName,AdminUserName
   $newVM1 | Get-AzNetworkInterface | Select-Object -ExpandProperty IpConfigurations | Select-Object Name,PrivateIpAddress
   ```

1. 作成した Azure VM でパブリック IP アドレスを検索して接続できるようにするには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $publicIp = Get-AzPublicIpAddress -Name TestPublicIp -ResourceGroupName yournameM9
   
   $publicIp | Select-Object Name,IpAddress,@{label='FQDN';expression={$_.DnsSettings.Fqdn}}
   ```

1. テーブル出力の IPAddress の値をメモします。
1. **mstsc.exe** ツールを使用して、作成した Azure VM に接続します。 最初の手順で定義した資格情報を使用してサインインします。

### <a name="task-2-add-a-disk-to-the-azure-vm-by-using-powershell"></a>タスク 2: PowerShell を使用して Azure VM にディスクを追加する

1. **TestVM1** のプロパティに移動するには、Azure portal を使用します。
1. **TestVM1** に、オペレーティング システム ディスクのみがインストールされていることを確認します。
1. 次のコマンドを使用して、それぞれの後に Enter キーを押し、既存の VM 用のデータ ディスクを作成します。

   ```powershell
   $VirtualMachine = Get-AzVM -ResourceGroupName "yournameM9" -Name "TestVM1"
   Add-AzVMDataDisk -VM $VirtualMachine -Name "disk1" -LUN 0 -Caching ReadOnly -DiskSizeinGB 1 -CreateOption Empty
   Update-AzVM -ResourceGroupName "yournameM9" -VM $VirtualMachine
   ```

1. Azure portal に切り替えて、 **[Disks]** ページを更新します。 **[データ ディスク]** セクションに、**disk1** という名前の新しいディスクがあることを確認します。
