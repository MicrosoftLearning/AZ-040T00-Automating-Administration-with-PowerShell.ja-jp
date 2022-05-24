---
lab:
  title: 'ラボ: WMI と CIM を使って情報についてのクエリを実行する'
  type: Answer Key
  module: 'Module 5: Querying management information by using CIM and WMI'
ms.openlocfilehash: e0531bc1486e9cd6cf05607934da80c3b9ce9338
ms.sourcegitcommit: dca48f28a6753becf280d7814397cb9a415fa951
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/19/2022
ms.locfileid: "145180218"
---
# <a name="lab-querying-information-by-using-wmi-and-cim"></a>ラボ: WMI と CIM を使った情報のクエリ

## <a name="exercise-1-querying-information-by-using-wmi"></a>演習 1: WMI を使った情報のクエリ

### <a name="task-1-query-ip-addresses"></a>タスク 1: IP アドレスのクエリを実行する

1. **LON-CL1** で、タスク バーの **[何でも質問してください]** ボックスに「**PowerShell**」と入力します。 返されたリストで、 **[Windows PowerShell]** を右クリックするか、そのコンテキスト メニューをアクティブにしてから、 **[管理者として実行]** を選択します。

1. コンピューターで使用されている IP アドレスを一覧表示するリポジトリ クラスを検索するには、**Windows PowerShell** コンソールで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-WmiObject -Namespace root\cimv2 -List | Where Name -like '*configuration*' | Sort Name
   ```

   `Win32_NetworkAdapterConfiguration` クラスに注目してください。

1. 静的 IP アドレスを示すクラスのすべてのインスタンスを取得するには、**Windows PowerShell** コンソールで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-WmiObject -Class Win32_NetworkAdapterConfiguration | Where DHCPEnabled -eq $False | Select IPAddress
   ```

   最初のコマンドを実行し、パイプを使用してその出力を **Get-Member** に渡し、使用可能なプロパティを確認できます。

### <a name="task-2-query-operating-system-version-information"></a>タスク 2: オペレーティング システムのバージョン情報をクエリする

1. オペレーティング システム情報を一覧表示するリポジトリ クラスを検索するには、**Windows PowerShell** コンソールで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-WmiObject -Namespace root\cimv2 -List | Where Name -like '*operating*' | Sort Name
   ```

   `Win32_OperatingSystem` クラスに注目してください。

1. クラスのプロパティの一覧を表示するには、**Windows PowerShell** コンソールで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-WmiObject -Class Win32_OperatingSystem | Get-Member
   ```

1. **Version**、**ServicePackMajorVersion**、および **BuildNumber** プロパティに注目してください。

1. 指定した情報を表示するには、**Windows PowerShell** コンソールで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-WmiObject -Class Win32_OperatingSystem | Select Version,ServicePackMajorVersion,BuildNumber
   ```

   

### <a name="task-3-query-computer-system-hardware-information"></a>タスク 3: コンピューター システムのハードウェア情報をクエリする

1. コンピューター システム情報を表示するリポジトリ クラスを検索するには、**Windows PowerShell** コンソールで次のコマンドを入力し、Enter キーを押します。

    ```powershell
    Get-WmiObject -Namespace root\cimv2 -List | Where Name -like '*system*' | Sort Name 
    ```

    `Win32_ComputerSystem` クラスに注目してください。

1. インスタンス プロパティと値の一覧を表示するには、**Windows PowerShell** コンソールで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-WmiObject -Class Win32_ComputerSystem | Format-List -Property *
   ```

   `Get-Member` ではプロパティ値は表示されません。`Format-List` で表示できます。

