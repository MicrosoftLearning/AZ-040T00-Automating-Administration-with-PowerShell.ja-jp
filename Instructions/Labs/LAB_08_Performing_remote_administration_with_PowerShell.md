---
lab:
  title: 'ラボ: PowerShell を使用したリモート管理の実行'
  module: 'Module 8: Administering remote computers with Windows PowerShell'
ms.openlocfilehash: 692d89c683207781e9ee4ca4e64316b500a46d4f
ms.sourcegitcommit: a95a9bb3a7919b785df0574c3407f4b6c3bea9f5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2021
ms.locfileid: "132116701"
---
# <a name="lab-performing-remote-administration-with-powershell"></a>ラボ: PowerShell を使用したリモート管理の実行

## <a name="scenario"></a>シナリオ

あなたは Adatum Corporation の管理者であり、Windows Server 2019 を実行しているサーバー上でメンテナンス タスクを実行する必要があります。 サーバーに物理的にアクセスすることはできず、代わりに Windows PowerShell リモート処理を使用してタスクを実行する予定です。 また、サーバーと、Windows 10 を実行している別のクライアント コンピューターの両方で実行するタスクもいくつかあります。 あなたの環境では、ローカル コンピューターとサーバーとの間で、リモート プロシージャ コール (RPC) などの通信プロトコルはブロックされています。 あなたは Windows PowerShell リモート処理を使用することを予定しており、セッションを使用して永続性を確保し、即席のリモート処理接続で発生するセットアップとクリーンアップのオーバーヘッドを削減したいと考えています。

## <a name="objectives"></a>目標

このラボを完了すると、次のことができるようになります。

- クライアント コンピューターでリモート処理を有効にする。
- 一対一のリモート処理を使用して、リモート コンピューター上でタスクを実行する。
- 一対多のリモート処理を使用して、2 台のコンピューター上でタスクを実行する。
- PSSession を作成して管理する。
- コマンドを複数のコンピューターに並列で送信する。

## <a name="estimated-time-60-minutes"></a>予想所要時間: 60 分

## <a name="lab-setup"></a>ラボのセットアップ

仮想マシン:

- **LON-DC1**
- **LON-CL1**

ユーザー名: **Adatum\\Administrator**

パスワード: **Pa55w.rd**

このラボでは、使用可能な仮想マシン環境を使います。 ラボを開始する前に、次の手順を行ってください。

1. **LON-DC1** を開いて、**Administrator** として、パスワード **Pa55w.rd** を使用してサインインします。
1. **LON-CL1** について手順 1 を繰り返します。

## <a name="exercise-1-enabling-remoting-on-the-local-computer"></a>演習 1: ローカル コンピューターでリモート処理を有効にする

### <a name="scenario-1"></a>シナリオ 1

この演習では、クライアント コンピューターでリモート処理を有効にします。

この演習の主なタスクは次のとおりです。

- 着信接続に対してリモート処理を有効にする。

### <a name="task-1-enable-remoting-for-incoming-connections"></a>タスク 1: 着信接続に対してリモート処理を有効にする

1. **Adatum\\Administrator** として **LON-CL1** にサインインしていることを確認します。
1. Windows PowerShell コマンド ウィンドウが開いていることを確認します。
1. **ExecutionPolicy** が **RemoteSigned** に設定されていることを確認します。
1. **LON-CL1** クライアント コンピューターで着信接続のリモート処理を有効にします。
1. Windows PowerShell でセッション構成が複数作成されたことを確認します。 このリストを生成できるコマンドを見つける必要があります。

## <a name="exercise-2-performing-one-to-one-remoting"></a>演習 2: 一対一のリモート処理の実行

### <a name="scenario-2"></a>シナリオ 2

この演習では、リモート コンピューターに接続し、メンテナンス タスクを実行します。

この演習の主なタスクは次のとおりです。

1. リモート コンピューターに接続し、オペレーティング システム機能をインストールする。
1. マルチホップ リモート処理をテストする。
1. リモート処理の制限事項を確認する。

