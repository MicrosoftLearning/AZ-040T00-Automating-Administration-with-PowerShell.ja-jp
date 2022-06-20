---
lab:
  title: 'ラボ: Windows PowerShell の構成およびコマンドの検索と実行'
  type: Answer Key
  module: 'Module 1: Getting Started with Windows PowerShell'
ms.openlocfilehash: 7fbafd34f2836b39004834e68e625c8a0570f3a4
ms.sourcegitcommit: a95a9bb3a7919b785df0574c3407f4b6c3bea9f5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2021
ms.locfileid: "132116742"
---
# <a name="lab-configuring-windows-powershell-and-finding-and-running-commands"></a>ラボ: Windows PowerShell の構成およびコマンドの検索と実行

## <a name="exercise-1-configuring-the-windows-powershell-console-application"></a>演習 1: Windows PowerShell コンソール アプリケーションの構成

### <a name="task-1-start-the-console-application-as-administrator-and-pin-the-windows-powershell-icon-to-the-taskbar"></a>タスク 1: 管理者としてコンソール アプリケーションを起動し、Windows PowerShell アイコンをタスク バーにピン留めする

1. **LON-CL1** で、 **[開始]** を選びます。
1. 「**powershell**」と入力して、Windows PowerShell アイコンを表示します。 アイコン名が **Windows PowerShell (x86)** ではなく **Windows PowerShell** と表示されていることを確認します。
1. **[Windows PowerShell]** を右クリックするか、そのコンテキスト メニューをアクティブにしてから、 **[管理者として実行]** を選びます。
1. ウィンドウのタイトル バーに **"管理者"** と表示され、 **"(x86)"** というテキストが含まれていないことを確認します。 これは、このアプリケーションが 64 ビット コンソール アプリケーションであり、管理者が実行していることを示しています。
1. タスク バーにある **Windows PowerShell** アイコンを右クリックするか、コンテキスト メニューを開いて **[タスク バーにピン留めする]** を選びます。 これで Windows PowerShell コンソールが開き、**管理者** によって実行され、今後もタスク バーで使用できるようになりました。

### <a name="task-2-configure-the-windows-powershell-console-application"></a>タスク 2: Windows PowerShell コンソール アプリケーションを構成する

1. **Consolas** フォントを使うように Windows PowerShell を構成するには:

   a.   **Windows PowerShell** コンソール ウィンドウの左上隅にあるコントロール ボックスを選びます。

   b.   **[プロパティ]** を選択します。

   c.    **[Windows PowerShell のプロパティ]** ダイアログ ボックスで **[フォント]** タブを選び、 **[フォント]** リストから **[Consolas]** を選びます。

   d.   **[サイズ]** リストから **[16]** を選びます。

1. 別の表示色を選ぶには、 **[色]** タブで使用できる **[画面の文字]** と **[画面の背景]** の色を確認します。

    > **注:** さまざまな組み合わせを試してください。 カラー ピッカーを使うと、簡単に色を変更し、読みやすくすることができます。

1. ウィンドウのサイズを変更し、水平スクロール バーを削除するには:

   a.   **[レイアウト]** タブの **[ウィンドウのサイズ]** 設定で、**Windows PowerShell** コンソール ペインのプレビューが **[ウィンドウのプレビュー]** 領域に完全に収まるまで、領域の **[幅]** と **[高さ]** の値を変更します。

   b.   **[レイアウト]** タブの **[画面バッファーの** **サイズ]** 設定で、 **[幅]** の値を **[ウィンドウのサイズ]** 設定の **[幅]** の値と同じになるように変更します。

1. **[OK]** を選択します。 これでコンソール アプリケーションを使う準備は完了です。

### <a name="task-3-start-a-shell-transcript"></a>タスク 3: シェル トランスクリプトを開始する

