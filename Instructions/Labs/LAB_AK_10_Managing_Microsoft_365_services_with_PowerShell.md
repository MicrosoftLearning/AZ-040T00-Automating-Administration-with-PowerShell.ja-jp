---
lab:
  title: 'ラボ: PowerShell を使用した Microsoft 365 の管理'
  type: Answer Key
  module: 'Module 10: Managing Microsoft 365 services with PowerShell'
ms.openlocfilehash: bed969d7321c01a318d873fc5fb9682c269bd0d4
ms.sourcegitcommit: dca48f28a6753becf280d7814397cb9a415fa951
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/19/2022
ms.locfileid: "145180220"
---
# <a name="lab-answer-key-managing-microsoft-365-with-powershell"></a>ラボの回答キー: PowerShell を使用した Microsoft 365 の管理

## <a name="exercise-1-managing-users-and-groups-in-azure-ad"></a>演習 1: Azure AD でのユーザーとグループの管理

### <a name="task-1-connect-to-azure-ad"></a>タスク 1: Azure AD に接続する

1. **LON-CL1** で、**[スタート]** を選択して、「**powersh**」と入力します。
1. 結果の一覧で、 **[Windows PowerShell]** を右クリックするか、またはそのコンテキスト メニューをアクティブにして、 **[管理者として実行]** を選択します。
1. **AzureAD** モジュールをインストールするには、 **[管理者: Windows PowerShell]** コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Install-Module AzureAD
   ```

1. Azure AD に接続するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Connect-AzureAD
   ```

1. **[アカウントにサインインする]** ウィンドウで、全体管理者のアクセス許可を持つユーザー名を入力して、 **[次へ]** を選択します。
1. **[パスワードの入力]** プロンプトで、自分のパスワードを入力して、 **[サインイン]** を選択します。
1. Azure AD でユーザーの一覧を確認するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-AzureADUser
   ```

### <a name="task-2-create-a-new-administrative-user"></a>タスク 2: 新しい管理ユーザーを作成する

1. パスワード プロファイル オブジェクトを作成するには、**Windows PowerShell** コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $PasswordProfile = New-Object -TypeName Microsoft.Open.AzureAD.Model.PasswordProfile
   ```

1. 新しいユーザーで使用するパスワードを考え、忘れないようにメモします。 次の手順では、`<password>` を、自分で考えたパスワードに置き換えます。
1. パスワード プロファイル オブジェクトの password プロパティを設定するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $PasswordProfile.Password = "<password>"
   ```

1. 新しい Azure AD ユーザーを作成するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   New-AzureADUser -DisplayName "Noreen Riggs" -UserPrincipalName Noreen@yourdomain.onmicrosoft.com -AccountEnabled $true -PasswordProfile $PasswordProfile -MailNickName Noreen
   ```

1. 新しいユーザーを変数内に格納するには、コンソールで次のコマンドを入力して Enter キーを押します。

   ```powershell
   $user = Get-AzureADUser -ObjectID Noreen@yourdomain.onmicrosoft.com
   ```

1. 全体管理者ロールを変数内に格納するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $role = Get-AzureADDirectoryRole | Where {$_.displayName -eq 'Global Administrator'}
   ```

1. 全体管理者ロールに Noreen を割り当てるには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId $user.ObjectID
   ```

1. Noreen が全体管理者ロールに追加されていることを確認するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
   ```

### <a name="task-3-create-and-license-a-new-user"></a>タスク 3: 新しいユーザーを作成してライセンスを取得する

1. 新しい Azure AD ユーザーを作成するには、**Windows PowerShell** コンソールで、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   New-AzureADUser -DisplayName "Allan Yoo" -UserPrincipalName Allan@yourdomain.onmicrosoft.com -AccountEnabled $true -PasswordProfile $PasswordProfile -MailNickName Allan
   ```

1. ユーザーの場所を設定するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Set-AzureADUser -ObjectId Allan@yourdomain.onmicrosoft.com -UsageLocation US
   ```

1. テナント内で使用可能なライセンスを確認するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-AzureADSubscribedSku | FL
   ```

1. 目的のライセンスの SKU ID を変数内に格納するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $SkuId = (Get-AzureADSubscribedSku | Where SkuPartNumber -eq "ENTERPRISEPREMIUM").SkuID
   ```

1. **AssignedLicense** オブジェクトを作成するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $License = New-Object -TypeName Microsoft.Open.AzureAD.Model.AssignedLicense
   ```