### <a name="task-1-connect-to-the-remote-computer-and-install-an-operating-system-feature-on-it"></a>タスク 1: リモート コンピューターに接続し、オペレーティング システム機能をインストールする

1. **LON-CL1** に引き続きサインインしていることを確認します。
1. **LON-CL1** の Windows PowerShell コンソールで、リモート処理を使用して **LON-DC1** への一対一の接続を確立します。
1. **LON-DC1** にネットワーク負荷分散 (NLB) 機能をインストールします。
1. **LON-DC1** から接続解除します。

### <a name="task-2-test-multi-hop-remoting"></a>タスク 2: マルチホップ リモート処理をテストする

1. **LON-DC1** への一対一のリモート処理接続を確立します。
1. **LON-DC1** から **LON-CL1** への接続の確立を試みます。 何が行われますか。また、その理由は何ですか。
1. 接続を閉じます。

### <a name="task-3-observe-remoting-limitations"></a>タスク 3: リモート処理の制限事項を確認する

1. コンピューター名 **localhost** を使用して、**LON-CL1** への一対一のリモート処理接続を確立します。 これはローカル コンピューターですが、この新しい接続により、コンピューター上に 2 番目のユーザー セッションが作成されます。
1. Windows PowerShell を使用して、メモ帳の新しいインスタンスを起動します。 何が行われますか。また、その理由は何ですか。
1. パイプラインを停止してメモ帳のプロセスを終了します。
1. 接続を閉じます。

## <a name="exercise-3-performing-one-to-many-remoting"></a>演習 3: 一対多のリモート処理の実行

この演習では、複数のコンピューターに対してコマンドを実行します。 そのうちの 1 つはクライアント コンピューターですが、それに対する 2 番目のサインインを各コマンドの継続中に確立します。

この演習の主なタスクは次のとおりです。

1. 2 台のコンピューターから物理ネットワーク アダプターのリストを取得する。
1. ローカル コマンドの出力をリモート コマンドのものと比較する。

### <a name="task-1-retrieve-a-list-of-physical-network-adapters-from-two-computers"></a>タスク 1: 2 台のコンピューターから物理ネットワーク アダプターのリストを取得する

1. **LON-CL1** 上で **adapter** などのキーワードを使用して、ネットワーク アダプターを一覧表示できるコマンドを見つけます。
1. コマンドのヘルプを確認し、出力を物理アダプターに制限する switch パラメーターを見つけます。
1. リモート処理を使用して、**LON-CL1** と **LON-DC1** の両方でコマンドを実行します。

### <a name="task-2-compare-the-output-of-a-local-command-to-that-of-a-remote-command"></a>タスク 2: ローカル コマンドの出力をリモート コマンドのものと比較する

1. パイプを使用して **Process** オブジェクトのコレクションを **Get-Member** に渡します。
2. リモート処理を使用して **LON-DC1** から **Process** オブジェクトのリストを取得し、パイプを使用して **Get-Member** に渡します。
3. 2 つの **Get-Member** の結果出力を比較します。

## <a name="exercise-4-using-implicit-remoting"></a>演習 4: 暗黙的なリモート処理の使用

この演習では、暗黙的なリモート処理を使用して、リモート コンピューターからコマンドをインポートして実行します。

この演習の主なタスクは次のとおりです。

1. サーバーへの永続的なリモート処理接続を作成する。
1. サーバーからモジュールをインポートして使用する。
1. 開いているすべてのリモート処理接続を閉じる。

### <a name="task-1-create-a-persistent-remoting-connection-to-a-server"></a>タスク 1: サーバーへの永続的なリモート処理接続を作成する

1. **LON-CL1** で、**Adatum\\Administrator** として、パスワード **Pa55w.rd** を使用してサインインします。
2. **[スタート]** メニューを選択してから、「**powersh**」と入力します。
3. 結果リストで、 **[Windows PowerShell]** を右クリックするか、そのコンテキスト メニューをアクティブにしてから、 **[管理者として実行]** を選択します。
4. Windows PowerShell コマンド ウィンドウで、**LON-DC1** への永続的なリモート処理接続を作成し、生成された PSSession オブジェクトを変数 `$dc` に保存します。
5. `$dc` 内の PSSession のリストを表示し、それらを使用できることを確認します。

