---
lab:
  title: 'ラボ: PowerShell を使用したリモート管理の実行'
  type: Answer Key
  module: 'Module 8: Administering remote computers with Windows PowerShell'
ms.openlocfilehash: f975899c5345c9e4279f26ca78eaafbb467572d2
ms.sourcegitcommit: a95a9bb3a7919b785df0574c3407f4b6c3bea9f5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2021
ms.locfileid: "132116730"
---
# <a name="lab-answer-key-performing-remote-administration-with-powershell"></a>ラボの応答キー: PowerShell を使用したリモート管理の実行

## <a name="exercise-1-enabling-remoting-on-the-local-computer"></a>演習 1: ローカル コンピューターでリモート処理を有効にする

### <a name="task-1-enable-remoting-for-incoming-connections"></a>タスク 1: 着信接続に対してリモート処理を有効にする

1. パスワード **Pa55w.rd** を使用して、**Adatum\\Administrator** として **LON-CL1** 仮想マシンにサインインしていることを確かめます。

1. **[スタート]** メニューを選択してから、「**powersh**」と入力します。

1. 結果リストで、 **[Windows PowerShell]** を右クリックするか、そのコンテキスト メニューをアクティブにして、 **[管理者として実行]** を選択します。

1. 正しい実行ポリシーが設定されていることを確かめるには、Windows PowerShell コマンド ウィンドウで次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Set-ExecutionPolicy RemoteSigned
   ```

1. **[はい]** を選択するか、「**Y**」と入力して変更を確定します。

1. **LON-CL1** コンピューターで、次のコマンドを実行します。

   ```powershell
   Enable-PSremoting
   ```

   プロンプトが表示されたら、 **[Y]** を選択してすべてのプロンプトに対して [はい] と答えます。これにより、リモート処理が有効になります。

1. セッション構成を一覧表示できるコマンドを見つけるには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   help *sessionconfiguration*
   ```

   > **注:** **Get-PSSessionConfiguration** コマンドに注目してください。

1. セッション構成を一覧表示するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-PSSessionConfiguration
   ```

1. 2 つから 4 つのセッション構成が作成されたことを確認します。 Windows PowerShell ウィンドウは開いたままにしておきます。

## <a name="exercise-2-performing-one-to-one-remoting"></a>演習 2: 一対一のリモート処理の実行

### <a name="task-1-connect-to-the-remote-computer-and-install-an-operating-system-feature"></a>タスク 1: リモート コンピューターに接続し、オペレーティング システム機能をインストールする

1. 引き続き、**Pa55w.rd** パスワードを使用して、**Adatum\\Administrator** として **LON-CL1** 仮想マシンにサインインしていることを確かめます。
1. **LON-CL1** で、**LON-DC1** への一対一の接続を確立するには、Windows PowerShell で次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Enter-PSSession –ComputerName LON-DC1
   ```

1. 接続後、**LON-DC1** にネットワーク負荷分散 (NLB) 機能をインストールするには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Install-WindowsFeature NLB
   ```

1. コマンドが完了するまで待機します。
1. 切断するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Exit-PSSession
   ```

### <a name="task-2-test-multi-hop-remoting"></a>タスク 2: マルチホップ リモート処理をテストする

1. **LON-DC1** への一対一のリモート処理接続を確立するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Enter-PSSession –ComputerName LON-DC1
   ```

2. **LON-DC1** から **LON-CL1** への接続を確立するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Enter-PSSession –ComputerName LON-CL1
   ```

   > **注:** 2 番目のホップを示すエラーが表示されるはずです。 既定では、既に確立されている接続を介して接続を確立することはできません。

3. 接続を閉じるには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Exit-PSSession
   ```

### <a name="task-3-observe-remoting-limitations"></a>タスク 3: リモート処理の制限事項を確認する

1. パスワード **Pa55w.rd** を使用して、**Adatum\\Administrator** として **LON-CL1** 仮想マシンにサインインしていることを確かめます。
1. **LON-CL1** への一対一の接続を確立するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Enter-PSSession –ComputerName localhost
   ```