1. SKU ID をライセンス オブジェクトに追加するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $License.SkuId = $SkuId
   ```

1. **AssignedLicenses** オブジェクトを作成するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $LicensesToAssign = New-Object -TypeName Microsoft.Open.AzureAD.Model.AssignedLicenses
   ```

1. **AddLicenses** プロパティに **AssignedLicense** オブジェクトを追加するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $LicensesToAssign.AddLicenses = $License
   ```

1. **AssignedLicenses** オブジェクトを使用して Allan を構成するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Set-AzureADUserLicense -ObjectId Allan@yourdomain.onmicrosoft.com -AssignedLicenses $LicensesToAssign
   ```

### <a name="task-4-create-and-populate-a-group"></a>タスク 4: グループを作成して設定する

1. 既存のグループを確認するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-AzureADGroup
   ```

1. 新しいセキュリティ グループを作成するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   New-AzureADGroup -DisplayName "Sales Security Group" -SecurityEnabled $true -MailEnabled $false -MailNickName "SalesSecurityGroup"
   ```

1. Sales Security Group を変数内に格納するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $group = Get-AzureAdGroup -SearchString "Sales Security"
   ```

1. Allan Yoo を変数内に格納するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $user = Get-AzureADUser -ObjectId Allan@yourdomain.onmicrosoft.com
   ```

1. Allan Yoo を Sales Security Group のメンバーとして追加するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Add-AzureADGroupMember -ObjectId $group.ObjectId -RefObjectId $user.ObjectId
   ```

1. Allan Yoo が Sales Security Group のメンバーになっていることを確認するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-AzureADGroupMember -ObjectId $group.ObjectId
   ```

## <a name="exercise-2-managing-exchange-online"></a>演習 2: Exchange Online の管理

### <a name="task-1-connect-to-exchange-online"></a>タスク 1: Exchange Online に接続する

1. **LON-CL1** で、**[スタート]** を選択して、「**powersh**」と入力します。
1. 結果の一覧で、 **[Windows PowerShell]** を右クリックするか、またはそのコンテキスト メニューをアクティブにして、 **[管理者として実行]** を選択します。
1. **ExchangeOnlineManagement** モジュールをインストールするには、**管理者: Windows PowerShell** コンソールで、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Install-Module ExchangeOnlineManagement
   ```

1. Exchange Online に接続するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Connect-ExchangeOnline
   ```

1. **[アカウントにサインインする]** ウィンドウで、ユーザー名を入力して、 **[次へ]** を選択します。
1. **[パスワードの入力]** プロンプトで、自分のパスワードを入力して、 **[サインイン]** を選択します。
1. Exchange Online でメールボックスの一覧を確認するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-EXOMailbox
   ```

### <a name="task-2-create-a-room-mailbox"></a>タスク 2: 会議室メールボックスを作成する

1. 新しい会議室メールボックスを作成するには、**Windows PowerShell** コンソールで、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   New-Mailbox -Room -Name BoardRoom
   ```

1. 会議出席依頼を受け入れるように新しい会議室を構成するには、コンソールで、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Set-CalendarProcessing BoardRoom -AutomateProcessing AutoAccept
   ```

### <a name="task-3-verify-room-resource-booking"></a>タスク 3: 会議室のリソース予約を確認する

1. **LON-CL1** のタスク バーで **[Microsoft Edge]** を選択します。
1. Microsoft Edge のアドレス バーに、「**https://outlook.office.com**」と入力して、Enter キーを押します。
1. Allan Woo としてサインインし、指示に従ってパスワードを変更します。 後の演習で思い出すことができるように、必ずパスワードをメモしてください。
1. サインインしたままにするかどうかを尋ねられたら、「**いいえ**」を選択します。
1. メニュー バーから **[カレンダー]** を選択して、 **[新しいイベント]** を選択します。
1. **[タイトルの追加]** ボックスに、「**スタッフ会議**」 と入力します。
1. **[出席者の招待]** ボックスに、「**BoardRoom**」と入力し、**[BoardRoom]** を選択し、最初に使用可能な時刻を選択して、**[送信]** を選択します。
1. メニューから **[メール]** を選択します。
1. 会議出席依頼が受け入れられたという応答を、Allan が **BoardRoom** から受信していることを確認します。
1. [Microsoft Edge] を閉じます。

## <a name="exercise-3-managing-sharepoint-online"></a>演習 3: SharePoint Online の管理

### <a name="task-1-connect-to-sharepoint-online"></a>タスク 1: SharePoint Online に接続する

1. Windows PowerShell コンソールを閉じます。

1. **[スタート]** を選択して、「**powersh**」と入力します。

1. 結果の一覧で、 **[Windows PowerShell]** を右クリックするか、またはそのコンテキスト メニューをアクティブにして、 **[管理者として実行]** を選択します。

1. SharePoint Online Management Shell をインストールするには、**Windows PowerShell** コンソールで、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Install-Module -Name Microsoft.Online.SharePoint.PowerShell
   ```

