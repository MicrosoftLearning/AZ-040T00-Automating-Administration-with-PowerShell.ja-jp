---
lab:
  title: 'ラボ: Windows PowerShell の構成およびコマンドの検索と実行'
  module: 'Module 1: Getting Started with Windows PowerShell'
ms.openlocfilehash: fcf09d463080430d0f294b9f328d1f4700d6998f
ms.sourcegitcommit: 9c31a6ab628c30fac88ec9070c3d807f2a9bbfdb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2022
ms.locfileid: "146824959"
---
# <a name="lab-configuring-windows-powershell-and-finding-and-running-commands"></a>ラボ: Windows PowerShell の構成およびコマンドの検索と実行

## <a name="scenario"></a>シナリオ

あなたは管理者であり、多くの管理タスクを自動化するために Windows PowerShell を使用しているとしましょう。 適切な Windows PowerShell ホスト アプリケーションを正常に起動できること、また外観をカスタマイズすることにより、将来の使用に向けて、それらのアプリケーションを構成できることを保証する必要があります。

さらに、Windows PowerShell を使用していくつかの管理タスクを完了する準備も行っています。 あなたは、これらのタスクを実行するために使用するコマンドを検出し、いくつかのコマンドを実行してこれらのタスクの実行を開始し、これらのタスクを完了できるようにする新しい Windows PowerShell 機能について学習する必要があります。

## <a name="objectives"></a>目標

このラボを完了すると、次のことができるようになります。

- Windows PowerShell コンソール アプリケーションを開いて構成する。
- Windows PowerShell ISE アプリケーションを開いて構成する。
- Windows PowerShell コマンドを見つけて実行する。
- Windows PowerShell のヘルプと概要のトピックを参照して、シェルの新しい概念と手法について学習する。

## <a name="estimated-time"></a>推定時間

約 60 分

## <a name="lab-setup"></a>ラボのセットアップ

仮想マシン: **AZ-040T00A-LON-DC1**、**AZ-040T00A-LON-CL1**

ユーザー名: **Adatum\\Administrator**

パスワード: **Pa55w.rd**

## <a name="lab-startup"></a>ラボのスタートアップ

1. **[LON-DC1]** を選択します。
1. 次の資格情報を使用してサインインします。
   - ユーザー名: **Administrator**
   - パスワード: **Pa55w.rd**
   - ドメイン: **Adatum**

1. **LON-CL1** についてこれらの手順を繰り返します。

## <a name="exercise-1-configuring-the-windows-powershell-console-application"></a>演習 1: Windows PowerShell コンソール アプリケーションの構成

### <a name="exercise-scenario-1"></a>演習のシナリオ 1

Windows PowerShell をカスタマイズするには、最初にコンソールに変更を加える必要があります。 この演習では、Windows PowerShell コンソール アプリケーションを開き、その外観とレイアウトを構成します。

この演習の主なタスクは次のとおりです。

1. 管理者としてコンソール アプリケーションを起動し、Windows PowerShell アイコンをタスク バーにピン留めする。
1. Windows PowerShell コンソール アプリケーションを構成する。
1. シェル トランスクリプトを開始します。

### <a name="task-1-start-the-console-application-as-administrator-and-pin-the-windows-powershell-icon-to-the-taskbar"></a>タスク 1: 管理者としてコンソール アプリケーションを起動し、Windows PowerShell アイコンをタスク バーにピン留めする

1. **LON-CL1** で、**管理者** として **Windows PowerShell** アプリケーションを起動します。 ウィンドウのタイトル バーに "**管理者**" と表示され、"(x86)" というテキストが含まれていないことを確認します。 これは、このアプリケーションが 64 ビット コンソール アプリケーションであり、管理者がそれを実行していることを示しています。
1. Windows PowerShell アイコンをタスク バーにピン留めします。

### <a name="task-2-configure-the-windows-powershell-console-application"></a>タスク 2: Windows PowerShell コンソール アプリケーションを構成する

