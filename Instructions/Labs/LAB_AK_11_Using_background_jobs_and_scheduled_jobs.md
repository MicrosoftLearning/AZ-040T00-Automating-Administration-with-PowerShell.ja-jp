---
lab:
  title: 'ラボ: PowerShell を使用したジョブ管理'
  type: Answer Key
  module: 'Module 11: Using background jobs and scheduled jobs'
---

# PowerShell を使用したジョブ管理

このラボは完了するまで、約 **15** 分かかります。

## 演習 1:ジョブの開始と管理

### タスク 1: Windows PowerShell リモート ジョブを開始する

1. **LON-CL1** に **Adatum\\Administrator** として、パスワード **Pa55w.rd** を使用して確実にサインインします。
1. **[スタート]** を選択して、「**powersh**」と入力します。
1. 結果リストで、**[Windows PowerShell]** を右クリックするか、そのコンテキスト メニューをアクティブにしてから、**[管理者として実行]** を選択します。
1. 物理ネットワーク アダプターの一覧を **LON-DC1** および **LON-SVR1** から取得する Windows PowerShell リモート ジョブを開始するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Invoke-Command –ScriptBlock { Get-NetAdapter –Physical } –ComputerName LON-DC1,LON-SVR1 –AsJob –JobName RemoteNetAdapt
   ```

1. サーバー メッセージ ブロック (SMB) 共有の一覧を **LON-DC1** および **LON-SVR1** から取得する Windows PowerShell リモート ジョブを開始するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Invoke-Command –ScriptBlock { Get-SMBShare } –ComputerName LON-DC1,LON-SVR1 –AsJob –JobName RemoteShares
   ```

1. **Win32_Volume** クラスのすべてのインスタンスを Active Directory Domain Services (AD DS) 内のすべてのコンピューターから取得する Windows PowerShell リモート ジョブを開始するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Enable-PSRemoting –SkipNetworkProfileCheck –Force
   Invoke-Command –ScriptBlock { Get-CimInstance –ClassName Win32_Volume } –ComputerName (Get-ADComputer –Filter * | Select –Expand Name) –AsJob –JobName RemoteDisks
   ```

> **注:** PowerShell リモート処理を使用して接続するには、LON-CL1 で PowerShell リモート処理を有効にする必要があります。Windows 10 では既定で無効になっています。 **RemoteDisk** は、LON-CL1 を含むすべてのドメイン コンピューターを対象とします。

> **注:** ドメイン内の一部のコンピューターがオンラインでない可能性があるため、このジョブは正常に終了しない場合があります。 これは正しい動作です。

### タスク 2: ローカル ジョブを開始する

1. セキュリティ イベント ログからすべてのエントリを取得するローカル ジョブを開始するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Start-Job –ScriptBlock { Get-EventLog –LogName Security } –Name LocalSecurity
   ```

1. 100 件のディレクトリの一覧を生成するローカル ジョブを開始するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Start-Job –ScriptBlock { 1..100 | ForEach-Object { Dir C:\ -Recurse } } –Name LocalDir
   ```

> **注:** この操作は完了するまで時間がかかる場合があります。 完了するまで待たないでください。 次のタスクに進みます。

### タスク 3: ジョブの状態を確認して管理する

1. **LON-CL1** に **Adatum\\Administrator** として、パスワード **Pa55w.rd** を使用してサインインしていることを確かめます。
1. 実行中のジョブの一覧を表示するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-Job
   ```

1. 名前が **remote** で始まる実行中のジョブの一覧を表示するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-Job –Name Remote*
   ```

1. **LocalDir** ジョブを停止するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Stop-Job –Name LocalDir
   ```

1. 状態が **[実行中]** であるジョブが存在しなくなるまで **Get-Job** を実行します。
1. **RemoteNetAdapt** ジョブの結果を受け取るには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Receive-Job –Name RemoteNetAdapt
   ```

1. **RemoteDisks** ジョブの結果を受け取るには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-Job –Name RemoteDisks –IncludeChildJob | Receive-Job
   ```

## 演習 2:スケジュールされたジョブの作成

### タスク 1: ジョブ オプションとジョブ トリガーを作成する

1. **LON-CL1** に **Adatum\\Administrator** として、パスワード **Pa55w.rd** を使用してサインインしていることを確かめます。
1. 新しいジョブ オプションを作成するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $option = New-ScheduledJobOption –WakeToRun -RunElevated
   ```

1. 新しいジョブ トリガーを作成するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $trigger1 = New-JobTrigger –Once –At (Get-Date).AddMinutes(5)
   ```

### タスク 2: スケジュールされたジョブを作成し、結果を取得する

1. ジョブを登録するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Register-ScheduledJob –ScheduledJobOption $option `
   –Trigger $trigger1 `
   –ScriptBlock { Get-EventLog –LogName Security } `
   –Name LocalSecurityLog
   ```

