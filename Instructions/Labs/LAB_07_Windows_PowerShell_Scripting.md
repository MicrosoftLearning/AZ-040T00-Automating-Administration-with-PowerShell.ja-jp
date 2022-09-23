---
lab:
  title: 'ラボ: PowerShell でのスクリプトの使用'
  module: 'Module 4: Windows PowerShell scripting'
ms.openlocfilehash: 9a27794afee306862c49d89c27958f811c456b5b
ms.sourcegitcommit: a95a9bb3a7919b785df0574c3407f4b6c3bea9f5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2021
ms.locfileid: "132116714"
---
# <a name="lab-using-scripts-with-powershell"></a>ラボ: PowerShell でのスクリプトの使用

## <a name="scenario"></a>シナリオ

組織での管理を簡略化するために、Windows PowerShell スクリプトの開発を開始しました。 実行するタスクは複数あり、それぞれに Windows PowerShell スクリプトを作成します。

## <a name="objectives"></a>目標

このラボを完了すると、次のことができるようになります。

- スクリプトにデジタル署名する。
- ForEach を使用して配列を処理する。
- If ステートメントを使用して項目を処理する。
- CSV ファイルに基づいてユーザー アカウントを作成する。
- リモート コンピューターからのディスク情報のクエリを実行する。
- 代替の資格情報を使用するようにスクリプトを更新する。

## <a name="estimated-time-150-minutes"></a>予想所要時間: 150 分

## <a name="lab-setup"></a>ラボのセットアップ

- 仮想マシン: **LON-DC1**、**LON-SVR1**、および **LON-CL1**
- ユーザー名: **Adatum\\Administrator**
- パスワード: **Pa55w.rd**

このラボでは、使用可能な仮想マシン環境を使います。 ラボを開始する前に、次の手順を行ってください。

1. **LON-DC1** を開き、パスワード **Pa55w.rd** を使用して **Adatum\\Administrator** としてサインインします。
1. **LON-SVR1** と **LON-CL1** に対して手順 1 を繰り返します。

## <a name="exercise-1-signing-a-script"></a>演習 1: スクリプトへの署名

### <a name="exercise-scenario-1"></a>演習のシナリオ 1

セキュリティを強化するために、環境内のすべての Windows PowerShell スクリプトにデジタル署名するという要件を検討しています。 この要件を実装する前に、プロセスをテストしたいと考えています。

この演習の主なタスクは次のとおりです。

1. コード署名証明書をインストールします。
1. スクリプトにデジタル署名します。
1. 実行ポリシーを設定します。

### <a name="task-1-install-a-code-signing-certificate"></a>タスク 1: コード署名証明書をインストールする

1. **LON-CL1 で**、**MMC** コンソールを開いてから、 **[ユーザー アカウント]** に焦点を当てた **[証明書]** スナップインを追加します。
1. **MMC** コンソールで、 **[証明書 - 現在のユーザー\\個人]** を参照します。
1. **個人** フォルダーのコンテキスト メニューを使用し、 **[新しい証明書の要求]** を選択します。
1. **[証明書の登録]** ウィザードで次の設定を使用します。

   - **Active Directory 登録ポリシー**
   - **Adatum コード署名** テンプレート

1. **MMC** コンソールで、新しいコード署名証明書があることを確認します。
1. **MMC** コンソールを閉じます。

### <a name="task-2-digitally-sign-a-script"></a>タスク 2: スクリプトへのデジタル署名

1. Windows PowerShell プロンプトを開きます。
1. **Cert:\CurrentUser\My** のコード署名証明書を変数に配置します。
1. **E:\Mod07\Labfiles** で、**HelloWorld.txt** の名前を **HelloWorld.ps1** に変更します。
1. **Set-Authenticode** コマンドレットを使用して、デジタル署名を **HelloWorld.ps1** に適用します。

### <a name="task-3-set-the-execution-policy"></a>タスク 3: 実行ポリシーを設定する

1. **LON-CL1** で、署名付きスクリプトのみを許可するように実行ポリシーを設定します。
1. **HelloWorld.ps1** を実行できることを確認します。
1. 実行ポリシーを **[無制限]** に設定します。

## <a name="exercise-2-processing-an-array-with-a-foreach-loop"></a>演習 2: ForEach ループを使用する配列の処理

### <a name="scenario-2"></a>シナリオ 2

Adatum Corporation では、新しいボイス オーバー IP 通話 (VoIP) およびビデオ会議システムをテストしています。 このシステムをサポートするには、テスト ユーザーのグループに **ipPhone** 属性を設定する必要があります。 **ipPhone** 属性に対して選択されている名前付け規則は、 **FirstName.LastName@adatum.com** です。

