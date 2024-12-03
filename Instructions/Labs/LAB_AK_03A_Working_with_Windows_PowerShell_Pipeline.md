---
lab:
  title: 'ラボ A:PowerShell パイプラインの使用'
  type: Answer Key
  module: 'Module 3: Working with the Windows PowerShell pipeline'
---

# ラボ回答キー: PowerShell パイプラインの使用

## 演習 1:データの選択、並べ替え、表示

### タスク 1: 現在の日付を表示する

1. **LON-CL1** で、**[スタート]** を選択して、「**powersh**」と入力します。
1. 検索結果で、**Windows PowerShell** を右クリックするか、そのコンテキスト メニューをアクティブにしてから、**[管理者として実行]** を選択します。
1. 
          **[管理者:Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   help *date* 
   ```

   > **注:** **Get-Date** コマンドに注目してください。

1. コンソールで、次のコマンドを入力して Enter キーを押します。  

   ```powershell
   Get-Date | Get-Member
   ```

   > **注:** **DayOfYear** プロパティに注目してください。

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-Date | Select-Object –Property DayOfYear
   ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-Date | Select-Object -Property DayOfYear | fl
   ```

### タスク 2: インストールされている修正プログラムに関する情報を表示する

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-Command *hotfix* 
   ```

   > **注:** **Get-Hotfix** コマンドに注目してください。

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-Hotfix | Get-Member 
   ```

   > **注:** **Hotfix** オブジェクトのプロパティが表示されます。 必要に応じて、**Get-Hotfix** を実行し、通常はそれらのプロパティに表示される値の一部を確認します。

1. コンソールで、次のコマンドを入力して Enter キーを押します。  

   ```powershell
    Get-Hotfix | Select-Object –Property HotFixID,InstalledOn,InstalledBy
   ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-Hotfix | Select-Object –Property HotFixID,@{n='HotFixAge';e={(New-TimeSpan -Start $PSItem.InstalledOn).Days}},InstalledBy
   ```

### タスク 3: DHCP サーバーから使用可能なスコープのリストを表示する

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   help *scope* 
   ```

   > **注:** **Get-DHCPServerv4Scope** コマンドに注目してください。

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Help Get-DHCPServerv4Scope –ShowWindow 
   ```

   > **注:** 使用可能なパラメーターに注目してください。

1. **[Get-DHCPServerv4Scope]** ウィンドウを閉じます。
1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-DHCPServerv4Scope –ComputerName LON-DC1
   ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-DHCPServerv4Scope –ComputerName LON-DC1 | Select-Object –Property ScopeId,SubnetMask,Name | fl
   ```

### タスク 4: 有効な Windows ファイアウォール規則の並べ替えられたリストを表示する

1. コンソールで、次のコマンドを入力して Enter キーを押します。

      ```powershell
      help *rule* 
      ```

   > **注:** **Get-NetFirewallRule** コマンドに注目してください。

1. コンソールで、次のコマンドを入力して Enter キーを押します。

      ```powershell
      Get-NetFirewallRule 
      ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

      ```powershell
      Help Get-NetFirewallRule –ShowWindow
      ```
  
1. **[Get-NetFirewallRule ヘルプ]** ウィンドウを閉じます。
1. コンソールで、次のコマンドを入力して Enter キーを押します。

      ```powershell
      Get-NetFirewallRule –Enabled True

      ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。
  
      ```powershell
      Get-NetFirewallRule –Enabled True | Format-Table -wrap
      ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

      ```powershell
      Get-NetFirewallRule –Enabled True | Select-Object –Property DisplayName,Profile,Direction,Action | Sort-Object –Property Profile, DisplayName | ft -GroupBy Profile
      ```

### タスク 5: ネットワーク近隣ノードの並べ替えられたリストを表示する

1. コンソールで、次のコマンドを入力して Enter キーを押します。

      ```powershell
      help *neighbor*  
      ```

   > **注:** **Get-NetNeighbor** コマンドに注目してください。

1. コンソールで、次のコマンドを入力して Enter キーを押します。

      ```powershell
      help Get-NetNeighbor –ShowWindow
      ```

1. **[Get-NetNeighbor ヘルプ]** ウィンドウを閉じます。
1. コンソールで、次のコマンドを入力して Enter キーを押します。

      ```powershell
      Get-NetNeighbor
      ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

      ```powershell
      Get-NetNeighbor | Sort-Object –Property State
      ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

      ```powershell
      Get-NetNeighbor | Sort-Object –Property State | Select-Object –Property IPAddress,State | Format-Wide -GroupBy State -AutoSize
      ```

### タスク 6: DNS 名前解決キャッシュからの情報を表示する

1. コンソールで、次のコマンドを入力して Enter キーを押します。

      ```powershell
      Test-NetConnection LON-DC1
      ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

      ```powershell
      help *cache* 
      ```

      > **注:** **Get-DnsClientCache** コマンドに注目してください。

1. コンソールで、次のコマンドを入力して Enter キーを押します。

      ```powershell
      Get-DnsClientCache
      ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

      ```powershell
      Get-DnsClientCache | Select Name,Type,TimeToLive | Sort Name | Format-List
      ```

      > **注:** **型**データからは、期待した内容 (A や CNAME など) が返されないことに注目してください。 代わりに、生の数値データが返されます。 各数値はレコードの型に直接マップされます。マップ (1= A、5 = CNAME など) がわかっている場合は、それらの型をフィルター処理できます。 このモジュールの後半では、さらにフィルターを追加して、数値とそれに対応するレコードの型を決定する方法について学習します。 **状態**データなど、他の返されるデータでも同じような状況になることがわかります。

1. 開いているすべてのウィンドウを閉じます。

### 演習 1 の結果

この演習を完了すると、環境からの管理情報を含むカスタム レポートがいくつか生成されているはずです。

## 演習 2: オブジェクトのフィルター処理

### タスク 1: Active Directory の Users コンテナー内のすべてのユーザーのリストを表示する

1. **LON-CL1** で、**[スタート]** を選択してから、「**PowerShell**」と入力します。
1. 検索結果で、**Windows PowerShell** を右クリックするか、そのコンテキスト メニューをアクティブにしてから、**[管理者として実行]** を選択します。
1. 
          **[管理者:Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   help *user*
   ```

   > **注:** **Get-ADUser** コマンドに注目してください。

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-Help Get-ADUser –ShowWindow
   ```

   > **注:** *-Filter* パラメーターが必須であることに注目してください。 コマンドの例を確認します。

1. **[Get-ADUser ヘルプ]** ウィンドウを閉じます。  
1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-ADUser –Filter * | ft
   ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-ADUser –Filter * -SearchBase "cn=Users,dc=Adatum,dc=com" | ft
   ```

### タスク 2: イベント ID 4624 のセキュリティ イベント ログ エントリを表示するレポートを作成する

1. 
          **[管理者:Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-EventLog -LogName Security | 
   Where EventID -eq 4624 | Measure-Object | fw
   ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-EventLog -LogName Security | 
   Where EventID -eq 4624 | 
   Select TimeWritten,EventID,Message
   ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-EventLog -LogName Security | 
   Where EventID -eq 4624 | 
   Select TimeWritten,EventID,Message -Last 10 | fl
   ```

### タスク 3: コンピューターにインストールされている暗号化証明書のリストを表示する

1. 
          **[管理者:Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-ChildItem -Path CERT: -Recurse
   ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-ChildItem -Path CERT: -Recurse | 
   Get-Member
   ```

1. コンソール ウィンドウで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-ChildItem -Path CERT: -Recurse | 
   Where HasPrivateKey -eq $False | Select-Object -Property FriendlyName,Issuer | fl
   ```

   または:

   ```powershell
   Get-ChildItem -Path CERT: -Recurse | 
   Where { $PSItem.HasPrivateKey -eq $False } | Select-Object -Property FriendlyName,Issuer | fl
   ```

1. コンソール ウィンドウで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-ChildItem -Path CERT: -Recurse | 
   Where { $PSItem.HasPrivateKey -eq $False -and $PSItem.NotAfter -gt (Get-Date) -and $PSItem.NotBefore -lt (Get-Date) } | Select-Object -Property NotBefore,NotAfter, FriendlyName,Issuer | ft -wrap
   ```

### タスク 4: 領域が不足しているディスク ボリュームのレポートを作成する

1. 
          **[管理者:Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-Volume
   ```

   > **注:** コマンド名がわからない場合は、**Help \*ボリューム\*** を実行して検出できます。

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-Volume | Get-Member
   ```

   > **注:** **SizeRemaining** プロパティに注目してください。

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-Volume | Where-Object { $PSItem.SizeRemaining -gt 0 } | fl
   ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-Volume | Where-Object { $PSItem.SizeRemaining -gt 0 -and $PSItem.SizeRemaining / $PSItem.Size -lt .99 }| Select-Object DriveLetter, @{n='Size';e={'{0:N2}' -f ($PSItem.Size/1MB)}}
   ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-Volume | Where-Object { $PSItem.SizeRemaining -gt 0 -and $PSItem.SizeRemaining / $PSItem.Size -lt .1 }
   ```

   > **注:** コンピューターの各ボリュームの空き領域が 10% を超える場合、このコマンドではラボ コンピューターに出力が生成されない可能性があります。

### タスク 5: 指定されたコントロール パネル項目を表示するレポートを作成する

1. 
          **[管理者:Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   help *control* 
   ```

   > **注:** **Get-ControlPanelItem** コマンドに注目してください。

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-ControlPanelItem 
   ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-ControlPanelItem –Category 'System and Security' | Sort Name
   ```

    > **注:** **Where-Object** を使用する必要がないことに注目してください。

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-ControlPanelItem -Category 'System and Security' | Where-Object -FilterScript {-not ($PSItem.Category -notlike '*System and Security*')} | Sort Name
   ```

### 演習 2 の結果

この演習を完了すると、フィルター処理を使用して、指定されたデータと要素のみを含む管理情報のリストを生成したことになります。