1. 次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Notepad
   ```

   > **注:** メモ帳が開くのを待機している間は、シェルの応答が停止しているように見えることに注目してください。これは、メモ帳がグラフィカル アプリケーションであり、シェルではグラフィカル ユーザー インターフェイス (GUI) を表示できないためです。

1. **Ctrl + C** キーを押してプロセスをキャンセルし、シェル プロンプトに戻ります。
1. 切断するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Exit-PSSession
   ```

## <a name="exercise-3-performing-one-to-many-remoting"></a>演習 3: 一対多のリモート処理の実行

### <a name="task-1-retrieve-a-list-of-physical-network-adapters-from-two-computers"></a>タスク 1: 2 台のコンピューターから物理ネットワーク アダプターのリストを取得する

1. 引き続き、**Pa55w.rd** パスワードを使用して、**Adatum\\Administrator** として **LON-CL1** 仮想マシンにサインインしていることを確かめます。
1. **LON-CL1** で、ネットワーク アダプターを一覧表示できるコマンドを見つけるには、Windows PowerShell ウィンドウで次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   help *adapter*
   ```

   > **注:** **Get-NetAdapter** コマンドに注意してください。

1. コマンドのヘルプを確認するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   help Get-NetAdapter 
   ```

    > **注:** *–Physical* パラメーターに注意してください。

1. リモート処理を使用して **LON-DC1** と **LON-CL1** でコマンドを実行するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Invoke-Command –ComputerName LON-CL1,LON-DC1 –ScriptBlock { Get-NetAdapter –Physical }
   ```

### <a name="task-2-compare-the-output-of-a-local-command-to-that-of-a-remote-command"></a>タスク 2: ローカル コマンドの出力をリモート コマンドのものと比較する

1. **Process** オブジェクトのメンバーを確認するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-Process | Get-Member
   ```

1. リモートの Process オブジェクトのメンバーを確認するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Invoke-Command –ComputerName LON-DC1 –ScriptBlock { Get-Process } | Get-Member
   ```

   > **注:** 2 番目の結果セットには、2 つの **MemberType** **メソッド GetType** と **ToString** のみが含まれます。 これは、ローカル出力と比較した場合、リモート値の **TypeName** が逆シリアル化されるためです。

## <a name="exercise-4-using-implicit-remoting"></a>演習 4: 暗黙的なリモート処理の使用

### <a name="task-1-create-a-persistent-remoting-connection-to-a-server"></a>タスク 1: サーバーへの永続的なリモート処理接続を作成する

1. **LON-CL1** で、パスワード **Pa55w.rd** を使用して **Adatum\\Administrator** としてサインインしていることを確かめます。
1. Windows PowerShell ウィンドウが閉じている場合は、 **[スタート]** メニューを選択してから、「**powersh**」と入力します。
1. 結果リストで、 **[Windows PowerShell]** を右クリックするか、そのコンテキスト メニューをアクティブにしてから、 **[管理者として実行]** を選択します。
1. Windows PowerShell コマンド ウィンドウで、**LON-DC1** への永続的な接続を作成し、それを変数に格納します。 次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $dc = New-PSSession –ComputerName LON-DC1
   ```

1. 変数の PSSession リストを確認するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   $dc
   ```

   > **注:** 接続が使用可能であることを確認します。

### <a name="task-2-import-and-use-a-module-from-a-server"></a>タスク 2: サーバーからモジュールをインポートして使用する

1. **LON-DC1** のモジュールのリストを表示するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-Module –ListAvailable –PSSession $dc
   ```

1. サーバー メッセージ ブロック (SMB) 共有で使用できる **LON-DC1** のモジュールを見つけるには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-Module –ListAvailable –PSSession $dc | Where { $_.Name –Like '*share*' }
   ```

1. **LON-DC1** からローカル コンピューターにモジュールをインポートし、重要なコマンドの名詞にプレフィックス **DC** を追加するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Import-Module –PSSession $dc –Name SMBShare –Prefix DC
   ```

1. **LON-DC1** の共有のリストを表示するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-DCSMBShare
   ```

   > **注:** このコマンドは **LON-DC1** で暗黙的に実行されるため、コマンドではそのコンピューター上の共有が表示されます。

1. ローカル コンピューターの共有のリストを表示するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-SMBShare
   ```

   > **注:** インポートされたコマンドに DC プレフィックスを追加したので、ローカル コマンドは引き続き元の名前で使用できます。