この演習の主なタスクは次のとおりです。

1. テスト グループを作成します。
1. `ipPhone` 属性を構成するためのスクリプトを作成します。

### <a name="task-1-create-a-test-group"></a>タスク 1: テスト グループを作成する

1. **LON-CL1** で、PowerShell を使用して、**IT** 組織単位で **IPPhoneTest** という名前の新しい Active Directory Domain Services (AD DS) グループを作成します。
1. 次のユーザーを **IPPhoneTest** グループのメンバーとして追加します。

   - **Abbi Skinner**
   - **Ida Alksne**
   - **Parsa Schoonen**
   - **Tonia Guthrie**

### <a name="task-2-create-a-script-to-configure-the-ipphone-attribute"></a>タスク 2: ipPhone 属性を構成するためのスクリプトを作成する

1. **E:\\Mod07\\Labfiles\\ipPhone.ps1** という名前のスクリプトを作成してから、Windows PowerShell ISE でそれを開きます。
1. **Get-ADGroupMember** を使用して、**IPPhoneTest** グループのメンバーシップを取得するためのクエリを作成します。
1. **IPPhoneTest** のメンバーであるユーザーを処理する **ForEach** ループを作成します。
1. ループで次のようにします。

   - **Get-ADUser** を使用して、ユーザーの名と姓を取得します。
   - **ipPhone** 属性に必要な値を計算します。
   - *-Replace* パラメーターを指定した **Set-ADUser** を使用して、ユーザーの **ipPhone** 属性を設定します。

1. スクリプトを実行してから、選択されたユーザーの **ipPhone** 属性が変更されていることを確認します。

## <a name="exercise-3-processing-items-by-using-if-statements"></a>演習 3: If ステートメントを使用した項目の処理

組織内の一部のサーバーには、サーバーの再起動時に正常に開始されないサービスがあります。 指定されたリストのサービスを開始するために使用できるスクリプトを作成したいと考えています。 十分なテストを行ったら、スクリプトを実行するスケジュールされたタスクを構成する予定です。 テスト フェーズ中は、Windows タイムおよび印刷スプーラー サービスを使用します。

この演習の主なタスクは次のとおりです。

1. サービス名を含む services.txt を作成します。
1. 停止されたサービスを開始するスクリプトを作成します。

### <a name="task-1-create-servicestxt-with-service-names"></a>タスク 1: サービス名を含む services.txt を作成する

1. **LON-CL1** で、Windows PowerShell を開きます。
1. 新しいファイル **E:\\Mod07\\Labfiles\\services.txt** を作成します。
1. **印刷スプーラー** サービスの正しい名前を特定し、それを **services.txt** に追加します。
1. **Windows タイム** サービスの正しい名前を特定し、それを **services.txt** に追加します。

### <a name="task-2-create-a-script-that-starts-stopped-services"></a>タスク 2: 停止されたサービスを開始するスクリプトを作成する

1. 新しいスクリプト **E:\\Mod07\\Labfiles\\StartServices.ps1** を作成します。
1. **services.txt** からサービス名を取得し、それらを変数に配置します。
1. **ForEach** ループを使用して各サービスを処理します。

   - サービスが実行されていない場合は開始してから、サービスが開始されたことを示すテキストを画面に入力します。
   - サービスが実行されている場合は何もせずに、操作が不要であることを示すテキストを画面に入力します。

## <a name="exercise-4-creating-users-based-on-a-csv-file"></a>演習 4: CSV ファイルに基づくユーザーの作成

### <a name="exercise-scenario-4"></a>演習のシナリオ 4

Adatum のヘルプデスクでは、人事部によって提供されたデータに基づいて、週に 1 回ユーザー アカウントを作成しています。 このデータは CSV ファイルで提供されます。

新しいユーザー アカウントが正しい情報で作成されていないインスタンスが複数あります。 ヘルプデスクでは、CSV ファイルを参照として使用し、グラフィカル ツールを使ってユーザー アカウントを作成しています。 あなたは、これらのエラーを回避するためにこのプロセスを自動化したいと考えています。

この演習の主なタスクは次のとおりです。

- CSV ファイルから AD DS を作成します。

### <a name="task-1-create-ad-ds-users-from-a-csv-file"></a>タスク 1: CSV ファイルから AD DS ユーザーを作成する

