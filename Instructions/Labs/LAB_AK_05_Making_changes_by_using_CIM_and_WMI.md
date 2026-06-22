---
lab:
  title: 'ラボ: WMI と CIM を使った情報のクエリ'
  type: Answer Key
  module: 'Module 5: Querying management information by using CIM and WMI'
  description: WMI と CIM 両方のコマンドレットを使って、システム情報のクエリを実行します。 関連するクラスを検出し、結果をフィルター処理して、管理タスクのリポジトリ メソッドを呼び出します。
  duration: 45 minutes
  level: 200
  islab: true
  primarytopics:
    - WMI
    - CIM
    - PowerShell
---

# WMI と CIM を使った情報のクエリ

このラボは完了するまで、約 **45** 分かかります。

## シナリオ

複数のコンピューターに管理情報のクエリを実行する必要があります。 まず、ローカル コンピューターと環境内の 1 つのテスト コンピューターからクエリを実行します。

## 目標

このラボを完了すると、次のことができるようになります。

- Windows Management Instrumentation (WMI) コマンドを使用して情報についてのクエリを実行する。
- Common Information Model (CIM) コマンドを使用して情報についてのクエリを実行する。
- WMI および CIM コマンドを使用してメソッドを呼び出します。

## ラボのセットアップ

仮想マシン: **AZ-040T00A-LON-DC1** と** AZ-040T00A-LON-CL1**

ユーザー名: **Adatum\\Administrator**

パスワード: **Pa55w.rd**

このラボでは、提供されている仮想マシン環境を使用します。 ラボを開始する前に、次の手順を行ってください。

1. **LON-DC1** を開き、パスワード **Pa55w.rd** を使って**Adatum\\Administrator** としてサインインします。
1. **LON-CL1** について手順 1 を繰り返します。

## 演習 1:WMI を使用した情報の照会

> **注:** `Get-WmiObject` コマンドレットは、Windows PowerShell 5.1 でのみ使用できます。 PowerShell 7 では代わりに `Get-CimInstance` を使用してください (演習 2 を参照)。 この演習は必ず **Windows PowerShell** コンソール上で実行してください。

### シナリオ 1

この演習では、リポジトリ クラスを検出し、WMI コマンドを使用してこれに対するクエリを実行します。

この演習の主なタスクは次のとおりです。

1. IP アドレスのクエリを実行する。
1. オペレーティング システムのバージョン情報のクエリを実行する。
1. コンピューター システムのハードウェア情報のクエリを実行する。
1. サービス情報のクエリを実行する。

### タスク 1: IP アドレスのクエリを実行する

1. **LON-CL1** で、**[スタート]** を選択して、「**powersh**」と入力します。
1. 結果リストで、**[Windows PowerShell]** を右クリックするか、そのコンテキスト メニューをアクティブにしてから、**[管理者として実行]** を選択します。
1. コンピューターで使用されている IP アドレスを一覧表示するリポジトリ クラスを検索するには、**Windows PowerShell** コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-WmiObject -Namespace root\cimv2 -List | Where Name -like '*configuration*' | Sort Name
   ```

   `Win32_NetworkAdapterConfiguration` クラスに注目してください。

1. 静的 IP アドレスを示すクラスのすべてのインスタンスを取得するには、**Windows PowerShell** コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-WmiObject -Class Win32_NetworkAdapterConfiguration | Where DHCPEnabled -eq $False | Select IPAddress
   ```

   最初のコマンドを実行し、パイプを使用してその出力を **Get-Member** に渡し、使用可能なプロパティを確認できます。

### タスク 2: オペレーティング システムのバージョン情報のクエリを実行する

1. オペレーティング システム情報を一覧表示するリポジトリ クラスを検索するには、**Windows PowerShell** コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-WmiObject -Namespace root\cimv2 -List | Where Name -like '*operating*' | Sort Name
   ```

   `Win32_OperatingSystem` クラスに注目してください。

1. クラスのプロパティの一覧を表示するには、**Windows PowerShell** コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-WmiObject -Class Win32_OperatingSystem | Get-Member
   ```

