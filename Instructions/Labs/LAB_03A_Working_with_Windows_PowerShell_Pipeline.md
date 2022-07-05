---
lab:
  title: ラボ A:PowerShell パイプラインの使用
  module: 'Module 3: Working with the Windows PowerShell pipeline'
ms.openlocfilehash: 740699b8d650e3d8b35dd231b1051a2ba0050edd
ms.sourcegitcommit: 9c31a6ab628c30fac88ec9070c3d807f2a9bbfdb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2022
ms.locfileid: "146825144"
---
# <a name="lab-using-powershell-pipeline"></a>ラボ: PowerShell パイプラインの使用

## <a name="scenario"></a>シナリオ

Adatum Corporation の管理タスクの 1 つは、高度な PowerShell スクリプトを構成することです。 オブジェクトの並べ替え、フィルター処理、列挙、および変換を行うことによって、PowerShell パイプラインの操作の基礎を理解する必要があります。

## <a name="objectives"></a>目標

このラボを完了すると、次のことができるようになります。

- パイプラインを使用してデータを選択、並べ替え、表示する。
- 基本および高度な構文の形式を使用して、パイプラインからオブジェクトをフィルター処理する。

## <a name="estimated-time-60-minutes"></a>予測される所要時間:60 分

## <a name="lab-setup"></a>ラボのセットアップ

仮想マシン: **AZ-040T00A-LON-DC1**、**AZ-040T00A-LON-SVR1**、および **AZ-040T00A-LON-CL1**

ユーザー名: **Adatum\\Administrator**

パスワード: **Pa55w.rd**

## <a name="lab-startup"></a>ラボのスタートアップ

1. **[LON-DC1]** を選択します。
1. 次の資格情報を使用してサインインします。
   - ユーザー名: **Administrator**
   - パスワード: **Pa55w.rd**
   - ドメイン: **Adatum**

1. **LON-CL1** と **LON-SVR1** についても同じ手順を繰り返します。

## <a name="exercise-1-selecting-sorting-and-displaying-data"></a>演習 1: データの選択、並べ替え、表示

### <a name="exercise-scenario-1"></a>演習のシナリオ 1

この演習では、環境内のコンピューターから管理情報のリストを生成します。 タスクごとに、必要なコマンドを検出し、**Select-Object**、**Sort‑Object**、および書式設定のコマンドレットを使用して、コマンドごとの最終出力をカスタマイズします。

この演習の主なタスクは次のとおりです。

1. 現在の日付を表示する。
1. インストールされている修正プログラムに関する情報を表示する。
1. DHCP サーバーから使用可能なスコープのリストを表示する。
1. 有効な Windows ファイアウォール規則の並べ替えられたリストを表示する。
1. ネットワーク近隣ノードの並べ替えられたリストを表示する。
1. DNS 名前解決キャッシュの情報を表示する。

### <a name="task-1-display-the-current-day-of-the-year"></a>タスク 1: 現在の日付を表示する

1. **LON-CL1** で、管理資格情報を使用して Windows PowerShell を起動します。
2. **date** などのキーワードを使用して、現在の日付を表示できるコマンドを検索します。
3. 前の手順で見つけたコマンドによって生成されるオブジェクトのメンバーを表示します。
4. その年の日付のみを表示します。
5. 前のコマンドの結果を 1 行に表示します。

### <a name="task-2-display-information-about-installed-hotfixes"></a>タスク 2: インストールされている修正プログラムに関する情報を表示する

1. **hotfix** などのキーワードを使用して、インストールされている修正プログラムのリストを表示できるコマンドを検索します。
1. 前の手順で見つけたコマンドによって生成されるオブジェクトのメンバーを表示します。
1. インストールされている修正プログラムのリストを表示します。 インストール日、修正プログラム ID 番号、および修正プログラムをインストールしたユーザーの名前のみを表示します。
1. インストールされている修正プログラムのリストを表示します。 修正プログラム ID、修正プログラムがインストールされてからの日数、および修正プログラムをインストールしたユーザーの名前のみを表示します。

