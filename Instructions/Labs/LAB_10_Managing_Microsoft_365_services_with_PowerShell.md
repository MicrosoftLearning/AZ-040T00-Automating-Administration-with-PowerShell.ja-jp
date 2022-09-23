---
lab:
  title: 'ラボ: PowerShell を使用した Microsoft 365 の管理'
  module: 'Module 10: Managing Microsoft 365 services with PowerShell'
ms.openlocfilehash: 15571fd913e80e2c72c66f5eda1fa46cd88ecc68
ms.sourcegitcommit: 9c31a6ab628c30fac88ec9070c3d807f2a9bbfdb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2022
ms.locfileid: "146824955"
---
# <a name="lab-managing-microsoft-365-with-powershell"></a>ラボ: PowerShell を使用した Microsoft 365 の管理

## <a name="scenario"></a>シナリオ

新しい Microsoft 365 テナントを作成しました。 新しい管理者として、ユーザーへの展開を開始する前に、PowerShell を使用して一部の Microsoft 365 サービスを管理してみたいと思っています。

## <a name="objectives"></a>目標

このラボを完了すると、次のことができるようになります。

- Azure AD でユーザーを管理する。
- Exchange Online を管理する。
- SharePoint Online の管理。
- Microsoft Teams を管理する。

## <a name="estimated-time-60-minutes"></a>予想所要時間: 60 分

## <a name="lab-setup"></a>ラボのセットアップ

仮想マシン:**LON-DC1**、**LON-CL1**

ユーザー名: **Adatum\\Administrator**

パスワード: **Pa55w.rd**

このラボでは、使用可能な仮想マシン環境を使います。 ラボを開始する前に、次の手順を行ってください。

1. **LON-DC1** を開き、パスワード **Pa55w.rd** を使って **Adatum\\Administrator** としてサインインします。
1. **LON-CL1** について手順 1 を繰り返します。

> **注**: このラボでは、Office 365 テナントと、そのテナントでグローバル管理者のアクセス許可を持つユーザーが必要です。

## <a name="exercise-1-managing-users-and-groups-in-azure-ad"></a>演習 1: Azure AD でのユーザーとグループの管理

### <a name="exercise-scenario-1"></a>演習のシナリオ 1

あなたは、ユーザーを作成および管理する方法を理解していることを確認する必要があります。 特に、ライセンスをユーザーに割り当てる方法を学習することに関心があります。 この演習では、管理ユーザーと標準ユーザーを作成します。 また、グループを作成して設定します。

この演習の主なタスクは次のとおりです。

1. Azure AD に接続します。
1. 新しい管理ユーザーを作成する。
1. 新しいユーザーを作成してライセンスを付与する。

### <a name="task-1-connect-to-azure-ad"></a>タスク 1: Azure AD に接続する

1. **LON-CL1** で、管理者として Windows PowerShell を開きます。
1. Azure AD を管理できるようにするモジュールをインストールします。
1. Azure AD に接続し、管理者ユーザー アカウントを使用してサインインします。
1. 接続されていることを確認するには、Azure AD 内のユーザーの一覧を確認します。

### <a name="task-2-create-a-new-administrative-user"></a>タスク 2: 新しい管理ユーザーを作成する

1. 変数に新しい **PasswordProfile** オブジェクトを作成し、**password** プロパティを設定します。
1. 忘れないようにパスワードをメモします。
1. 検証済みの Azure AD ドメインの名前を識別し、$verifiedDomain という名前の変数にそれを保存します。
1. 次の属性を持つ **PasswordProfile** オブジェクトを使用して、新しいユーザー オブジェクトを作成します。
   - 表示名: **Noreen Riggs**
   - ユーザー プリンシパル名:**Noreen@$verifiedDomain**
   - アカウントが有効
   - MailNickName: **Noreen**
1. グローバル管理者ロールを **Noreen Riggs** ユーザー アカウントに割り当てます。
1. **Get-AzureADDirectoryRoleMember** コマンドレットを使用して、グローバル管理者ロールが **Noreen Riggs** ユーザー アカウントに割り当てられていることを確認します。

### <a name="task-3-create-and-license-a-new-user"></a>タスク 3: 新しいユーザーを作成してライセンスを取得する

1. 次の属性を持つ **PasswordProfile** オブジェクトを使用して、新しいユーザー オブジェクトを作成します。
   - 表示名: **Allan Yoo**
   - ユーザー プリンシパル名:**Allan@$verifiedDomain**
   - アカウントが有効
   - MailNickName: **Allan**
1. Allan Yoo の利用場所を **[US]** に設定します。
1. テナントで使用可能なライセンス SKU を一覧表示します。
1. **AssignedLicense** オブジェクトを作成し、テナントの **ENTERPRISEPREMIUM** ライセンスの *SkuID* 値を使用して、**SkuId** プロパティを構成します。
1. **AssignedLicenses** オブジェクトを作成し、**AssignedLicense** オブジェクトを **AddLicenses** プロパティに配置します。
1. **AssignedLicenses** オブジェクトを使用して、Allan Yoo にライセンスを割り当てます。

### <a name="task-4-create-and-populate-a-group"></a>タスク 4: グループを作成して設定する

1. Azure AD 内のグループを一覧表示します。
1. 次の属性を使用して、Azure AD に新しいグループ オブジェクトを作成します。
   - 表示名: **Sales Security Group**
   - セキュリティが有効: `$true`
   - メールが有効: `$false`
   - メール ニックネーム: **SalesSecurityGroup**
