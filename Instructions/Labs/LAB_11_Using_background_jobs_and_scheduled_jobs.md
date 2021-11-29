---
lab:
  title: 'ラボ: PowerShell を使用したジョブ管理'
  module: 'Module 11: Using background jobs and scheduled jobs'
ms.openlocfilehash: 8383e48f8043717452c6d8c847ee296e18837f53
ms.sourcegitcommit: a95a9bb3a7919b785df0574c3407f4b6c3bea9f5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2021
ms.locfileid: "132116712"
---
# <a name="lab-jobs-management-with-powershell"></a>ラボ: PowerShell を使用したジョブ管理

## <a name="scenario"></a>シナリオ

バックグラウンド ジョブは、複数のコマンドを同時に実行したり、実行時間の長いコマンドをバックグラウンドで実行したりするのに便利な方法です。 このラボでは、3 つの基本的な種類のジョブのうち、2 つを作成して管理する方法について説明します。

スケジュールされた 2 つのジョブを作成して構成します。 また、特定のセキュリティ グループから無効になっているアカウントを検索して削除する Windows PowerShell スクリプトを使用して、スケジュールされたタスクを作成します。

## <a name="objectives"></a>目標

このラボを完了すると、次のことができるようになります。

- ジョブを開始して管理する。
- スケジュールされたジョブを作成する。

### <a name="estimated-time-30-minutes"></a>予想所要時間: 30 分

## <a name="lab-setup"></a>ラボのセットアップ

仮想マシン: **LON-DC1**、**LON-SVR1**、および **LON-CL1**

ユーザー名: **Adatum\\Administrator**

パスワード: **Pa55w.rd**

このラボでは、提供されている仮想マシン (VM) 環境を使用します。 ラボを開始する前に、次の手順を行ってください。

1. **LON-DC1** を開いて、**Adatum\\Administrator** として、パスワード **Pa55w.rd** を使用してサインインします。
1. **LON-SVR1** および **LON-CL1** に対して手順 1 を繰り返し行います。

## <a name="exercise-1-starting-and-managing-jobs"></a>演習 1: ジョブの開始と管理

### <a name="exercise-scenario-1"></a>演習のシナリオ 1

この演習では、2 つの基本的なジョブの種類を使用してジョブを開始します。

この演習の主なタスクは次のとおりです。

1. Windows PowerShell ジョブを開始します。
1. ローカル ジョブを開始します。
1. ジョブの状態を確認および管理します。

### <a name="task-1-start-a-windows-powershell-job"></a>タスク 1: Windows PowerShell ジョブを開始する

1. **LON-CL1** に **Adatum\\Administrator** として、パスワード **Pa55w.rd** を使用して確実にサインインします。
1. 物理ネットワーク アダプターの一覧を **LON-DC1** および **LON-SVR1** から取得する Windows PowerShell リモート処理ジョブを開始します。 ジョブに **RemoteNetAdapt** という名前を付けます。
1. サーバー メッセージ ブロック (SMB) の一覧を **LON-DC1** および **LON-SVR1** から取得する Windows PowerShell リモート処理ジョブを開始します。 ジョブに **RemoteShares** という名前を付けます。
1. **Win32_Volume** Common Information Model (CIM) クラスのすべてのインスタンスを、Active Directory ドメイン サービス (AD DS) 内のすべてのコンピューターから取得する Windows PowerShell リモート処理ジョブを開始します。 ジョブに **RemoteDisks** という名前を付けます。 一部のドメイン コンピューターは起動しない可能性があるため、一部の子ジョブは失敗する可能性があります。

### <a name="task-2-start-a-local-job"></a>タスク 2: ローカル ジョブを開始する

1. すべてのエントリを **セキュリティ** イベント ログから取得するローカル ジョブを開始します。 ジョブに **LocalSecurity** という名前を付けます。
1. 範囲演算子 ( **..** ) と **ForEach-Object** を使用して、ドライブ C の 100 個のディレクトリ一覧 (サブフォルダーを含む) を生成するローカル ジョブを開始します。 ジョブに **LocalDir** という名前を付けます。 このジョブがまだ実行されている間に、次のタスクに進みます。

### <a name="task-3-review-and-manage-job-status"></a>タスク 3: ジョブの状態を確認して管理する

1. **LON-CL1** に **Adatum\\Administrator** として、パスワード **Pa55w.rd** を使用して確実にサインインします。
1. 実行中のジョブの一覧を表示します。
1. 名前が **リモート** で始まる実行中のジョブの一覧を表示します。
1. **LocalDir** ジョブを強制的に停止します。
1. 残りのすべてのジョブが正常終了するか、失敗するのを待ちます。
1. **RemoteNetAdapt** ジョブの結果を受信します。
1. **RemoteDisks** ジョブについては、単一のコマンド ラインを使用して **LON-DC1** から結果を取得します。