1. 指定した情報を表示するには、**Windows PowerShell** コンソールで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-WmiObject -Class Win32_ComputerSystem | Select Manufacturer,Model,@{n='RAM';e={$PSItem.TotalPhysicalMemory}}
   ```


### <a name="task-4-query-service-information"></a>タスク 4: サービス情報をクエリする

1. サービスについての情報を含むリポジトリ クラスを検索するには、**Windows PowerShell** コンソールで次のコマンドを入力し、Enter キーを押します。
    
    ```powershell
    Get-WmiObject -Namespace root\cimv2 -List | Where Name –like '*service*' | Sort Name
    ```
    
    `Win32_Service` クラスに注目してください。
    
1. インスタンス プロパティと値の一覧を表示するには、**Windows PowerShell** コンソールで次のコマンドを入力し、Enter キーを押します。
   
   ```powershell
   Get-WmiObject -Class Win32_Service | FL *
   ```
   
1. 指定した情報を表示するには、**Windows PowerShell** コンソールで、次のコマンドを入力し、Enter キーを押します。
   
   ```powershell
   Get-WmiObject –Class Win32_Service –Filter "Name LIKE 'S%'" | Select Name,State,StartName
   ```
   
1. 次の演習のために、**Windows PowerShell** コンソールを開いたままにします。

# <a name="exercise-2-querying-information-by-using-cim"></a>演習 2: CIM を使った情報のクエリ

### <a name="task-1-query-user-accounts"></a>タスク 1: ユーザー アカウントをクエリする

1. ユーザー アカウントを一覧表示するリポジトリ クラスを検索するには、**Windows PowerShell** コンソールで次のコマンドを入力し、Enter キーを押します。
   
   ```powershell
   Get-CimClass -ClassName *user*
   ```
   
   `Win32_UserAccount` クラスに注目してください。
   
1. クラス プロパティの一覧を表示するには、**Windows PowerShell** コンソールで次のコマンドを入力し、Enter キーを押します。
   
   ```powershell
   Get-CimInstance -Class Win32_UserAccount | Get-Member
   ```
   
1. 指定した情報を表示するには、**Windows PowerShell** コンソールで、次のコマンドを入力し、Enter キーを押します。
   
   ```powershell
   Get-CimInstance -Class Win32_UserAccount | Format-Table -Property Caption,Domain,SID,FullName,Name
   ```
   
   返されたすべてのドメインおよびローカル アカウントの一覧に注目してください。

### <a name="task-2-query-bios-information"></a>タスク 2: BIOS 情報をクエリする

1. BIOS 情報を含むリポジトリ クラスを検索するには、**Windows PowerShell** コンソールで次のコマンドを入力し、Enter キーを押します。

    ```powershell
    Get-CimClass -ClassName *bios*
    ```

    `Win32_BIOS` クラスに注目してください。

1. 指定した情報を表示するには、**Windows PowerShell** コンソールで、次のコマンドを入力し、Enter キーを押します。

    ```powershell
    Get-CimInstance -Class Win32_BIOS
    ```

    

### <a name="task-3-query-network-adapter-configuration-information"></a>タスク 3: ネットワーク アダプターの構成情報をクエリする

1. すべてのローカル `Win32_NetworkAdapterConfiguration` インスタンスの一覧を表示するには、**Windows PowerShell** コンソールで次のコマンドを入力し、Enter キーを押します。
   
   ```powershell
   Get-CimInstance -Classname Win32_NetworkAdapterConfiguration
   ```
   
1. **LON-DC1** 上にあるすべての `Win32_NetworkAdapterConfiguration` インスタンスの一覧を表示するには、**Windows PowerShell** コンソールで次のコマンドを入力し、Enter キーを押します。
   
   ```powershell
   Get-CimInstance -Classname Win32_NetworkAdapterConfiguration -ComputerName LON-DC1
   ```

### <a name="task-4-query-user-group-information"></a>タスク 4: ユーザー グループ情報をクエリする

1. ユーザー グループを一覧表示するリポジトリ クラスを検索するには、**Windows PowerShell** コンソールで次のコマンドを入力し、Enter キーを押します。

    ```powershell
    Get-CimClass -ClassName *group*
    ```

    `Win32_Group` クラスに注目してください。

1. 指定した情報を表示するには、**Windows PowerShell** コンソールで、次のコマンドを入力し、Enter キーを押します。

    ```powershell
    Get-CimInstance -ClassName Win32_Group -ComputerName LON-DC1
    ```

1. 次の演習のために、**Windows PowerShell** コンソールを開いたままにします。

## <a name="exercise-3-invoking-methods"></a>演習 3: メソッドの呼び出し

### <a name="task-1-invoke-a-cim-method"></a>タスク 1: CIM メソッドを呼び出す

1. **LON-DC1** を再起動するには、**Windows PowerShell** コンソールで次のコマンドを入力し、Enter キーを押します。
   
    ```powershell
    Invoke-CimMethod -ClassName Win32_OperatingSystem -ComputerName LON-DC1 -MethodName Reboot
    ```
   **ReturnValue=0** と **PSComputerName=LON-DC1** を含む応答に注目してください。
1. **LON-DC1** 仮想マシンに切り替えて、再起動を確認します。
1. 再起動が完了したら、**LON-CL1** 仮想マシンに戻ります。

### <a name="task-2-invoke-a-wmi-method"></a>タスク 2: WMI メソッドを呼び出す

1. WinRM サービスのプロパティを確認するには、**Windows PowerShell** コンソールで次のコマンドを入力し、Enter キーを押します。
    
    ```powershell
    Get-Service WinRM | FL *
    ```
    **StartType** が **Manual** であることを確認してください。
1. 指定したサービスの起動モードを変更するには、**Windows PowerShell** コンソールで、次のコマンドを入力し、Enter キーを押します。
    
    ```powershell
    Get-WmiObject -Class Win32_Service -Filter "Name='WinRM'" | Invoke-WmiMethod -Name ChangeStartMode -Argument 'Automatic'
    ```
1. WinRM サービスの StartType が変更されたことを確認するには、**Windows PowerShell** コンソールで、次のコマンドを入力し、Enter キーを押します。
   
    ```powershell
    Get-Service WinRM | FL *
    ```

   **StartType** が **Automatic** であることを確認してください。