1. **Version**、**ServicePackMajorVersion**、および **BuildNumber** プロパティに注目してください。

1. 指定した情報を表示するには、**Windows PowerShell** コンソールで、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-WmiObject -Class Win32_OperatingSystem | Select Version,ServicePackMajorVersion,BuildNumber
   ```

   

### タスク 3: コンピューター システムのハードウェア情報のクエリを実行する

1. コンピューター システム情報を表示するリポジトリ クラスを検索するには、**Windows PowerShell** コンソールで次のコマンドを入力して、Enter キーを押します。

    ```powershell
    Get-WmiObject -Namespace root\cimv2 -List | Where Name -like '*system*' | Sort Name 
    ```

    `Win32_ComputerSystem` クラスに注目してください。

1. インスタンス プロパティと値の一覧を表示するには、**Windows PowerShell** コンソールで次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-WmiObject -Class Win32_ComputerSystem | Format-List -Property *
   ```

   `Get-Member` ではプロパティ値は表示されません。`Format-List` で表示できます。

1. 指定した情報を表示するには、**Windows PowerShell** コンソールで、次のコマンドを入力して、Enter キーを押します。

   ```powershell
   Get-WmiObject -Class Win32_ComputerSystem | Select Manufacturer,Model,@{n='RAM';e={$PSItem.TotalPhysicalMemory}}
   ```


### タスク 4: サービス情報のクエリを実行する

1. サービスについての情報を含むリポジトリ クラスを検索するには、**Windows PowerShell** コンソールで次のコマンドを入力して、Enter キーを押します。
    
    ```powershell
    Get-WmiObject -Namespace root\cimv2 -List | Where Name –like '*service*' | Sort Name
    ```
    
    `Win32_Service` クラスに注目してください。
    
1. インスタンス プロパティと値の一覧を表示するには、**Windows PowerShell** コンソールで次のコマンドを入力して、Enter キーを押します。
   
   ```powershell
   Get-WmiObject -Class Win32_Service | FL *
   ```
   
1. 指定した情報を表示するには、**Windows PowerShell** コンソールで、次のコマンドを入力し、Enter キーを押します。
   
   ```powershell
   Get-WmiObject –Class Win32_Service –Filter "Name LIKE 'S%'" | Select Name,State,StartName
   ```
   
1. 次の演習のために、**Windows PowerShell** コンソールを開いたままにします。

## 演習 2:CIM を使用した情報の照会

### シナリオ 2

この演習では、新しいリポジトリ クラスを検出し、CIM コマンドを使用してクエリを実行します。

この演習の主なタスクは次のとおりです。

1. ユーザー アカウントのクエリを実行する。
1. BIOS 情報のクエリを実行する。
1. ネットワーク アダプターの構成情報のクエリを実行する。
1. ユーザー グループ情報のクエリを実行する。

### タスク 1: ユーザー アカウントのクエリを実行する

1. ユーザー アカウントを一覧表示するリポジトリ クラスを検索するには、**Windows PowerShell** コンソールで次のコマンドを入力して、Enter キーを押します。
   
   ```powershell
   Get-CimClass -ClassName *user*
   ```
   
   `Win32_UserAccount` クラスに注目してください。
   
1. クラス プロパティの一覧を表示するには、**Windows PowerShell** コンソールで次のコマンドを入力して、Enter キーを押します。
   
   ```powershell
   Get-CimInstance -Class Win32_UserAccount | Get-Member
   ```
   
1. 指定した情報を表示するには、**Windows PowerShell** コンソールで、次のコマンドを入力して、Enter キーを押します。
   
   ```powershell
   Get-CimInstance -Class Win32_UserAccount | Format-Table -Property Caption,Domain,SID,FullName,Name
   ```
   
   返されたすべてのドメインおよびローカル アカウントの一覧に注目してください。

### タスク 2: BIOS 情報をクエリする

1. BIOS 情報を含むリポジトリ クラスを検索するには、**Windows PowerShell** コンソールで次のコマンドを入力して、Enter キーを押します。

    ```powershell
    Get-CimClass -ClassName *bios*
    ```

    `Win32_BIOS` クラスに注目してください。