> **注:** 開始するには、親ジョブに対してクエリを実行してから、その **ChildJob** プロパティを展開する必要があります。 **LON-DC1** ジョブだけが残るように子ジョブをフィルター処理し、次にそのジョブから結果を受け取ります。 この手順を完了するには、合計 4 つのコマンドを使用します。

## <a name="exercise-2-creating-a-scheduled-job"></a>演習 2: スケジュールされたジョブの作成

### <a name="exercise-scenario-2"></a>演習のシナリオ 2

この演習では、スケジュールされたジョブを作成して実行し、その結果を取得します。 次に、AD DS 内のセキュリティ グループから無効になっているユーザーを削除する Windows PowerShell スクリプトを使用してスケジュールされたタスクを作成して実行します。

この演習の主なタスクは次のとおりです。

1. ジョブ オプションとジョブ トリガーを作成します。
1. スケジュールされたジョブを作成し、結果を取得します。
1. スケジュールされたタスクとして、Windows PowerShell スクリプトを使用します。

### <a name="task-1-create-job-options-and-job-triggers"></a>タスク 1: ジョブ オプションとジョブ トリガーを作成する

1. **LON-CL1** に **Adatum\\Administrator** として、パスワード **Pa55w.rd** を使用して確実にサインインします。
1. ジョブ オプション オブジェクトを作成し、`$option` に格納します。 ジョブで次のことが行われるように、ジョブ オブジェクトを構成します。

    - 実行するコンピューターのスリープ状態を解除します。
    - 昇格されたアクセス許可で実行します。

1. ジョブ トリガー オブジェクトを作成し、それを `$trigger1` に格納します。 ジョブで次のことが行われるようにトリガーを構成します。

    - 5 分に 1 回実行します。 **Get-Date** と、結果として生成された **DateTime** オブジェクトのメソッドを使用して、今から 5 分先を計算します。

### <a name="task-2-create-a-scheduled-job-and-retrieve-results"></a>タスク 2: スケジュールされたジョブを作成し、結果を取得する

1. `$option` と `$trigger1` を使用して、次の属性を持つ新しいスケジュールされたジョブを作成します。

    - ジョブ アクションによって、**セキュリティ** イベント ログからすべてのエントリが取得されます。
    - ジョブ名は **LocalSecurityLog** です。
    - ジョブの結果の最大数は 5 です。

1. スケジュールされたジョブ **LocalSecurityLog** のジョブ トリガーの一覧 (時刻を含む) を表示します。
1. 手順 2 で表示された時間が経過するまで待ちます。
1. ジョブの一覧を表示します。
1. **LocalSecurityLog** のジョブ結果を受け取ります。

### <a name="task-3-use-a-windows-powershell-script-as-a-scheduled-task"></a>タスク 3: スケジュールされたタスクとして Windows PowerShell スクリプトを使用する

1. **LON-DC1** 上で、 **[Active Directory ユーザーとコンピューター]** を開きます。
1. **[Active Directory ユーザーとコンピューター]** の **[Managers]** 組織単位 (OU) からユーザーを選択し、そのユーザー アカウントを無効にします
1. [**タスク スケジューラ**] を開きます。
1. 次のプロパティを使用して新しいタスクを作成します。

    - 名前と説明: **Managers セキュリティ グループから、無効にされたユーザーを削除する**
    - セキュリティ オプション: **[ユーザーがログオンしているかどうかにかかわらず実行する]** と **[最高の特権で実行する]**
    - トリガー設定: **[毎日]** 。時刻は現在の時刻の 5 分後に設定します
    - アクション: **プログラムまたはスクリプト** を **PowerShell.exe** に設定します
    - 引数の追加 (省略可能): 「 **-ExecutionPolicy Bypass E:\\Labfiles\\Mod11\\DeleteDisabledUserManagersGroup.ps1**」と入力します
    - 設定: **[タスクが既に実行中の場合に適用される規則]** を **[既存のインスタンスの停止]** に設定します

1. 5 分後に、タスク **Managers セキュリティ グループから、無効にされたユーザーを削除する** の履歴を確認します。
1. **[Active Directory ユーザーとコンピューター]** に戻り、手順 2 で無効にしたユーザー アカウントが **Managers** セキュリティ グループのメンバーではなくなったことを確認します。