1. リポジトリを信頼するように求めるメッセージが表示されたら、「**A**」と入力して、Enter キーを押します。

1. SharePoint Online に接続するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Connect-SPOService -Url https://yourdomain-admin.sharepoint.com
   ```

1. Noreen Riggs としてサインインし、指示に従ってパスワードを変更します。 後の演習で思い出すことができるように、必ずパスワードをメモしてください。

1. 既存のサイトを確認するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-SPOSite
   ```

### <a name="task-2-create-a-new-site"></a>タスク 2: 新しいサイトを作成する

1. 使用可能なテンプレートを確認するには、**Windows PowerShell** コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-SPOWebTemplate
   ```

1. 新しいサイトを作成するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   New-SPOSite -Url https://yourdomain.sharepoint.com/sites/Sales -Owner noreen@yourdomain.onmicrosoft.com -StorageQuota 256 -Template EHS#1
   ```

   > **注:** このコマンドが処理を完了するまで数分かかる場合があります。

1. SharePoint サイトの状態を確認するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-SPOSite | FL Url,Status
   ```

   > **注:** サイトの作成には 10 分以上かかる場合があります。 サイトの作成が完了するまで待たないでください。

1. SharePoint Online から切断するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Disconnect-SPOService
   ```

## <a name="exercise-4-managing-microsoft-teams"></a>演習 4: Microsoft Teams の管理

### <a name="task-1-connect-to-microsoft-teams"></a>タスク 1: Microsoft Teams に接続する

1. Microsoft Teams PowerShell Module をインストールするには、 **[Windows PowerShell]** コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Install-Module MicrosoftTeams
   ```

1. リポジトリを信頼するように求めるメッセージが表示されたら、「**A**」と入力して、Enter キーを押します。
1. Microsoft Teams に接続するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Connect-MicrosoftTeams
   ```

1. 管理者ユーザーとしてサインインします。 チームを作成するには Microsoft Teams ライセンスが必要であるため、このアクティビティには Noreen を使用できないことに注意してください。
1. 既存のチームがないことを確認するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-Team
   ```

### <a name="task-2-create-a-new-team"></a>タスク 2: 新しいチームを作成する

1. 営業チームを作成するには、**Windows PowerShell** コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   New-Team -DisplayName "Sales Team" -MailNickName "SalesTeam"
   ```

1. チーム情報を変数内に格納するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $team = Get-Team -DisplayName "Sales Team"
   ```

1. 自分のチームに関する情報を確認するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $team | FL
   ```

1. 営業チームに関する情報を確認します。 **GroupId** は一意の識別子であることに注意してください。
1. チームにユーザーを追加するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Add-TeamUser -GroupId $team.GroupId -User Allan@yourdomain.onmicrosoft.com -Role Member
   ```

1. チーム ユーザーを確認するには、コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-TeamUser -GroupId $team.GroupId
   ```

   > **注:** チームを作成したユーザーが所有者であることに注意してください。

## <a name="task-3-verify-access-to-the-team"></a>タスク 3: チームへのアクセスを確認する

1. **LON-CL1** のタスク バーで **[Microsoft Edge]** を選択します。
1. Microsoft Edge のアドレス バーに、「**https://teams.microsoft.com**」と入力して、Enter キーを押します。
1. Allan Yoo としてサインインします。
1. サインインしたままにするかどうかを尋ねられたら、「**いいえ**」を選択します。
1. **[チームをまとめましょう]** ウィンドウを閉じて、**営業チーム** が一覧に表示されていることを確認します。
1. **[新しい会話]** を選択し、「**価格は月末に 10% 増加します**」と入力し、Enter キーを押します。
1. [Microsoft Edge] を閉じます。
