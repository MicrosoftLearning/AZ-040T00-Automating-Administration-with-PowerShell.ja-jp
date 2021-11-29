---
lab:
  title: 'ラボ: PowerShell を使用したローカル システム管理の実行'
  module: 'Module 2: Windows PowerShell for local systems administration'
ms.openlocfilehash: 717c899152e99c75ac726822171023f20ed781c9
ms.sourcegitcommit: a95a9bb3a7919b785df0574c3407f4b6c3bea9f5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2021
ms.locfileid: "132116715"
---
# <a name="lab-performing-local-system-administration-with-powershell"></a>ラボ: PowerShell を使用したローカル システム管理の実行

## <a name="scenario"></a>シナリオ

あなたは、Adatum Corporation のサーバー サポート チームで働いています。 最初の業務の 1 つは、新しいブランチ オフィスのインフラストラクチャ サービスを構成する方法です。 Windows PowerShell を使用してタスクを完了することに決定しました。

## <a name="objectives"></a>目標

このラボを完了すると、次のことができるようになります。

- Windows PowerShell を使用して Active Directory オブジェクトを作成して管理する。
- Windows PowerShell を使用して、Windows Server でネットワーク設定を構成する。
- Windows PowerShell を使用して IIS Web サイトを作成する。

## <a name="estimated-time-60-minutes"></a>予想所要時間: 60 分

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

## <a name="exercise-1-creating-and-managing-active-directory-objects"></a>演習 1: Active Directory オブジェクトの作成と管理

### <a name="exercise-scenario-1"></a>演習のシナリオ 1

この演習では、Active Directory オブジェクトを作成および管理して、ブランチ オフィスの組織単位 (OU) と OU 管理者のグループを作成します。 ブランチ オフィスの既定の OU でユーザーおよびコンピューターのアカウントを作成し、そのユーザーを administrators グループに追加します。 後で、ブランチ オフィス用に作成した OU にユーザーとコンピューターを移動します。 個々の Windows PowerShell コマンドを使用して、クライアント コンピューターからこれらのタスクを実行します。

この演習の主なタスクは次のとおりです。

1. ブランチ オフィス用の新しい OU を作成する。
1. ブランチ オフィスの管理者のグループを作成する。
1. ブランチ オフィスのユーザーとコンピューター アカウントを作成する。
1. グループ、ユーザー、コンピューター アカウントをブランチ オフィス OU に移動する。

### <a name="task-1-create-a-new-ou-for-a-branch-office"></a>タスク 1: ブランチ オフィス用の新しい OU を作成する

- **[LON-CL1]** から、Windows PowerShell を使用して **London** という名前の新しい OU を作成します。

### <a name="task-2-create-a-group-for-branch-office-administrators"></a>タスク 2: ブランチ オフィスの管理者のグループを作成する

- **PowerShell** コンソールで、**London Admins** グローバル セキュリティ グループを作成します。

### <a name="task-3-create-a-user-and-computer-account-for-the-branch-office"></a>タスク 3: ブランチ オフィスのユーザーとコンピューター アカウントを作成する

1. **PowerShell** コンソールで、ユーザー **Ty Carlson** のユーザー アカウントを作成します。
2. **London Admins** グループにユーザーを追加します。
3. **LON-CL2** コンピューターのコンピューター アカウントを作成します。

### <a name="task-4-move-the-group-user-and-computer-accounts-to-the-branch-office-ou"></a>タスク 4: グループ、ユーザー、コンピューター アカウントをブランチ オフィス OU に移動する

- Windows PowerShell を使用して、次のグループ、ユーザー、およびコンピューターのアカウントを London OU に移動します。

  - **London Admins**
  - **Ty Carlson**
  - **LON-CL2**

### <a name="exercise-1-results"></a>演習 1 の結果

この演習を完了すると、Windows PowerShell コマンドライン インターフェイスで Active Directory オブジェクトを管理するためのコマンドを正しく識別して使用できるようになります。

## <a name="exercise-2-configuring-network-settings-on-windows-server"></a>演習 2: Windows Server でのネットワーク設定の構成

### <a name="exercise-scenario-2"></a>演習のシナリオ 2