1. Allan Yoo のユーザー オブジェクトを Sales Security Group のメンバーとして追加します。
1. Sales Security Group メンバーシップに対してクエリを実行し、Allan Yoo のユーザー オブジェクトがそのメンバーであることを確認します。
1. **[Windows PowerShell]** ウィンドウは開いたままにしておきます。

## <a name="exercise-2-managing-exchange-online"></a>演習 2: Exchange Online の管理

### <a name="scenario-2"></a>シナリオ 2

この演習では、新しい会議室メールボックスを作成し、会議要求を自動的に受け入れるようにリソース予約プロパティを構成します。 また、リソース予約が期待したとおりに動作することを確認します。

この演習の主なタスクは次のとおりです。

1. Exchange Online に接続する。
1. 会議室メールボックスを作成する。
1. 会議室のリソース予約を確認する。

### <a name="task-1-connect-to-exchange-online"></a>タスク 1: Exchange Online に接続する

1. **LON-CL1** で、同じ **[Windows PowerShell]** ウィンドウに、Exchange Online を管理できるモジュールをインストールします。
1. Exchange Online に接続し、管理者ユーザー アカウントを使用してサインインします。
1. 接続されていることを確認するには、Exchange Online 内のメールボックスの一覧を確認します。

### <a name="task-2-create-a-room-mailbox"></a>タスク 2: 会議室メールボックスを作成する

1. **BoardRoom** という名前の会議室メールボックスを作成します。
1. 会議要求を自動的に受け入れるように **BoardRoom** の予定表の処理を構成します。

### <a name="task-3-verify-room-resource-booking"></a>タスク 3: 会議室のリソース予約を確認する

1. Microsoft Edge ブラウザーを開き、 **https://outlook.office.com** に移動します。
2. **Allan Yoo** としてサインインします。
3. 予定表から、新しいイベントを作成し、出席者として **BoardRoom** を含めます。
4. 使用可能な時間を選択し、会議の招待状を送信します。
5. Outlook 受信トレイで、会議要求が受理されたという応答を受信したことを確認します。

## <a name="exercise-3-managing-sharepoint-online"></a>演習 3: SharePoint Online の管理

SharePoint Online の管理に使用される Web ベースの管理コンソールを使い慣れています。 これはあなたのニーズを十分に満たしますが、プロセスを自動化したい場合に備えて、Windows PowerShell を使用してサイトを作成するためのプロセスをテストする必要があります。

この演習の主なタスクは次のとおりです。

1. SharePoint Online に接続する。
1. 新しいサイトを作成する。

### <a name="task-1-connect-to-sharepoint-online"></a>タスク 1: SharePoint Online に接続する

1. **LON-CL1** で、同じ **[Windows PowerShell]** ウィンドウに、SharePoint Online を管理できるモジュールをインストールします。
1. SharePoint Online に接続し、**Noreen Riggs** としてサインインします。
1. 接続を確認するには、テナント内のサイトを一覧表示します。

### <a name="task-2-create-a-new-site"></a>タスク 2: 新しいサイトを作成する

1. **Windows PowerShell** コンソールで、SharePoint で使用可能なテンプレートの一覧を確認します。
1. 次の設定を使用して新しい SharePoint サイトを作成します。

   - URL: `https://yourdomain.sharepoint.com/sites/Sales`
   - 所有者: `noreen@yourdomain.onmicrosoft.com`
   - ストレージ クォータ: **256**
   - テンプレート: **EHS#1**

   > **注:** サイトの作成には 10 分以上かかる場合があります。 **-NoWait** パラメーターを使用して、サイトを非同期的に作成します。 

1. **Get-SPOSite** を使用して、新しいサイトの状態を確認します。
1. SharePoint Online から切断します。
1. **[Windows PowerShell]** ウィンドウは開いたままにしておきます。

## <a name="exercise-4-managing-microsoft-teams"></a>演習 4: Microsoft Teams の管理

営業部門は、コラボレーションに Microsoft Teams を使用することに関心を持っています。 あなたは、彼らの Microsoft Teams の使用経験を支援するために、PowerShell を使用してチームを作成します。 その後、営業部門から Allan Yoo へのアクセス権を付与します。

この演習の主なタスクは次のとおりです。

1. Microsoft Teams に接続します。
1. 新しいチーム プロジェクトを作成します。
1. チームへのアクセス権を確認します。

### <a name="task-1-connect-to-microsoft-teams"></a>タスク 1: Microsoft Teams に接続する

1. **LON-CL1** で、同じ **[Windows PowerShell]** ウィンドウに、Microsoft Teams を管理できるモジュールをインストールします。
1. Microsoft Teams に接続し、管理者ユーザー アカウントを使用してサインインします。
1. 接続を確認するには、テナント内のサイトを一覧表示します。
1. **Get-Team** コマンドレットを使用して、既存のチームがないことを確認します。

### <a name="task-2-create-a-new-team"></a>タスク 2: 新しいチームを作成する

1. 次のプロパティを使用して新しいチームを作成します。

   - 表示名: **Sales Team**
   - MailNickName: **SalesTeam**

2. **Sales Team** に **Allan Yoo** をユーザーとして追加します
3. Sales Team のユーザーを一覧表示し、Allan Yoo が追加されたことを確認します。

   > **注:** チームを作成したユーザーは所有者であることに注意してください。

### <a name="task-3-verify-access-to-the-team"></a>タスク 3: チームへのアクセスを確認する

1. Microsoft Edge を開き、 **https://teams.microsoft.com** に移動します。
2. Allan Yoo としてサインインします。
3. Sales Team で、テキスト "**価格は月末に 10% 増加している**" を含む新しい会話を作成します。