1. 指定した情報を表示するには、**Windows PowerShell** コンソールで、次のコマンドを入力して、Enter キーを押します。

    ```powershell
    Get-CimInstance -Class Win32_BIOS
    ```    

### タスク 3: ネットワーク アダプターの構成情報をクエリする

1. すべてのローカル `Win32_NetworkAdapterConfiguration` インスタンスの一覧を表示するには、**Windows PowerShell** コンソールで次のコマンドを入力して、Enter キーを押します。
   
   ```powershell
   Get-CimInstance -Classname Win32_NetworkAdapterConfiguration
   ```
   
1. **LON-DC1** 上にあるすべての `Win32_NetworkAdapterConfiguration` インスタンスの一覧を表示するには、**Windows PowerShell** コンソールで次のコマンドを入力して、Enter キーを押します。
   
   ```powershell
   Get-CimInstance -Classname Win32_NetworkAdapterConfiguration -ComputerName LON-DC1
   ```

### タスク 4: ユーザー グループ情報のクエリを実行する

1. ユーザー グループを一覧表示するリポジトリ クラスを検索するには、**Windows PowerShell** コンソールで次のコマンドを入力して、Enter キーを押します。

    ```powershell
    Get-CimClass -ClassName *group*
    ```

    `Win32_Group` クラスに注目してください。

1. 指定した情報を表示するには、**Windows PowerShell** コンソールで、次のコマンドを入力して、Enter キーを押します。

    ```powershell
    Get-CimInstance -ClassName Win32_Group -ComputerName LON-DC1
    ```

1. 次の演習のために、**Windows PowerShell** コンソールを開いたままにします。

## 演習 3:メソッドの呼び出し

### シナリオ 3

この演習では、WMI および CIM コマンドを使用して、リポジトリ オブジェクトのメソッドを呼び出します。

この演習の主なタスクは次のとおりです。

1. CIM メソッドを呼び出す。
1. WMI メソッドを呼び出す。

### タスク 1: CIM メソッドを呼び出す

1. **LON-DC1** を再起動するには、**Windows PowerShell** コンソールで次のコマンドを入力して、Enter キーを押します。
   
    ```powershell
    Invoke-CimMethod -ClassName Win32_OperatingSystem -ComputerName LON-DC1 -MethodName Reboot
    ```
   **ReturnValue=0** と **PSComputerName=LON-DC1** を含む応答に注目してください。
1. **LON-DC1** 仮想マシンに切り替えて、再起動を確認します。
1. 再起動が完了したら、**LON-CL1** 仮想マシンに戻ります。

### タスク 2: WMI メソッドを呼び出す

> **注:** `Get-WmiObject` と `Invoke-WmiMethod` は Windows PowerShell 5.1 でのみ使用できます。 PowerShell 7 では、CIM コマンドレット `Get-CimInstance -ClassName Win32_Service -Filter "Name='WinRM'" | Invoke-CimMethod -MethodName ChangeStartMode -Arguments @{StartMode='Automatic'}` を使用してください。

1. WinRM サービスのプロパティを確認するには、**Windows PowerShell** コンソールで次のコマンドを入力して、Enter キーを押します。
    
    ```powershell
    Get-Service WinRM | FL *
    ```
    **StartType** が **Manual** であることを確認してください。
1. 指定したサービスの起動モードを変更するには、**Windows PowerShell** コンソールで、次のコマンドを入力して、Enter キーを押します。
    
    ```powershell
    Get-WmiObject -Class Win32_Service -Filter "Name='WinRM'" | Invoke-WmiMethod -Name ChangeStartMode -ArgumentList 'Automatic'
    ```
1. WinRM サービスの StartType が変更されたことを確認するには、**Windows PowerShell** コンソールで、次のコマンドを入力して、Enter キーを押します。
   
    ```powershell
    Get-Service WinRM | FL *
    ```

   **StartType** が **Automatic** であることを確認してください。