### <a name="task-3-close-all-open-remoting-connections"></a>タスク 3: 開いているすべてのリモート処理接続を閉じる

1. Windows PowerShell コマンド ウィンドウで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-PSSession | Remove-PSSession
   ```

2. リモート処理セッションが閉じられていることを確認するコマンドが、**E:\\Mod08\\Labfiles\\ImplicitRemoting.ps1.txt** で提供されるサンプルの応答スクリプトで明示的に呼び出されていないことに注意してください。 リモート処理接続が閉じられていることを確認するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-PSSession
   ```

   > **注:** 返されたセッションがないことを確認します。

## <a name="exercise-5-managing-multiple-computers"></a>演習 5: 複数のコンピューターの管理

### <a name="task-1-create-pssessions-to-two-computers"></a>タスク 1: 2 台のコンピューターに対する PSSession を作成する

1. 引き続き、パスワード **Pa55w.rd** を使用して、**Adatum\\Administrator** として **LON-CL1** にサインインしていることを確かめます。

2. まだ開いていない場合は、Windows PowerShell を開きます。

3. **LON-CL1** と **LON-DC1** に対して PSSession を作成し、それらを変数に保存するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   $computers = New-PSSession –ComputerName LON-CL1,LON-DC1
   ```

4. 接続を確認するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   $computers
   ```

   > **注:** 2 つの接続が使用可能と表示されていることを確認します。

### <a name="task-2-create-a-report-that-displays-windows-firewall-rules-from-two-computers"></a>タスク 2: 2 台のコンピューターの Windows ファイアウォール規則を表示するレポートを作成する

1. ネットワーク セキュリティを使用できるモジュールを見つけるには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-Module *security* –ListAvailable
   ```

1. リストの **Net-Security** モジュールに注意してください。
1. **LON-CL1** と **LON-DC1** でモジュールをメモリに読み込むには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Invoke-Command –Session $computers –ScriptBlock { Import-Module NetSecurity }
   ```

1. Windows ファイアウォール規則を表示できるコマンドを見つけるには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-Command –Module NetSecurity
   ```

   > **注:** **Get-NetFirewallRule** コマンドを確認してください。

1. コマンドのヘルプを確認する場合は、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Help Get-NetFirewallRule -ShowWindow
   ```

   > **注:** ヘルプが正しく表示されない場合は、Windows PowerShell ISE ではなく、管理者として Windows PowerShell コマンド ウィンドウで、手順 1 から 3 のコマンドを実行します。

1. **LON-DC1** と **LON-CL1** で有効になっているファイアウォール規則のリストを表示するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Invoke-Command –Session $computers –ScriptBlock { Get-NetFirewallRule –Enabled True } | Select Name,PSComputerName
   ```

1. **LON-DC1** と **LON-CL1** でモジュールをアンロードするには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Invoke-Command –Session $computers –ScriptBlock { Remove-Module NetSecurity }
   ```

### <a name="task-3-create-and-display-an-html-report-that-displays-local-disk-information-from-two-computers"></a>タスク 3: 2 台のコンピューターからのローカル ディスク情報を表示する HTML レポートを作成して表示する

1. ドライブの種類が **3** のもののみを含めるようにフィルター処理されたローカル ハード ドライブのリストを表示するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-WmiObject –Class Win32_LogicalDisk –Filter "DriveType=3"
   ```

1. リモート処理を使用して **LON-DC1** と **LON-CL1** で同じコマンドを実行するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Invoke-Command –Session $computers –ScriptBlock { Get-WmiObject –Class Win32_LogicalDisk –Filter "DriveType=3" }
   ```

   > **注:** レポートには、各コンピューターの名前、各ドライブの文字、および各ドライブの空き領域と合計サイズ (バイト単位) を含める必要があります。

1. 前のコマンドの結果を含む HTML レポートを生成するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Invoke-Command –Session $computers –ScriptBlock { Get-WmiObject –Class Win32_LogicalDisk –Filter "DriveType=3" } | ConvertTo-Html –Property PSComputerName,DeviceID,FreeSpace,Size
   ```

### <a name="task-4-close-all-open-pssessions"></a>タスク 4: 開いているすべての PSSession を閉じる

- 開いているすべての PSSession を閉じるには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-PSSession | Remove-PSSession
   ```