- Windows PowerShell コンソールで、次のコマンドを入力し、Enter キーを押します。

   ```ps
   Start-Transcript C:\DayOne.txt
   ```

    > **注:** これで、Windows PowerShell セッションのトランスクリプトが開始されました。 **Stop-Transcript** を実行するか、Windows PowerShell ウィンドウを閉じるまで、入力したすべてのコマンドとコマンド出力がテキスト ファイルに保存されます。 トランスクリプトの内容は、いつでも **C:\DayOne.txt** を開いて確認できます。

### <a name="exercise-1-results"></a>演習 1 の結果

この演習を完了すると、Windows PowerShell コンソール アプリケーションを開き、その外観とレイアウトを構成できるようになります。

## <a name="exercise-2-configuring-the-windows-powershell-ise-application"></a>演習 2: Windows PowerShell ISE アプリケーションの構成

### <a name="task-1-open-the-windows-powershell-ise-application-as-administrator"></a>タスク 1: 管理者として Windows PowerShell ISE アプリケーションを開く

1. Windows PowerShell コンソールで「**ise**」と入力し、Enter キーを押します。

   > **注:** ISE を開くこの方法は、管理者がコンソールを実行している場合にのみ正しく機能します。

1. ISE ウィンドウを閉じます。

1. タスクバーにある **Windows PowerShell** アイコンを右クリックするか、コンテキスト メニューを起動して、 **[ISE を管理者として実行する]** を選びます。 これで、**管理者** として Windows PowerShell ISE が実行されます。

### <a name="task-2-customize-the-appearance-of-the-ise-to-use-the-single-pane-view-hide-the-command-pane-and-adjust-the-font-size"></a>タスク 2: ISE の外観をカスタマイズし、単一枠ビュー、コマンド ペインの非表示、フォント サイズの調整を行う

1. 単一枠ビューを使うように ISE を構成するには:

    a. Windows PowerShell ISE ツール バーで **[スクリプト ウィンドウを最大表示]** オプションを選びます。
    
    b. **[スクリプト ウィンドウを非表示にします]** の上矢印アイコンを選び、コンソールを表示します。

1. **[コマンド]** ペインが表示されていない場合は、 **[コマンド アドオンを** **表示]** オプションを選んで表示します。

1. **[コマンド]** ペインを非表示にするには、 **[コマンド アドオンを表示]** オプションを選びます。

1. ウィンドウの右下隅にあるスライダーを使って、快適な見え方になるまでフォント サイズを調整します。

1. Windows PowerShell ISE と Windows PowerShell のウィンドウを閉じます。

### <a name="exercise-2-results"></a>演習 2 の結果

この演習を完了すると、Windows PowerShell 統合スクリプト環境 (ISE) アプリケーションの外観をカスタマイズできるようになります。

## <a name="exercise-3-finding-and-running-windows-powershell-commands"></a>演習 3: Windows PowerShell コマンドの検索と実行

### <a name="task-1-find-commands-that-will-accomplish-specified-tasks"></a>タスク 1: 指定したタスクを実行するコマンドを見つける

1. **LON-CL1** のタスク バーにある **Windows PowerShell** を右クリックし、 **[管理者として実行]** を選びます。

1. コンソールで次のいずれかのコマンドを入力し、Enter キーを押します。

   ```ps
   Get-Help *resolve* 
   ```

   または

   ```ps
   Get-Command *resolve* 
   ```

   または

   ```ps
   Get-Command -Verb resolve 
   ```

   > **注:** 最初の 2 つのコマンドを使うと、名前のどこかに *Resolve* が使われているコマンドの一覧が表示されます。 3 つ目を使うと、名前の中に動詞 *Resolve* が使われているコマンドの一覧が表示されます。 このラボ環境では、この 3 つのコマンドから同じ結果が返されます。 これで **Resolve-DNSName** コマンドが見つかります。