### <a name="task-3-display-a-list-of-available-scopes-from-the-dhcp-server"></a>タスク 3: DHCP サーバーから使用可能なスコープのリストを表示する

1. **DHCP** や **scope** などのキーワードを使用して、インターネット プロトコル バージョン 4 (IPv4) の動的ホスト構成プロトコル (DHCP) スコープのリストを表示できるコマンドを検索します。
1. コマンドのヘルプを確認します。
1. **LON-DC1** で使用可能な IPv4 DHCP スコープのリストを表示します。
1. **LON-DC1** で使用可能な IPv4 DHCP スコープのリストを表示します。 今回は、スコープ ID、サブネット マスク、スコープ名のみを含め、データを 1 つの列に表示します。

### <a name="task-4-display-a-sorted-list-of-enabled-windows-firewall-rules"></a>タスク 4: 有効な Windows ファイアウォール規則の並べ替えられたリストを表示する

1. **rule** などのキーワードを使用して、ファイアウォール規則を表示できるコマンドを検索します。
1. ファイアウォール規則のリストを表示します。
1. ファイアウォール規則を表示するコマンドのヘルプを確認します。
1. 有効になっているファイアウォール規則のリストを表示します。
1. テーブルに同じデータを表示し、情報が切り詰められていないことを確認します。
1. 有効になっているファイアウォール規則のリストを表示します。 規則の表示名、所属するプロファイル、その指示、アクセスを許可するか拒否するかのみを表示します。
1. リストをプロファイルごとに、次に表示名で、アルファベット順に並べ替えます。結果は、プロファイルごとに個別のテーブルに表示されます。

### <a name="task-5-display-a-sorted-list-of-network-neighbors"></a>タスク 5: ネットワーク近隣ノードの並べ替えられたリストを表示する

1. **neighbor** などのキーワードを使用して、ネットワーク近隣ノードを表示できるコマンドを検索します。
1. コマンドのヘルプを確認します。
1. ネットワーク近隣ノードのリストを表示します。
1. 状態によって並べ替えられたネットワーク近隣ノードのリストを表示します。
1. 状態別にグループ化されたネットワーク近隣ノードのリストに、可能な限りコンパクトな形式で IP アドレスのみを表示して、Windows PowerShell がレイアウトを最適化する方法を判断できるようにします。

### <a name="task-6-display-information-from-the-dns-name-resolution-cache"></a>タスク 6: DNS 名前解決キャッシュの情報を表示する

1. **LON-DC1** と **LON-CL1** の両方へのネットワーク接続をテストして、ドメイン ネーム システム (DNS) クライアント キャッシュにデータが設定されていることを確認します。
1. **cache** などのキーワードを使用して、DNS クライアント キャッシュの項目を表示できるコマンドを検索します。
1. DNS クライアント キャッシュを表示します。
1. DNS クライアント キャッシュを表示します。 リストをレコード名で並べ替え、レコード名、レコードの種類、および Time to Live のみを表示します。 列を 1 つだけ使用して、すべてのデータを表示します。

### <a name="exercise-1-results"></a>演習 1 の結果

この演習を完了すると、環境からの管理情報を含むカスタム レポートがいくつか生成されているはずです。

## <a name="exercise-2-filtering-objects"></a>演習 2: オブジェクトのフィルター処理

### <a name="exercise-scenario-2"></a>演習のシナリオ 2

この演習では、フィルター処理を使用して、生成する必要があるレポートに指定されたデータと要素のみを含む管理情報のリストを生成します。

この演習の主なタスクは次のとおりです。

1. Active Directory の [ユーザー] コンテナーにあるすべてのユーザーのリストを表示する。
1. イベント ID **4624** のセキュリティ イベント ログ エントリを表示するレポートを作成する。
1. コンピューターにインストールされている暗号化証明書のリストを表示する。
1. 領域が不足しているディスク ボリュームを表示するレポートを作成する。
1. 指定されたコントロール パネル項目を表示するレポートを作成する。