1. **[Windows PowerShell のプロパティ]** を開き、ポイント サイズが **[16]** の **[Consolas]** を使用するように Windows PowerShell を構成します。
2. **[色]** タブで、プライマリ テキストと背景の代替の表示色を選択します。
3. **[レイアウト]** タブで、ウィンドウのサイズを画面に収まるサイズに設定し、水平スクロール バーを削除します。

### <a name="task-3-start-a-shell-transcript"></a>タスク 3: シェル トランスクリプトを開始する

1. **[Windows PowerShell]** コンソールで、次のコマンドを入力し、Enter キーを押します。

   ```ps
   Start-Transcript C:\DayOne.txt
   ```

> **注:** これで、Windows PowerShell セッションのトランスクリプトが開始されました。 **Stop-Transcript** を実行するか、 **[Windows PowerShell]** ウィンドウを閉じるまで、入力したすべてのコマンドとコマンド出力がテキスト ファイルに保存されます。 トランスクリプトの内容は、いつでも **C:\DayOne.txt** を開いて確認できます。

### <a name="exercise-1-results"></a>演習 1 の結果

この演習を完了すると、Windows PowerShell コンソール アプリケーションを開き、その外観とレイアウトを構成できるようになります。

## <a name="exercise-2-configuring-the-windows-powershell-ise-application"></a>演習 2: Windows PowerShell ISE アプリケーションの構成

### <a name="exercise-scenario-2"></a>演習のシナリオ 2

この演習では、Windows PowerShell ISE アプリケーションの外観をカスタマイズします。

この演習の主なタスクは次のとおりです。

1. 管理者として Windows PowerShell ISE アプリケーションを開く。
1. 単一枠ビューを使用するように ISE の外観をカスタマイズし、 **[コマンド]** ウィンドウを非表示にし、フォント サイズを調整します。

### <a name="task-1-open-the-windows-powershell-ise-application-as-administrator"></a>タスク 1: 管理者として Windows PowerShell ISE アプリケーションを開く

1. タスクバーの **Windows PowerShell** アイコンを右クリックするか、コンテキスト メニューをアクティブ化し、**管理者** として **Windows PowerShell ISE** アプリケーションを開きます。

### <a name="task-2-customize-the-appearance-of-the-ise-to-use-the-single-pane-view-hide-the-command-pane-and-adjust-the-font-size"></a>タスク 2: ISE の外観をカスタマイズし、単一枠ビュー、コマンド ペインの非表示、フォント サイズの調整を行う

1. 単一枠ビューを使用し、コンソール ウィンドウを表示するように ISE を構成します。
2. **[コマンド]** ウィンドウを非表示にします。
3. 見やすくなるようにフォント サイズを調整します。
4. **Windows PowerShell ISE** と **Windows PowerShell** のウィンドウを閉じます。

### <a name="exercise-2-results"></a>演習 2 の結果

この演習を完了すると、Windows PowerShell ISE アプリケーションの外観をカスタマイズできるようになります。

## <a name="exercise-3-finding-and-running-windows-powershell-commands"></a>演習 3: Windows PowerShell コマンドの検索と実行

### <a name="exercise-scenario-3"></a>演習のシナリオ 3

この演習では、Windows PowerShell の **Get-Help** および **Get-Command** コマンドを使用して、Windows PowerShell 内で特定のタスクを完了できる新しいコマンドを検出します。 また、いくつかの基本的な Windows PowerShell コマンドを実行します。 場合によっては、タスクを完了させるために使用するコマンドを見つける必要が生じることがあります。

この演習の主なタスクは次のとおりです。

1. 指定したタスクを遂行するコマンドを見つける。
1. コマンドを実行して指定したタスクを遂行する。

### <a name="task-1-find-commands-thatll-accomplish-specified-tasks"></a>タスク 1: 指定したタスクを実行するコマンドを見つける