この演習では、Windows Server でのネットワーク設定を構成します。 効果を確認するには、変更の前と後にネットワーク接続をテストします。 個々の Windows PowerShell コマンドを使用して、サーバー上でこれらのタスクを実行します。

この演習の主なタスクは次のとおりです。

1. ネットワーク接続をテストし、構成を確認する。
1. サーバーの IP アドレスを変更する。
1. サーバーの DNS 設定と既定のゲートウェイを変更する。
1. 変更を確認してテストする。

### <a name="task-1-test-the-network-connection-and-review-the-configuration"></a>タスク 1: ネットワーク接続をテストし、構成を確認する

1. **LON-SVR1** に切り替えます。
2. 管理者特権で **Windows PowerShell** を開きます。
3. **LON-DC1** への接続をテストし、テストの速度をメモします。
4. **LON-SVR1** のネットワーク構成を確認します。
5. IP アドレス、既定のゲートウェイ、DNS サーバーをメモします。

### <a name="task-2-change-the-server-ip-address"></a>タスク 2: サーバーの IP アドレスを変更する

- Windows PowerShell を使用して **Ethernet** ネットワーク インターフェイスの IP アドレスを **172.16.0.15/16** に変更します。

### <a name="task-3-change-the-dns-settings-and-default-gateway-for-the-server"></a>タスク 3: サーバーの DNS 設定と既定のゲートウェイを変更する

1. **Ethernet** ネットワーク インターフェイスの DNS 設定を **172.16.0.12** を指すように変更します。
2. **Ethernet** ネットワーク インターフェイスの既定のゲートウェイを **172.16.0.2** に変更します。

### <a name="task-4-verify-and-test-the-changes"></a>タスク 4: 変更を確認してテストする

1. **LON-SVR1** で、ネットワーク構成への変更を確認します。
2. **LON-DC1** への接続をテストし、テストの速度の違いをメモします。

### <a name="exercise-2-results"></a>演習 2 の結果

この演習を完了すると、ネットワーク構成を管理するための Windows PowerShell コマンドを正しく識別して使用できます。

## <a name="exercise-3-creating-a-website"></a>演習 3: Web サイトの作成

### <a name="exercise-scenario-3"></a>演習のシナリオ 3

この演習では、Web サーバー (IIS) サーバーの役割をインストールし、ロンドン ブランチ用の新しい内部 Web サイトを作成します。 個々の Windows PowerShell コマンドを使用して、サーバー上でこれらのタスクを実行します。

この演習の主なタスクは次のとおりです。

1. サーバーに Web サーバー (IIS) の役割をインストールする。
1. Web サイトのファイル用のフォルダーをサーバーに作成する。
1. IIS Web サイトを作成する。

### <a name="task-1-install-the-web-server-iis-role-on-the-server"></a>タスク 1: サーバーに Web サーバー (IIS) の役割をインストールする

- **LON-SVR1** で、Windows PowerShell を使用して **LON-SVR1** に Web サーバー (IIS) の役割をインストールします。

### <a name="task-2-create-a-folder-on-the-server-for-the-website-files"></a>タスク 2: Web サイトのファイル用のフォルダーをサーバーに作成する

- **LON-SVR1** では、PowerShell を使用して、Web サイト ファイルの **C:\inetpub\wwwroot** の下に、**London** という名前のフォルダーを作成します。

### <a name="task-3-create-the-iis-website"></a>タスク 3: IIS Web サイトを作成する

1. **LON-SVR1** では、PowerShell を使用して、次の構成を使用して IIS Web サイトを作成します。

    - 名前: **London**
    - 物理パス: 以前に作成したフォルダー
    - バインド情報: ポート **8080** を使用した **LON-SVR1** の現在の IP アドレス

1. IP アドレスとポート **8080** を使用して Internet Explorer で Web サイトを開き、指定された設定がサイトで使用されていることを確認します。 Internet Explorer から、URL に対してドキュメントが構成されていないというエラー メッセージが表示されます。 エラー メッセージの詳細に、サイトの物理パスが表示されます。これは、**C:\\inetpub\\wwwroot\\london** であるはずです。

### <a name="exercise-3-results"></a>演習 3 の結果

この演習を完了すると、標準化された Web サーバー構成の一部として使用される Windows PowerShell コマンドを正しく識別して使用できます。