1. **E:\\Mod07\\Labfiles\\CreateUsers.ps1** という名前の新しいスクリプトを作成します。
1. **users.csv** をインポートし、変数にオブジェクトを格納します。
1. 変数のデータを処理してユーザー アカウントを作成するための **ForEach** ループを作成します。

   - ユーザーの組織単位のライトウェイト ディレクトリ アクセス プロトコル (LDAP) 名を含む変数を作成します。 例: OU=IT,DC=Adatum,DC=com
   - 新しいユーザーのユーザー プリンシパル名を含む変数を作成します。 これは、 **UserID@adatum.com** という形式である必要があります。
   - 新しいユーザーの表示名を含む変数を作成します。 これは、*FirstName LastName* という形式である必要があります。
   - 作成されているユーザーとその場所を示すステータス メッセージを画面に入力します。
   - 新しいユーザーを作成し、必ず次のように設定してください。

      - **GivenName**
      - **Surname**
      - **名前**
      - **DisplayName**
      - **SamAccountName**
      - **UserPrincipalName**
      - **Path**
      - **部門**

## <a name="exercise-5-querying-disk-information-from-remote-computers"></a>演習 5: リモート コンピューターからのディスク情報のクエリの実行

### <a name="exercise-scenario-5"></a>演習のシナリオ 5

Adatum では、すべてのコンピューターの論理ディスク構成を文書化していません。 ドキュメントの情報収集の一環として、論理ディスク情報を収集するためのスクリプトを作成します。

ディスク情報のスクリプトには、次の要件があります。

- リモート コンピューター名をパラメーターとして受け入れます。
- コンピューター名がパラメーターとして指定されていない場合は、ユーザーにコンピューター名の入力を求める必要があります。
- 情報のクエリでは、分散コンポーネント オブジェクト モデル (DCOM) ではなく、Web Services-Management (WS-MAN) を使用する必要があります。
- 物理ディスク情報ではなく、論理ディスク情報 (ボリューム) を表示します。
- ローカル ディスク (ハード ドライブ) の情報のみを含める必要があります。

この演習の主なタスクは次のとおりです。

- 現在の資格情報でディスク情報のクエリを実行するスクリプトを作成します。

### <a name="task-1-create-a-script-that-queries-disk-information-with-current-credentials"></a>タスク 1: 現在の資格情報でディスク情報のクエリを実行するスクリプトを作成する

1. **LON-CL1** を使用して、すべてのタスクを実行します。
1. WS-MAN を使用して、リモートでハードウェア情報のクエリを実行できるコマンドレットを特定します。
1. 論理ディスク情報のクエリを実行するために必要な構文を特定し、ローカル ディスクのみを含めます。
1. 新しいスクリプト **E:\\Mod07\\QueryDisk.ps1** を作成します。
1. コンピューター名を受け入れ、指定されていない場合はコンピューター名の入力を求めるスクリプトに **param()** ブロックを追加します。
1. スクリプトで **LON-DC1** からのディスク情報のクエリが正しく実行されることを確認します。

## <a name="exercise-6-updating-the-script-to-use-alternate-credentials"></a>演習 6: 別の資格情報を使用するためのスクリプトの更新

### <a name="exercise-scenario-6"></a>演習のシナリオ 6

リモート コンピューターからのディスク情報のクエリを実行するスクリプトを実行したいと考えています。 リモート サーバー上のディスク情報のクエリを実行するアクセス許可がユーザーにないシナリオを考慮するために、代替の資格情報が指定されている場合はそれらを受け入れるようにスクリプトを更新します。

このスクリプトは次の要件を満たす必要があります。

- 代替の資格情報が必要かどうかを示すスイッチ パラメーターを受け入れます。
- 代替の資格情報が必要な場合は、これらの資格情報を収集して使用します。

この演習の主なタスクは次のとおりです。

- 代替の資格情報を使用するようにスクリプトを更新します。

### <a name="task-1-update-the-script-to-use-alternate-credentials"></a>タスク 1: 代替の資格情報を使用するようにスクリプトを更新する

1. 編集するために **E:\\Mod07\\QueryDisk.ps1** を開きます。
1. 代替の資格情報が使用されることを示すスイッチを含めるように **param()** ブロックを更新します。
1. スイッチを評価する **If** ステートメントを追加します。

   - スイッチが true の場合は、代替の資格情報を収集する新しいコードを実行します。
   - スイッチが false の場合は、既存のクエリを実行します。

1. 新しいコードの場合は、次のことを行う必要があります。
   - ユーザーから資格情報を取得します。
   - その資格情報を使用して、リモート セッションを開きます。
   - そのセッションを使用して、クエリを送信します。
