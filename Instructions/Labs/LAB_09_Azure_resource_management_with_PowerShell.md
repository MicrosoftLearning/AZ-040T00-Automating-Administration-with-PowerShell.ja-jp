---
lab:
  title: 'ラボ: PowerShell を使った Azure リソースの管理'
  module: 'Module 9: Managing Azure resources with PowerShell'
---

# ラボ: PowerShell を使った Azure リソースの管理

## シナリオ

あなたは Adatum Corporation のロンドン ブランチ オフィスのシステム管理者です。 会社の仮想マシン (VM) とその他のリソースを実行するため、Microsoft Azure プラットフォームを評価する必要があります。 評価の一環として、Azure ベースのリソースの PowerShell 管理もテストする必要があります。

# 目標

このラボを完了すると、次のことができるようになります。

- Az module for Windows PowerShell をインストールする。
- Azure Cloud Shell 環境を実行して使用する。
- PowerShell を使用して Azure VM とディスクを管理する。

## 予想所要時間: 60 分

## ラボのセットアップ

仮想マシン:**LON-DC1**、**LON-CL1**

ユーザー名: **Adatum\\Administrator**

パスワード: **Pa55w.rd**

このラボでは、提供されている仮想マシン環境を使用します。 ラボを開始する前に、次の手順を行ってください。

1. **LON-DC1** を開き、パスワード **Pa55w.rd** を使って**Adatum\\Administrator** としてサインインします。
1. **LON-CL1** について手順 1 を繰り返します。

## 演習 1:Azure サブスクリプションのアクティブ化と PowerShell Az モジュールのインストール

### シナリオ 1

自分の環境で提供されている Azure サブスクリプションにサインインし、Windows PowerShell 用の AZ モジュールをインストールする必要があります。

この演習の主なタスクは次のとおりです。

1. Azure サブスクリプションにサインインします。
1. Azure Az module for PowerShell をインストールする。

### タスク 1: Azure サブスクリプションにサインインする

1. **LON-CL1** で Microsoft Edge ブラウザーを開き、**https://portal.azure.com/** に移動します。
1. 講師から提供された資格情報を使用してサインインします。

### タスク 2: Azure Az module for PowerShell をインストールする

1. **LON-CL1**で、PowerShell 7.1 環境を開始します。
1. PowerShell のバージョンを確認するには、`$PSVersionTable.PSVersion` を使用します。
1. 現在のユーザーの実行ポリシーを **RemoteSigned** に設定します。
1. PowerShell ギャラリーから、**install-module** コマンドを使用して、現在のユーザーの Az モジュールをインストールします。
1. **Connect-AzAccount** を使用して Azure サブスクリプションにサインインします。

## 演習 2:Azure Cloud Shell の使用

### シナリオ 2

Azure VM などの他の Azure リソースを操作するには、Azure リソースグループを作成する必要があります。 このタスクを実行するには、Azure Cloud Shell を使用することを決定します。

この演習の主なタスクは次のとおりです。

- Azure Cloud Shell を使って、リソース グループを作成する。

### タスク 1: Azure Cloud Shell を使って、リソース グループを作成する

1. Azure portal で Azure テナントを確認して、Azure VM またはストレージ アカウントが存在しないことを確認します。
1. Azure Cloud Shell 環境を開始します。
1. **Get-AzSubscription** コマンドを使用して、サブスクリプションを確認します。
1. **Get AzResourceGroup** コマンドを使用して、リソース グループが存在しないことを確認します。
1. Azure Cloud Shell で **Bash** 環境に切り替え、**az account list** と **az resource list** を使用して、サブスクリプションとリソース グループを確認します。 Azure Cloud Shell で PowerShell 環境に切り替えます。
1. **New-AzResourceGroup** コマンドを使用して、西ヨーロッパ リージョンに新しいリソース グループを作成します。 リソース グループに **YourNameM9** という名前を付け、*YourName* をご自分の名前に置き換えます。

## 演習 3:Azure PowerShell を使った Azure リソースの管理

Azure サブスクリプションとリソース グループを作成したら、PowerShell を使用して、Windows Server 2019 イメージに基づいて Azure VM を作成します。

この演習の主なタスクは次のとおりです。

1. PowerShell を使用して Azure VM を作成する。
1. PowerShell を使用して Azure VM にディスクを追加する。
1. Azure リソースを削除する。

### タスク 1: PowerShell を使用して Azure VM を作成する

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

### タスク 2: PowerShell を使用して Azure VM にディスクを追加する

1. Azure portal を使用して、**TestVM1** Azure VM のページに移動します。
1. **TestVM1** に接続されているディスクが 1 つだけであることを確実にします (オペレーティング システムをホストしています)。
1. 次のコマンドを実行して、既存の VM のデータ ディスクを作成します。

   ```powershell
   $VirtualMachine = Get-AzVM -ResourceGroupName $rgName -Name "TestVM1"
   Add-AzVMDataDisk -VM $VirtualMachine -Name "disk1" -LUN 0 -Caching ReadOnly -DiskSizeinGB 1 -CreateOption Empty
   Update-AzVM -ResourceGroupName $rgName -VM $VirtualMachine
   ```

1. Azure portal に切り替えて、**[Disks]** ページを更新します。 **[データ ディスク]** セクションに、**disk1**という名前の新しいディスクがあることを確認します。

### タスク 3:Azure リソースを削除する

1. **LON-CL1** コンピューターで、[PowerShell] ウィンドウに戻ります。
1. PowerShell コンソールで、次のコマンドを実行して、このラボで前に作成したリソース グループとそのすべてのリソースを削除します。

    ```powershell
    Remove-AzResourceGroup -Name $rgName -Force
    ```

1. コマンドが完了するまで待機します。 通常は 5 分もかかりません。
