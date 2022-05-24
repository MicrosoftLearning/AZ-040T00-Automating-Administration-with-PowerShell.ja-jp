---
lab:
  title: 'ラボ: PowerShell パイプラインの使用'
  type: Answer Key
  module: 'Module 3: Working with the Windows PowerShell pipeline'
ms.openlocfilehash: 6c41faa9b0c0c7a3eb41b5ffe58c273ac12ed917
ms.sourcegitcommit: dca48f28a6753becf280d7814397cb9a415fa951
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/19/2022
ms.locfileid: "145180219"
---
# <a name="lab-using-powershell-pipeline"></a>ラボ: PowerShell パイプラインの使用

## <a name="exercise-1-selecting-sorting-and-displaying-data"></a>演習 1: データの選択、並べ替え、および表示

### <a name="task-1-display-the-current-day-of-the-year"></a>タスク 1: 現在の日付を表示する

1. **LON-CL1** で、 **[スタート]** を選択してから、「**PowerShell**」と入力します。
1. 検索結果で、 **[Windows PowerShell]** を右クリックするか、そのコンテキスト メニューをアクティブにしてから、 **[管理者として実行]** を選択します。
1. **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   help *date* 
   ```

   > **注:** **Get-Date** コマンドに注目してください。

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。  

   ```powershell
   Get-Date | Get-Member
   ```

   > **注:** **DayOfYear** プロパティに注目してください。

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-Date | Select-Object –Property DayOfYear
   ```

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-Date | Select-Object -Property DayOfYear | fl
   ```

### <a name="task-2-display-information-about-installed-hotfixes"></a>タスク 2: インストールされている修正プログラムに関する情報を表示する

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-Command *hotfix* 
   ```

   > **注:** **Get-Hotfix** コマンドに注目してください。

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-Hotfix | Get-Member 
   ```

   > **注:** **Hotfix** オブジェクトのプロパティが表示されます。 必要に応じて、**Get-Hotfix** を実行し、通常はそれらのプロパティに表示される値の一部を確認します。

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。  

   ```powershell
    Get-Hotfix | Select-Object –Property HotFixID,InstalledOn,InstalledBy
   ```

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-Hotfix | Select-Object –Property HotFixID,@{n='HotFixAge';e={(New-TimeSpan -Start $PSItem.InstalledOn).Days}},InstalledBy
   ```

### <a name="task-3-display-a-list-of-available-scopes-from-the-dhcp-server"></a>タスク 3: DHCP サーバーから使用可能なスコープのリストを表示する

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   help *scope* 
   ```

   > **注:** **Get-DHCPServerv4Scope** コマンドに注目してください。

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Help Get-DHCPServerv4Scope –ShowWindow 
   ```

   > **注:** 使用可能なパラメーターに注目してください。

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-DHCPServerv4Scope –ComputerName LON-DC1
   ```

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-DHCPServerv4Scope –ComputerName LON-DC1 | Select-Object –Property ScopeId,SubnetMask,Name | fl
   ```

### <a name="task-4-display-a-sorted-list-of-enabled-windows-firewall-rules"></a>タスク 4: 有効な Windows ファイアウォール規則の並べ替えられたリストを表示する

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

      ```powershell
      help *rule* 
      ```

   > **注:** **Get-NetFirewallRule** コマンドに注目してください。

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

      ```powershell
      Get-NetFirewallRule 
      ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

      ```powershell
      Help Get-NetFirewallRule –ShowWindow
      ```
  
1. コンソールで、次のコマンドを入力して Enter キーを押します。

      ```powershell
      Get-NetFirewallRule –Enabled True

      ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。
  
      ```powershell
      Get-NetFirewallRule –Enabled True | Format-Table -wrap
      ```

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

      ```powershell
      Get-NetFirewallRule –Enabled True | Select-Object –Property DisplayName,Profile,Direction,Action | Sort-Object –Property Profile, DisplayName | ft -GroupBy Profile
      ```

