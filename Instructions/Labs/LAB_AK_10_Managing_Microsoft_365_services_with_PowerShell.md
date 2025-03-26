---
lab:
  title: 'ラボ: PowerShell を使用した Microsoft 365 の管理'
  type: Answer Key
  module: 'Module 10: Managing Microsoft 365 services with PowerShell'
---

# PowerShell を使用した Microsoft 365 の管理

## 演習 1:Microsoft Entra ID でユーザーとグループを管理する

### タスク 1:Microsoft Entra ID に接続する

Microsoft Graph PowerShell は PowerShell 7 以降で動作します。 また Windows PowerShell 5.1 と互換性があります

Windows PowerShell で Microsoft Graph PowerShell SDK を使用するには、次の前提条件が必要です。

- PowerShell 5.1 以降にアップグレードする

PowerShell スクリプト実行ポリシーは、リモート署名または制限の緩いものに設定する必要があります。 Get-ExecutionPolicy を使用して、現在の実行ポリシーを特定します。 詳細については、**[Microsoft Graph PowerShell SDK のインストール](https://learn.microsoft.com/en-us/powershell/microsoftgraph/installation?view=graph-powershell-1.0)** に関する記事を参照してください。

1. **LON-CL1** で、**[スタート]** を選択してから、「**Windows PowerShell**」と入力します。

1. 結果リストで、**[Windows PowerShell]** を右クリックするか、そのコンテキスト メニューをアクティブにしてから、**[管理者として実行]** を選択します。

1. PowerShell スクリプト実行ポリシーはリモート署名または制限の緩いものに設定する必要があり、実行ポリシーを設定するには、次のコマンドを実行し、「`A`」と入力して Enter キーを押します。

   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
   ```

1. **Microsoft.Graph** モジュールをインストールするには、**[管理者: Windows PowerShell]** コンソールで、次のコマンドを入力し、Enter キーを押します (確認のプロンプトが表示されたら、「`Y`」と入力し、Enter キーを続けて 2 回押します)。
   
   ```powershell
   Install-Module Microsoft.Graph -Scope CurrentUser
   ```

1. インストールが完了した後、次のコマンドを使用して、インストールされているバージョンを確認できます。

    ```powershell
   Get-InstalledModule Microsoft.Graph
   ```

1. Microsoft.Graph に接続するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   # Using interactive authentication for users, groups, teamsettings, RoleManagement.
   Connect-MgGraph -Scopes "User.ReadWrite.All", "Application.ReadWrite.All", "Sites.ReadWrite.All", "Directory.ReadWrite.All", "Group.ReadWrite.All", "RoleManagement.ReadWrite.Directory"
   ```

1. **[アカウントにサインインする]** ウィンドウで、このラボで使用する Microsoft Entra ID テナントの全体管理者の役割を持つユーザー アカウントの名前を入力し、**[次へ]** を選択します。

1. **[パスワードの入力]** プロンプトで、自分のパスワードを入力して、**[サインイン]** を選択します。

1. Microsoft Entra ID でユーザーの一覧を確認するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-MgUser
   ```

### タスク 2: 新しい管理ユーザーを作成する

1. 組織の確認済みドメインを取得するには、**[管理者: Windows PowerShell]** コンソールで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $verifiedDomain = (Get-MgOrganization).VerifiedDomains[0].Name
   ```

1. 新しいユーザーのパスワード プロファイルを作成するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $PasswordProfile = @{  
    "Password"="<password>";  
    "ForceChangePasswordNextSignIn"=$true  
   }  
   ```

   >**注:**  このラボの後半で必要になるため、新しいユーザーに使用する任意の複雑なパスワードを特定し、記録します。 小文字、大文字、数字、および少なくとも 1 つの特殊文字の組み合わせを含めて、パスワードの長さが 8 文字以上になるようにします。
   次のステップでは、`<password>` を、使用するパスワードに置き換えます。

1. 新しい Microsoft Entra ID ユーザーを作成するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   New-MgUser -DisplayName "Noreen Riggs" -UserPrincipalName "Noreen@$verifiedDomain" -AccountEnabled -PasswordProfile $PasswordProfile -MailNickName "Noreen"
   ```

1. 新しいユーザーへの参照を変数内に格納するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $user = Get-MgUser -UserId "Noreen@$verifiedDomain"
   ```

1. 新しい全体管理者の役割への参照を変数内に格納するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $role = Get-MgDirectoryRole | Where {$_.displayName -eq 'Global Administrator'}
   ```

1. Noreen のユーザー アカウントに全体管理者の役割を割り当てるには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $OdataId = "https://graph.microsoft.com/v1.0/directoryObjects/" + $user.id  
   New-MgDirectoryRoleMemberByRef -DirectoryRoleId $role.id -OdataId $OdataId    
   ```

1. Noreen のユーザー アカウントに全体管理者の役割が割り当てられたことを確認するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-MgDirectoryRoleMember -DirectoryRoleId $role.id
   ```

1. コマンドの出力で、Noreen のユーザー アカウントの **UserPrincipalName** 属性を特定し、記録します。 このラボの後の演習のいずれかで必要になります。

### タスク 3: 新しいユーザーを作成してライセンスを付与する

1. 別の Microsoft Entra ユーザーを作成するには、**[管理者: Windows PowerShell]** コンソールで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   New-MgUser -DisplayName "Allan Yoo" -UserPrincipalName Allan@$verifiedDomain -AccountEnabled -PasswordProfile $PasswordProfile -MailNickName "Allan"
   ```

1. 新しく作成したユーザー アカウントの location プロパティを設定するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Update-MgUser -UserId Allan@$verifiedDomain -UsageLocation US
   ```

1. テナント内で使用可能なライセンスを確認するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-MgSubscribedSku | FL
   ```

1. 目的のライセンスの SKU ID を変数内に格納するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $SkuId = (Get-MgSubscribedSku | Where-Object { $_.SkuPartNumber -eq "ENTERPRISEPREMIUM" }).SkuId
   ```

1. **AssignedLicense** オブジェクトを作成するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $License = New-Object -TypeName PSCustomObject -Property @{
    DisabledPlans = @()
    SkuId = $SkuId
   }
   ```

1. ライセンス オブジェクトに SKU ID を追加するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $License.SkuId = $SkuId 
   ```

1. **AssignedLicenses** オブジェクトを作成するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $LicensesToAssign = @($License)  
   ```

1. **AddLicenses** プロパティに **AssignedLicense** オブジェクトを追加するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $LicensesToAssign += $License
   ```

1. Allan のユーザー オブジェクトに **AssignedLicenses** オブジェクトを構成するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Set-MgUserLicense -UserId Allan@$verifiedDomain -AddLicenses @{SkuId = $SkuId} -RemoveLicenses @()
   ```

### タスク 4: グループを作成して設定する

1. 既存のグループを確認するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-MgGroup
   ```

1. 新しいセキュリティ グループを作成するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   New-MgGroup -DisplayName "Sales Security Group" -MailEnabled:$False  -MailNickName "SalesSecurityGroup" -SecurityEnabled
   ```

1. 販売セキュリティ グループへの参照を変数内に格納するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $group = Get-MgGroup -ConsistencyLevel eventual -Count groupCount -Search '"DisplayName:Sales Security"'
   ```

1. Allan Yoo のユーザー アカウントへの参照を変数内に格納するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $user = Get-MgUser -UserId Allan@$verifiedDomain
   ```

1. Allan Yoo のユーザー アカウントを販売セキュリティ グループに追加するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   New-MgGroupMember -GroupId $group.id -DirectoryObjectId $user.id
   ```

1. Allan Yoo のユーザー アカウントが販売セキュリティ グループのメンバーであることを確認するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-MgGroupMember -GroupId $group.id
   ```

1. コマンドの出力で、Allan のユーザー アカウントの **UserPrincipalName** 属性を特定し、記録します。 これは、次の演習で必要になります。

## 演習 2:Exchange Online を管理する

### タスク 1: Exchange Online に接続する

1. **LON-CL1** に **ExchangeOnlineManagement** モジュールをインストールするには、**[管理者: Windows PowerShell]** コンソールで、次のコマンドを入力し、Enter キーを押します (確認のプロンプトが表示されたら、「`A`」と入力し、Enter キーをもう一度押します)。

   ```powershell
   Install-Module ExchangeOnlineManagement -force
   ```

1. Exchange Online に接続するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Connect-ExchangeOnline
   ```

1. **[アカウントにサインインする]** ウィンドウで、このラボの前の演習で使用していたのと同じユーザー アカウントの名前を入力し、**[次へ]** を選択します。

1. **[パスワードの入力]** プロンプトで、自分のパスワードを入力して、**[サインイン]** を選択します。

1. Exchange Online でメールボックスの一覧を確認するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-EXOMailbox
   ```

### タスク 2: 会議室メールボックスを作成する

1. 新しい会議室メールボックスを作成するには、**[Windows PowerShell]** コンソールで、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   New-Mailbox -Room -Name BoardRoom
   ```

1. 会議出席依頼を受け入れるように新しい会議室を構成するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Set-CalendarProcessing BoardRoom -AutomateProcessing AutoAccept
   ```

### タスク 3: 会議室のリソース予約を確認する

1. **LON-CL1** のタスク バーで **[Microsoft Edge]** を選択します。

1. Microsoft Edge のアドレス バーに、「`https://outlook.office.com`」と入力して、Enter キーを押します。

1. ユーザー名として UserPrincipalName を使用し、このラボの前の演習で記録したパスワードを指定して、Allan Yoo としてサインインします。 プロンプトが表示されたら、指示に従ってパスワードを変更します。 以降の演習で使用できるように、必ずパスワードを記録してください。

1. サインインしたままにするかどうかを尋ねられたら、**[いいえ]** を選択します。

1. メニュー バーから **[カレンダー]** を選択して、**[新しいイベント]** を選択します。

1. **[タイトルの追加]** ボックスに、「**スタッフ会議**」 と入力します。

1. **[出席者の招待]** ボックスに、「**BoardRoom**」と入力し、**[BoardRoom]** を選択し、最初に使用可能な時刻を選択して、**[送信]** を選択します。

1. メニューから **[メール]** を選択します。

1. 会議出席依頼が受け入れられたという応答を、Allan が **BoardRoom** から受信していることを確認します。

1. Allan のユーザー アカウントからサインアウトします。

1. [Microsoft Edge] を閉じます。

## 演習 3:SharePoint Online を管理する

### タスク 1: SharePoint Online に接続する

1. SharePoint Online Management Shell をインストールするには、**LON-CL1** の **[管理者: Windows PowerShell]** コンソールで、次のコマンドを入力し、Enter キーを押します (確認のプロンプトが表示されたら、「`A`」と入力し、Enter キーをもう一度押します)。

   ```powershell
   Install-Module -Name Microsoft.Online.SharePoint.PowerShell -Scope CurrentUser
   ```

1. SharePoint Online に接続するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $verifiedDomainShort = $verifiedDomain.Split(".")[0]
   Connect-SPOService -Url "https://$verifiedDomainShort-admin.sharepoint.com"
   ```

1. プロンプトが表示されたら、Noreen Riggs としてサインインし、指示に従ってパスワードを変更します。 以降の演習で使用できるように、必ずパスワードを記録してください。

1. 既存の SharePoint Online サイトを一覧表示するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-SPOSite
   ```

### タスク 2: 新しいサイトを作成する

1. 使用可能なテンプレートを確認するには、**[Windows PowerShell]** コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-SPOWebTemplate
   ```

1. 新しいサイトを作成するには、次のコマンドを入力して Enter キーを押します。

   ```powershell
   New-SPOSite -Url https://$verifiedDomainShort.sharepoint.com/sites/Sales -Owner noreen@$verifiedDomain -StorageQuota 256 -Template EHS#1 -NoWait
   ```

   > **注:** サイトの作成には 10 分以上かかる場合があります。 **-NoWait** パラメーターは、このタスクを非同期的に実行するため、完了を待つ必要はありません。 待機する場合は、次のコマンドを入力し、Enter キーを押して SharePoint サイトの状態を確認できます。

   ```powershell
   Get-SPOSite | FL Url,Status
   ```

1. SharePoint Online から切断するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Disconnect-SPOService
   ```

## 演習 4:Microsoft Teams を管理する

### タスク 1: Microsoft Teams に接続する

1. Microsoft Teams PowerShell Module をインストールするには、**[管理者: Windows PowerShell]** コンソールで、次のコマンドを入力し、Enter キーを押します (確認のプロンプトが表示されたら、「`A`」と入力し、Enter キーをもう一度押します)。

   ```powershell
   Install-Module -Name MicrosoftTeams -Force -AllowClobber
   ```

1. Microsoft Teams に接続するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Connect-MicrosoftTeams
   ```

1. **[アカウントにサインインする]** ウィンドウで、**[他のアカウントを使用する]** オプションを選び、このラボで使用した Microsoft Entra ID テナントの全体管理者の役割を持つユーザー アカウントの名前を入力し、**[次へ]** を選択します。

1. 既存のチームがないことを確認するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-Team
   ```

### タスク 2: 新しいチームを作成する

1. 営業チームを作成するには、**[Windows PowerShell]** コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   New-Team -DisplayName "Sales Team" -MailNickName "SalesTeam"
   ```

1. チーム情報を変数内に格納するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $team = Get-Team -DisplayName "Sales Team"
   ```

1. 自分のチームに関する情報を確認するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   $team | FL
   ```

1. 営業チームに関する情報を確認します。 **GroupId** は一意の識別子であることに注意してください。

1. 組織の確認済みドメインを取得するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $verifiedDomain = (Get-MgOrganization).VerifiedDomains[0].Name
   ```

1. チームにユーザーを追加するには、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Add-TeamUser -GroupId $team.GroupId -User Allan@$verifiedDomain -Role Member
   ```

1. チーム ユーザーを確認するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-TeamUser -GroupId $team.GroupId
   ```

   > **注:** チームを作成したユーザーは所有者であることに注意してください。

## タスク 3: チームへのアクセスを確認する

1. **LON-CL1** のタスク バーで **[Microsoft Edge]** を選択します。

1. Microsoft Edge のアドレス バーに、「`https://teams.microsoft.com`」と入力して、Enter キーを押します。

1. ユーザー名として UserPrincipalName を使用し、このラボで前に変更したパスワードを指定して、Allan Yoo としてサインインします。 

1. サインインしたままにするかどうかを尋ねられたら、「**いいえ**」を選択します。

1. **[チームをまとめましょう]** ウィンドウを閉じて、**営業チーム**が一覧に表示されていることを確認します。

1. **[新しい会話]** を選択し、「**価格は月末に 10% 増加します**」と入力し、Enter キーを押します。

1. [Microsoft Edge] を閉じます。