### <a name="task-1-display-a-list-of-all-the-users-in-the-users-container-of-active-directory"></a>タスク 1: Active Directory の [ユーザー] コンテナーにあるすべてのユーザーのリストを表示する

1. **LON-CL1** で、**Windows PowerShell** を管理者として開きます。
1. **user** などのキーワードを使用して Active Directory ユーザーを一覧表示できるコマンドを検索します。
1. コマンドのヘルプを確認し、必須パラメーターを特定します。
1. プロパティを簡単に比較できる形式で Active Directory のすべてのユーザーのリストを表示します。
1. 同じ形式ですべてのユーザーの同じリストを表示します。 ただし、今度は Active Directory の **[ユーザー]** コンテナーにあるユーザーのみを表示します。 このタスクには、 **"cn=Users,dc=adatum,dc=com"** の検索ベースを使用します。

### <a name="task-2-create-a-report-displaying-the-security-event-log-entries-that-have-the-event-id-4624"></a>タスク 2:イベント ID 4624 のセキュリティ イベント ログ エントリを表示するレポートを作成する

1. イベント ID **4624** の **セキュリティ** イベント ログ エントリの合計数のみを表示します。
1. イベント ID **4624** の **セキュリティ** イベント ログ エントリの完全なリストを表示し、書き込まれた時刻、イベント ID、メッセージのみを表示します。
1. メッセージの詳細を確認できる形式で、10 個の最も古いエントリのみを表示します。

### <a name="task-3-display-a-list-of-the-encryption-certificates-installed-on-the-computer"></a>タスク 3: コンピューターにインストールされている暗号化証明書のリストを表示する

1. **CERT** ドライブ上のすべての項目のディレクトリ一覧を表示します。 リストにはサブフォルダーを含めます。
1. リストを再度表示し、秘密キーのない証明書についてのみ、名前と発行者を表示します。 結果を 1 つの列に表示します。
1. リストを再度表示し、現在の証明書のみを表示します。 これらの証明書には本日より前の **NotBefore** 日付と、本日より後の **NotAfter** 日付があります。 結果に **NotBefore** と **NotAfter** プロパティを含め、日付を簡単に比較できる形式で結果を表示します。 また、データが切り詰められていないことを確認します。

### <a name="task-4-create-a-report-that-displays-the-disk-volumes-that-are-running-low-on-space"></a>タスク 4: 領域が不足しているディスク ボリュームを表示するレポートを作成する

1. ディスク ボリュームのリストを表示します。
1. 空き領域が 0 バイトを超えるボリュームのリストを 1 つの列に表示します。
1. 空き領域が 99% 未満で、空き領域が 0 バイトを超えるボリュームのリストを表示します。 ドライブ文字とメガバイト (MB) 単位でのディスク サイズのみを表示します。
1. 空き領域が 10% 未満で、空き領域が 0 バイトを超えるボリュームのリストを表示します。 コンピューター上に条件を満たすボリュームがない場合、このコマンドを実行しても結果が生成されない可能性があります。

   > **注:** コンピューターの各ボリュームの空き領域が 10% を超える場合、このコマンドではラボ コンピューターに出力が生成されない可能性があります。

### <a name="task-5-create-a-report-that-displays-specified-control-panel-items"></a>タスク 5: 指定されたコントロール パネル項目を表示するレポートを作成する

1. コントロール パネルのすべての項目のリストを表示します。
1. **[システムとセキュリティ]** カテゴリのコントロール パネル項目の名前と説明を名前順に表示します。
1. 同じリストを表示しますが、複数のカテゴリに存在するコントロール パネル項目は除外されます。 コマンドのパフォーマンスが最適化されていることを確認します。

### <a name="exercise-2-results"></a>演習 2 の結果

この演習を完了すると、フィルター処理を使用して、指定されたデータと要素のみを含む管理情報のリストを生成したことになります。