### <a name="task-5-display-a-sorted-list-of-network-neighbors"></a>タスク 5: ネットワーク近隣ノードの並べ替えられたリストを表示する

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

      ```powershell
      help *neighbor*  
      ```

   > **注:** **Get-NetNeighbor** コマンドに注目してください。

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

      ```powershell
      help Get-NetNeighbor –ShowWindow
      ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

      ```powershell
      Get-NetNeighbor
      ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

      ```powershell
      Get-NetNeighbor | Sort-Object –Property State
      ```

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

      ```powershell
      Get-NetNeighbor | Sort-Object –Property State | Select-Object –Property IPAddress,State | Format-Wide -GroupBy State -AutoSize
      ```

### <a name="task-6-display-information-from-the-dns-name-resolution-cache"></a>タスク 6: DNS 名前解決キャッシュからの情報を表示する

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

      ```powershell
      Test-NetConnection LON-DC1
      ```

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

      ```powershell
      help *cache* 
      ```

      > **注:** **Get-DnsClientCache** コマンドに注目してください。

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

      ```powershell
      Get-DnsClientCache
      ```

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

      ```powershell
      Get-DnsClientCache | Select Name,Type,TimeToLive | Sort Name | Format-List
      ```

      > **注:** **型** データからは、期待した内容 (A や CNAME など) が返されないことに注目してください。 代わりに、生の数値データが返されます。 各数値はレコードの型に直接マップされます。マップ (1= A、5 = CNAME など) がわかっている場合は、それらの型をフィルター処理できます。 このモジュールの後半では、さらにフィルターを追加して、数値とそれに対応するレコードの型を決定する方法について学習します。 **状態** データなど、他の返されるデータでも同じような状況になることがわかります。

1. 開いているすべてのウィンドウを閉じます。

### <a name="exercise-1-results"></a>演習 1 の結果

この演習を完了すると、環境からの管理情報を含むカスタム レポートがいくつか生成されているはずです。

## <a name="exercise-2-filtering-objects"></a>演習 2: オブジェクトのフィルター処理

### <a name="task-1-display-a-list-of-all-the-users-in-the-users-container-of-active-directory"></a>タスク 1: Active Directory の Users コンテナー内のすべてのユーザーのリストを表示する

1. **LON-CL1** で、 **[スタート]** を選択してから、「**PowerShell**」と入力します。
1. 検索結果で、 **[Windows PowerShell]** を右クリックするか、そのコンテキスト メニューをアクティブにしてから、 **[管理者として実行]** を選択します。
1. **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   help *user*
   ```

   > **注:** **Get-ADUser** コマンドに注目してください。

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-Help Get-ADUser –ShowWindow
   ```

   > **注:** *-Filter* パラメーターが必須であることに注目してください。 コマンドの例を確認します。
  
1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-ADUser –Filter * | ft
   ```

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-ADUser –Filter * -SearchBase "cn=Users,dc=Adatum,dc=com" | ft
   ```

### <a name="task-2-create-a-report-of-the-security-event-log-entries-that-have-the-event-id-4624"></a>タスク 2: イベント ID が 4624 のセキュリティ イベント ログ エントリのレポートを作成する

1. **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-EventLog -LogName Security | 
   Where EventID -eq 4624 | Measure-Object | fw
   ```

1. コンソールで、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-EventLog -LogName Security | 
   Where EventID -eq 4624 | 
   Select TimeWritten,EventID,Message
   ```

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-EventLog -LogName Security | 
   Where EventID -eq 4624 | 
   Select TimeWritten,EventID,Message -Last 10 | fl
   ```

### <a name="task-3-display-a-list-of-the-encryption-certificates-installed-on-the-computer"></a>タスク 3: コンピューターにインストールされている暗号化証明書のリストを表示する

1. **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-ChildItem -Path CERT: -Recurse
   ```

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-ChildItem -Path CERT: -Recurse | 
   Get-Member
   ```

1. コンソール ウィンドウで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-ChildItem -Path CERT: -Recurse | 
   Where HasPrivateKey -eq $False | Select-Object -Property FriendlyName,Issuer | fl
   ```

   または

   ```powershell
   Get-ChildItem -Path CERT: -Recurse | 
   Where { $PSItem.HasPrivateKey -eq $False } | Select-Object -Property FriendlyName,Issuer | fl
   ```