### <a name="task-2-import-and-use-a-module-from-a-server"></a>タスク 2: サーバーからモジュールをインポートして使用する

1. **LON-CL1** から、**LON-DC1** 上の Windows PowerShell モジュールのリストを表示します。
1. サーバー メッセージ ブロック (SMB) 共有を使用できるモジュールを **LON-DC1** 上で見つけます。
1. そのモジュールをローカル コンピューターにインポートし、インポートされた各コマンドの名詞にプレフィックス **DC** を追加します。
1. **LON-DC1** 上の SMB 共有のリストを表示します。
1. クライアント コンピューター上の SMB 共有のリストを表示します。

### <a name="task-3-close-all-open-remoting-connections"></a>タスク 3: 開いているすべてのリモート処理接続を閉じる

- Windows PowerShell コンソールで、開いているすべてのリモート処理接続を閉じます。

## <a name="exercise-5-managing-multiple-computers"></a>演習 5: 複数のコンピューターの管理

この演習では、PSSession を使用して永続性を確保し、複数のコンピューターに対していくつかの管理タスクを実行します。

この演習の主なタスクは次のとおりです。

1. 2 台のコンピューターに対する PSSession を作成する。
1. 2 台のコンピューターの Windows ファイアウォール規則を表示するレポートを作成する。
1. 2 台のコンピューターからのローカル ディスク情報を表示する HTML レポートを作成して表示する。
1. 開いているすべての PSSession を閉じる。

### <a name="task-1-create-pssessions-to-two-computers"></a>タスク 1: 2 台のコンピューターに対する PSSession を作成する

1. **Windows PowerShell** コンソールで、**LON-CL1** と **LON-DC1** の両方に対する PSSession を作成します。
1. 両方の PSSession オブジェクトを変数 `$computers` に保存します。
1. `$computers の接続が開かれていて使用できることを確認します。

### <a name="task-2-create-a-report-that-displays-windows-firewall-rules-from-two-computers"></a>タスク 2: 2 台のコンピューターの Windows ファイアウォール規則を表示するレポートを作成する

1. ネットワーク セキュリティに対応した Windows PowerShell モジュールを見つけます。
1. 1 行のコマンド ラインを使用して、**LON-CL1** と **LON-DC1** の両方のメモリにモジュールを読み込みます。
1. Windows ファイアウォール規則を表示できるコマンドを見つけます。
1. 1 行のコマンド ラインを使用して、**LON-CL1** と **LON-DC1** の両方で有効になっているファイアウォール規則を一覧表示します。
1. 規則の名前と、各規則の元になっているコンピューターの名前のみを表示します。
1. 1 行のコマンド ラインを使用して、**LON-CL1** と **LON-DC1** の両方のメモリからネットワーク セキュリティ モジュールをアップロードします。

### <a name="task-3-create-and-display-an-html-report-that-displays-local-disk-information-from-two-computers"></a>タスク 3: 2 台のコンピューターからのローカル ディスク情報を表示する HTML レポートを作成して表示する

1. テストとして、**Get-WmiObject** を使用して、ローカル ハード ドライブ (ドライブの種類が **3** のドライブだけを含むようにフィルターを適用した **Win32_LogicalDisk** クラス) のリストを表示します。
1. **LON-DC1** と **LON-CL1** の両方に対して、リモート処理を使用して **Get-WmiObject** コマンドをもう一度実行します。 **Get-WmiObject** コマンドに **–ComputerName** パラメーターを追加しないでください。

   > **注:** レポートには、各コンピューターの名前、各ドライブの文字と空き領域、合計サイズ (バイト単位) を含める必要があります。

### <a name="task-4-close-all-open-pssessions"></a>タスク 4: 開いているすべての PSSession を閉じる

- **Windows PowerShell** コンソールで、開いているすべての PSSession を閉じます。