1. コンソールで次のいずれかのコマンドを入力し、Enter キーを押します。

   ```ps
   Get-Help *adapter* 
   ```

   または

   ```ps
   Get-Command *adapter* 
   ```

   または

   ```ps
   Get-Command -Noun *adapter*
   ```

   または

   ```ps
   Get-Command -Verb Set -Noun *adapter* 
   ```

   > **注:** 最初の 3 つのコマンドを使うと、名前に *Adapter* が使われているコマンドの一覧が表示されます。 4 つ目を使うと、名前に *Adapter* を含み、*Set* 動詞が使われているコマンドの一覧が表示されます。 これで **Set-NetAdapter** コマンドが見つかります。

1. **Get-Help Set-NetAdapter** を実行して、そのコマンドのヘルプを確認します。 これで、 *-MACAddress* パラメーターが見つかります。

1. コンソールで次のいずれかのコマンドを入力し、Enter キーを押します。

   ```ps
   Get-Help *sched* 
   ```

   または

   ```ps
   Get-Command *sched* 
   ```

   または

   ```ps
   Get-Module *sched* -ListAvailable
   ```

   > **注:** 最初の 2 つのコマンドを使うと、名前に *Sched* が使われているコマンドの一覧が表示されます。 3 つ目を使うと、名前に *Sched* を含むモジュールの一覧が表示されます。これで、モジュール **ScheduledTasks** が見つかります。 次にコマンド `Get-Command -Module \*ScheduledTask\*` を実行すると、そのモジュールのコマンドの一覧が表示されます。 これで **Enable-ScheduledTask** コマンドが見つかります。

1. コンソールで次のいずれかのコマンドを入力し、Enter キーを押します。

   ```ps
   Get-Command –Verb Block 
   ```

   または

   ```ps
   Get-Help *block* 
   ```

   > **注:** これらを使うと、コマンドの一覧が表示されます。これで **Block-SmbShareAccess** コマンドが見つかります。 次に **Get-Help Block-SmbShareAccess** を実行すると、このコマンドによってファイル共有の随意アクセス制御リスト (DACL) に Deny エントリが適用されることがわかります。

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```ps
   Get-Help *branch*
   ```

   > **注:** この結果、名前に *branch* が使われているコマンドがないため、ヘルプ システムで全文検索が行われます。

   *branch* というテキストを含むトピックの一覧が表示されますが、**BranchCache** キャッシュのクリアに関連するものはありません。

1. コンソールで次のいずれかのコマンドを入力し、Enter キーを押します。

   ```ps
   Get-Help *cache* 
   ```

   または

   ```ps
   Get-Command *cache* 
   ```

   または

   ```ps
   Get-Command -Verb clear
   ```

   > **注:** 最初の 2 つのコマンドを使うと、名前に *Cache* を含むコマンドの一覧が表示されます。 3 つ目を使うと、名前に動詞 *Clear* が使われているコマンドの一覧が表示されます。 いずれの方法でも、**Clear-BCCache** コマンドが見つかります。

1. コンソールで次のいずれかのコマンドを入力し、Enter キーを押します。

   ```ps
   Get-Help *firewall*
   ```

   または

   ```ps
   Get-Command *firewall* 
   ```

   または

   ```ps
   Get-Help *rule*
   ```

   または

   ```ps
   Get-Command *rule* 
   ```

   > **注:** 名前にこれらの単語が使われているコマンドの一覧が表示されるので、**Get-NetFirewallRule** コマンドが見つかります。

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```ps
   Get-Help Get-NetFirewallRule –Full
   ```

   > **注:** これでコマンドのヘルプが表示されるので、 *-Enabled* パラメーターが見つかります。

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```ps
   Get-Help *address* 
   ```

   > **注:** 名前に *address* が使われているコマンドの一覧が表示されます。 これで **Get-NetIPAddress** コマンドが見つかります。

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```ps
   Get-Command –Verb suspend
   ```

   > **注:** 名前に動詞 *Suspend* が使われているコマンドの一覧が表示されます。 これで **Suspend-PrintJob** コマンドが見つかります。