1. コンソール ウィンドウで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-ChildItem -Path CERT: -Recurse | 
   Where { $PSItem.HasPrivateKey -eq $False -and $PSItem.NotAfter -gt (Get-Date) -and $PSItem.NotBefore -lt (Get-Date) } | Select-Object -Property NotBefore,NotAfter, FriendlyName,Issuer | ft -wrap
   ```

### <a name="task-4-create-a-report-of-the-disk-volumes-that-are-running-low-on-space"></a>タスク 4: 領域が不足しているディスク ボリュームのレポートを作成する

1. **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-Volume
   ```

   > **注:** コマンド名がわからない場合は、**Help \*ボリューム\*** を実行して検出できます。

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-Volume | Get-Member
   ```

   > **注:** **SizeRemaining** プロパティに注目してください。

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-Volume | Where-Object { $PSItem.SizeRemaining -gt 0 } | fl
   ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-Volume | Where-Object { $PSItem.SizeRemaining -gt 0 -and $PSItem.SizeRemaining / $PSItem.Size -lt .99 }| Select-Object DriveLetter, @{n='Size';e={'{0:N2}' -f ($PSItem.Size/1MB)}}
   ```

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-Volume | Where-Object { $PSItem.SizeRemaining -gt 0 -and $PSItem.SizeRemaining / $PSItem.Size -lt .1 }
   ```

   > **注:** コンピューターの各ボリュームの空き領域が 10% を超える場合、このコマンドではラボ コンピューターに出力が生成されない可能性があります。

### <a name="task-5-create-a-report-that-displays-specified-control-panel-items"></a>タスク 5: 指定されたコントロール パネル項目を表示するレポートを作成する

1. **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   help *control* 
   ```

   > **注:** **Get-ControlPanelItem** コマンドに注目してください。

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-ControlPanelItem 
   ```

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-ControlPanelItem –Category 'System and Security' | Sort Name
   ```

    > **注:** **Where-Object** を使用する必要がないことに注目してください。

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-ControlPanelItem -Category 'System and Security' | Where-Object -FilterScript {-not ($PSItem.Category -notlike '*System and Security*')} | Sort Name
   ```

### <a name="exercise-2-results"></a>演習 2 の結果

この演習を完了すると、フィルター処理を使用して、指定されたデータと要素のみを含む管理情報のリストを生成したことになります。

## <a name="exercise-3-enumerating-objects"></a>演習 3: オブジェクトの列挙

### <a name="task-1-display-a-list-of-files-on-drive-e-of-your-computer"></a>タスク 1: コンピューターのドライブ E にあるファイルのリストを表示する

1. **LON-CL1** で、 **[スタート]** を選択してから、「**PowerShell**」と入力します。
1. 検索結果で、 **[Windows PowerShell]** を右クリックするか、そのコンテキスト メニューをアクティブにしてから、 **[管理者として実行]** を選択します。
1. **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-ChildItem -Path E: -Recurse
   ```

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-ChildItem -Path E: -Recurse | Get-Member 
   ```

   > **注:** **TypeName: System.IO.DirectoryInfo** の下にあるリストの **GetFiles** メソッドに注目してください。

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-ChildItem -Path E: -Recurse | ForEach GetFiles
   ```

### <a name="task-2-use-enumeration-to-produce-100-random-numbers"></a>タスク 2: 列挙を使用して 100 の乱数を生成する

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   help *random* 
   ```

   > **注:** **Get-Random** コマンドに注目してください。

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   help Get-Random –ShowWindow 
   ```

   > **注:** *-SetSeed* パラメーターに注目してください。

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   1..100 
   ```

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   1..100 | 
   ForEach { Get-Random –SetSeed $PSItem }
   ```

### <a name="task-3-run-a-method-of-a-windows-management-instrumentation-wmi-object"></a>タスク 3: Windows Management Instrumentation (WMI) オブジェクトのメソッドを実行する