- **LON-CL1** で、**Adatum\\Administrator** としてサインインしていることを確認し、次の質問に対する回答を決定します。

  - DNS 名を解決するために実行するコマンドは何か?
  - ネットワーク アダプターに変更を加えるために実行するコマンドは何か? このようなコマンドを見つけた後、(メディア アクセス制御 (MAC) アドレスに対する変更をサポートするアダプターで) MAC アドレスを変更するために使用するパラメーターは何か?
  - 以前に無効にした、スケジュールされたタスクを有効にするコマンドは何か?
  - 特定のユーザーによるファイル共有へのアクセスをブロックするために実行するコマンドは何か?
  - コンピューターのローカルの **BranchCache** キャッシュをクリアするために実行するコマンドは何か?
  - Windows ファイアウォール規則の一覧を表示するために実行するコマンドは何か? 有効にされた規則のみを表示する、そのコマンドのパラメーターは何か?
  - ローカルにバインドされたすべての IP アドレスの一覧を表示するために実行するコマンドは何か?
  - 印刷キューでアクティブな印刷ジョブを一時停止させるために実行するコマンドは何か?
  - テキスト ファイルの内容を確認するために実行するネイティブの Windows PowerShell コマンドは何か?

### <a name="task-2-run-commands-to-accomplish-specified-tasks"></a>タスク 2: コマンドを実行して指定されたタスクを遂行する

1. **Adatum\\Administrator** として **LON-CL1** 仮想マシンにサインインしていることを確認します。

2. 有効な Windows ファイアウォール規則の一覧を表示します。

3. すべてのローカル IPv4 アドレスの一覧を表示します。

4. BITS サービスのスタートアップの種類を **[自動]** に設定します。

   a.   **コンピューターの管理** コンソールを開き、 **[アプリケーションとサービス]** に移動します。

   b.   バックグラウンド インテリジェント転送サービス (BITS) を見つけて、Windows PowerShell でスタートアップの種類を変更する前と後のスタートアップの種類の設定をメモします。

5. **LON-DC1** へのネットワーク接続をテストします。 このコマンドでは、True または False の値のみが返され、他の出力は含まれません。

6. ローカル セキュリティ イベント ログから最新の 10 個のエントリを表示します。

### <a name="exercise-3-results"></a>演習 3 の結果

この演習を完了すると、特定のタスクを実行する Windows PowerShell コマンドを見つけて実行できるようになります。

## <a name="exercise-4-using-about-files"></a>演習 4: About ファイルの使用

### <a name="exercise-scenario-4"></a>演習のシナリオ 4

この演習では、ヘルプ検出手法を使用して、**About** ファイルの内容を検出し、その内容を使用して、グローバルな Windows PowerShell 機能に関する質問に回答します。

**Get-Help** とワイルドカード文字を使用する必要があることに注意してください。 **About** ファイルはコマンドではありません。**Get-Command** は、この演習では役に立ちません。

この演習の主なタスクは次のとおりです。

- **About** ヘルプ ファイルを見つけて確認する。

### <a name="task-1-locate-and-review-about-help-files"></a>タスク 1: About ヘルプ ファイルを見つけて確認する

- 前の演習から引き続き **Adatum\\Administrator** として **LON-CL1** にサインインしていることを確認し、次の質問に回答します。

  - Windows PowerShell でワイルドカード文字列の比較に使用される比較演算子は何ですか?
  - 通常、Windows PowerShell の比較演算子では大文字と小文字は区別されますか?
  - **COMPUTERNAME** 環境変数を表示するには、`$Env` をどのように使用しますか?
  - Windows PowerShell スクリプトに署名するために使用できる自己署名デジタル証明書を作成するために使用する外部コマンドは何ですか?

### <a name="exercise-4-results"></a>演習 4 の結果

この演習を完了すると、**About** ファイル内のヘルプ コンテンツを見つけられるようになります。