1. コンソールで次のいずれかのコマンドを入力し、Enter キーを押します。

   ```ps
   Get-Alias Type
   ```

   または

   ```ps
   Get-Command –Noun *content* 
   ```

   > **注:** 最初のコマンドを使うと、**Type** コマンドの別名の定義が表示されます。これは **cmd.exe** でファイルからテキストを確認するために使われるコマンドです。 2 つ目のコマンドを使うと、名前の名詞部分に *Content* を含むコマンドの一覧が表示されます。 これで **Get-Content** コマンドが見つかります。

### <a name="task-2-run-commands-to-accomplish-specified-tasks"></a>タスク 2: コマンドを実行して指定されたタスクを実行する

1. **Adatum\\Administrator** として **LON-CL1** 仮想マシンにサインインしている必要があります。

2. 有効になっているファイアウォール規則の一覧を表示するには、次のコマンドを入力してから、Enter キーを押します。

   ```ps
   Get-NetFirewallRule -Enabled True
   ```

3. すべてのローカル IPv4 アドレスの一覧を表示するには、次のコマンドを入力してから、Enter キーを押します。

   ```ps
   Get-NetIPAddress –AddressFamily IPv4
   ```

4. バックグラウンド インテリジェント転送サービス (BITS) のスタートアップの種類を設定するには、コンソールで次のコマンドを入力し、Enter キーを押します。

   ```ps
   Set-Service –Name BITS –StartupType Automatic 
   ```

5. **LON-DC1** への接続をテストするには、コンソールで次のコマンドを入力し、Enter キーを押します。

   ```ps
   Test-Connection –ComputerName LON-DC1 –Quiet
   ```

   > **注:** このコマンドからは True または False の値のみが返されます。他の出力は含まれません。

6. Security イベント ログから最新の 10 エントリを表示するには、コンソールで次のコマンドを入力し、Enter キーを押します。

   ```ps
   Get-EventLog –LogName Security –Newest 10 
   ```

### <a name="exercise-3-results"></a>演習 3 の結果

この演習を完了すると、特定のタスクを実行する Windows PowerShell コマンドを見つけて実行できるようになります。

## <a name="exercise-4-using-about-files"></a>演習 4: About ファイルの使用

### <a name="task-1-locate-and-review-about-help-files"></a>タスク 1: About ヘルプ ファイルを見つけて確認する

1. 前の演習で **Adatum\\Administrator** として **LON-CL1** にサインインしたままであることを確認します。

1. ワイルドカード文字列の比較に使われる演算子を見つけるには、コンソールで次のコマンドを入力し、Enter キーを押します。

   ```ps
   Get-Help *comparison*
   ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```ps
   Get-Help about_comparison_operators -ShowWindow
   ```

   モーダル ウィンドウに表示される **about_Comparison_Operators** ヘルプの **-Like** 演算子に注目してください。

1. **-Like** 演算子を見つけるには、**[検索]** テキストボックスに「**wildcard**」と入力し、**[次へ]** を選びます。

1. **about_Comparison_Operators** ファイルを確認すると、一般的な演算子では大文字と小文字が区別されないことがわかります。 **about_Comparison_Operators** で大文字と小文字を区別する特定の演算子が用意されています。

1. **COMPUTERNAME** 環境変数を表示するには、コンソールで次のコマンドを入力し、Enter キーを押します。

   ```ps
   $env:computername
   ```

   この手法については、**about_environment_variables** で学ぶことができます。

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```ps
   Get-Help *signing*
   ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```ps
   Get-Help about_signing 
   ```

1. コード署名について説明します。 **makecert.exe** が自己署名デジタル証明書を作成するために使われることを学ぶ必要があります。

### <a name="exercise-4-results"></a>演習 4 の結果

この演習を完了すると、About ファイル内のヘルプ コンテンツを見つけられるようになります。