1. ジョブ トリガーの一覧を表示するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-ScheduledJob –Name LocalSecurityLog | 
   Select –Expand JobTriggers 
   ```

1. 表示される時間に注目し、手順 2 の出力で返された時間が経過するまで待機します。
1. ジョブの一覧を表示するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-Job
   ```

1. ジョブの結果を受け取るには、**LocalSecurityLog** ジョブを登録してから 5 分経過するのを待ってから、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Receive-Job –Name LocalSecurityLog
   ```

> **注:** 出力がログ エントリの一覧で構成されていることを確認します。

### タスク 3: スケジュールされたタスクとして Windows PowerShell スクリプトを使用する

1. コンソール セッションを **LON-DC1** に切り替え、必要に応じてパスワード **Pa55w.rd** を使用して **Adatum\\Administrator** として **LON-DC1** にサインインします。
1. **LON-DC1** の**サーバー マネージャー**で、**[ツール]** を選択し、次に **[Active Directory ユーザーとコンピューター]** を選択します。
1. **[Active Directory ユーザーとコンピューター]** のコンソール ツリーで、**Adatum.com** を選択して展開し、次に **[マネージャー]** を選択します。
1. **[マネージャー]** の詳細ウィンドウで、いずれかのユーザー アカウント (**Adam Hobbs**など) を選択します。 アカウントを右クリックするか、そのコンテキスト メニューをアクティブにしてから、**[アカウントを無効にする]** を選択します。 
1. 無効にしたユーザーをダブルクリックするか、それを選択して Enter キーを押します。
1. **[所属するグループ]** タブを選択し、ユーザーがマネージャー セキュリティ グループのメンバーであることを確認します。
1. **[OK]** を選択し、次に **[Active Directory ユーザーとコンピューター]** を最小化します。
1. **[スタート]** を選択し、「**タスク スケジューラ**」と入力して、**[タスク スケジューラ]** を選択します。
1. **[タスク スケジューラ]** のコンソール ツリーで、**[タスク スケジューラ (ローカル)]** を右クリックするか、そのコンテキスト メニューをアクティブにしてから、**[タスクの作成]** を選択します。
1. **[タスクの作成]** ウィンドウの **[全般]** タブにある **[名前]** および **[説明]** ボックスに、「**無効化されたユーザーをセキュリティ グループから削除する**」と入力します。 
1. **[セキュリティ オプション]** セクションで、**[ユーザーがログオンしているかどうかにかかわらず実行する]** を選択し、次に **[最上位の特権で実行する]** チェック ボックスをオンにします。
1. **[トリガー]** タブで、**[新規]** を選択します。
1. **[新しいトリガー]** ウィンドウの **[設定]** で **[毎日]** を選択し、**[開始] 時刻**ボックスで、時刻を現在の時刻から 5 分後に変更し、**[OK]** を選択します。

   > **注:** **[タスク スケジューラ]** ポップアップ ウィンドウが表示されたら、**[OK]** を選択します。

1. **[操作]** タブで **[新規]** を選択します。
1. **[新しい操作]** ウィンドウで、**[プログラム/スクリプト]** ボックスに「**PowerShell.exe**」と入力します。
1. **[引数の追加 (オプション)]** ボックスに「**-ExecutionPolicy Bypass E:\\Labfiles\\\Mod11\\DeleteDisabledUserManagersGroup.ps1**」と入力し、**[OK]** を選択します。次に、ポップアップ ウィンドウで **[はい]** を選択します。
1. **[条件]** タブでは、項目を確認しますが、変更はしません。
1. **[設定]** タブの **[タスクが既に実行中の場合に適用される規則]** のドロップダウン リストで、**[既存のインスタンスを停止する]** を選択し、次に **[OK]** を選択します。
1. **[タスク スケジューラ]** の資格情報ポップアップ ウィンドウで、**[パスワード]** ボックスに「**Pa55w.rd**」と入力し、次に **[OK]** を選択します。
1. **[タスク スケジューラ]** で、**[タスク スケジューラ ライブラリ]** を選択し、次に詳細ウィンドウで **[Managers セキュリティ グループから、無効にされたユーザーを削除する]** を選択します。
1. 詳細ウィンドウで、**[履歴]** タブを選択します。5 分が経過したら、**[操作]** ウィンドウで **[最新の情報に更新]** を選択します。 **[タスクのカテゴリ]** が **[タスクが完了しました]** である項目が表示されるのがわかります。
1. **[Active Directory ユーザーとコンピューター]** を最大化します。
1. 無効にしたユーザーをダブルクリックするか、それを選択して Enter キーを押します。
1. **[所属するグループ]** タブを選択します。ユーザーは、マネージャー セキュリティ グループのメンバーではなくなっています。