1. **Windows PowerShell コンソール** 以外のすべてのアプリケーションを閉じます。
1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-WmiObject –Class Win32_OperatingSystem -EnableAllPrivileges
   ```

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-WmiObject –Class Win32_OperatingSystem -EnableAllPrivileges | 
   Get-Member
   ```

   > **注:** **Reboot** メソッドに注目してください。

   > **注:** 次のコマンドでは、実行しているコンピューターを再起動します。

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-WmiObject –Class Win32_OperatingSystem -EnableAllPrivileges | 
   ForEach Reboot
   ```

### <a name="exercise-3-results"></a>演習 3 の結果

この演習を完了すると、パイプライン内の複数のオブジェクトを操作するコマンドを作成したことになります。

## <a name="exercise-4-converting-objects"></a>演習 4: オブジェクトの変換

### <a name="task-1-update-active-directory-user-information"></a>タスク 1: Active Directory のユーザー情報を更新する

> **注:** このラボでは、通常、長いコマンドがいくつかの行に表示されます。 これは、コマンドの途中で意図しない改行が行われないようにするのに役立ちます。 しかし、これらのコマンドを入力するときは、1 行として入力する必要があります。 その行は、画面上で複数の行に折り返される場合がありますが、コマンドは引き続き機能します。 コマンド全体を入力した後にのみ、Enter キーを押します。

1. パスワード **Pa55w.rd** を使用して、**Adatum\\Administrator** として **LON-CL1** にサインインします。
1. **LON-CL1** で、 **[スタート]** を選択してから、「**PowerShell**」と入力します。
1. 検索結果で、 **[Windows PowerShell]** を右クリックするか、そのコンテキスト メニューをアクティブにしてから、 **[管理者として実行]** を選択します。
1. **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-ADUser -Filter * -Properties Department,City | Where {$PSItem.Department -eq ‘IT’ -and $PSItem.City -eq ‘London’} | Select-Object -Property Name,Department,City| Sort Name
   ```

1. **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-ADUser -Filter * -Properties Department,City | Where {$PSItem.Department -eq ‘IT’ -and $PSItem.City -eq ‘London’} | Set-ADUser -Office ‘LON-A/1000’
   ```

1. **[管理者: Windows PowerShell]** ウィンドウで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-ADUser -Filter * -Properties Department,City,Office | Where {$PSItem.Department -eq ‘IT’ -and $PSItem.City -eq ‘London’} | Select-Object -Property Name,Department,City,Office | Sort Name
   ```

### <a name="task-2-produce-an-html-report-listing-the-active-directory-users-in-the-it-department"></a>タスク 2: IT 部門内の Active Directory ユーザーを一覧表示する HTML レポートを生成する

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   help ConvertTo-Html –ShowWindow
   ```

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-ADUser -Filter * -Properties Department,City,Office | 
   Where {$PSItem.Department -eq 'IT' -and $PSItem.City -eq 'London'} | 
   Sort Name | 
   Select-Object -Property Name,Department,City,Office |
   ConvertTo-Html –Property Name,Department,City -PreContent Users | 
   Out-File E:\UserReport.html
   ```

1. HTML ファイルを確認するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Invoke-Expression E:\UserReport.html
   ```

1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-ADUser -Filter * -Properties Department,City,Office | 
   Where {$PSItem.Department -eq 'IT' -and $PSItem.City -eq 'London'} | 
   Sort Name | 
   Select-Object -Property Name,Department,City,Office |
   Export-Clixml E:\UserReport.xml
   ```

1. Internet Explorer のアドレス バーに、「**E:\\UserReport.xml**」と入力してから、Enter キーを押します。
1. コンソールで、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   Get-ADUser -Filter * -Properties Department,City,Office | 
   Where {$PSItem.Department -eq 'IT' -and $PSItem.City -eq 'London'} | 
   Sort Name | 
   Select-Object -Property Name,Department,City,Office |
   Export-Csv E:\UserReport.csv
   ```

1. エクスプローラーで、**E\\:** を参照し、**UserReport.csv** を右クリックするか、そのコンテキスト メニューをアクティブにして、 **[プログラムから開く]** を選択してから、 **[メモ帳]** を選びます。
1. エクスプローラーで、**E: \\** を参照し、 **[UserReport.csv]** を選択してから、Enter キーを押します。

### <a name="exercise-4-results"></a>演習 4 の結果

この演習を完了すると、Active Directory ユーザー オブジェクトを別のデータ形式に変換したことになります。
