---
lab:
  title: 'ラボ: PowerShell パイプラインの使用'
  type: Answer Key
  module: 'Module 3: Working with the Windows PowerShell pipeline'
---

# <a name="lab-answer-key-using-powershell-pipeline"></a>ラボの回答キー:PowerShell パイプラインの使用

## <a name="exercise-1-enumerating-objects"></a>演習 1:オブジェクトの列挙

### <a name="task-1-display-a-list-of-files-on-drive-e-of-your-computer"></a>タスク 1: コンピューターのドライブ E にあるファイルのリストを表示する

1. **LON-CL1** で、 **[スタート]** を選択して、「**powersh**」と入力します。
1. 検索結果で、 **[Windows PowerShell]** を右クリックするか、そのコンテキスト メニューをアクティブにしてから、 **[管理者として実行]** を選択します。
1. **[管理者:Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-ChildItem -Path E: -Recurse
   ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-ChildItem -Path E: -Recurse | Get-Member 
   ```

   > **注:** **TypeName: System.IO.DirectoryInfo** の下にあるリストの **GetFiles** メソッドに注目してください。

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-ChildItem -Path E: -Recurse | ForEach GetFiles
   ```

### <a name="task-2-use-enumeration-to-produce-100-random-numbers"></a>タスク 2: 列挙を使用して 100 の乱数を生成する

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   help *random* 
   ```

   > **注:** **Get-Random** コマンドに注目してください。

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   help Get-Random –ShowWindow 
   ```

   > **注:** *-SetSeed* パラメーターに注目してください。

1. **[Get-Random ヘルプ]** ウィンドウを閉じます。  
1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   1..100 
   ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   1..100 | 
   ForEach { Get-Random –SetSeed $PSItem }
   ```

### <a name="task-3-run-a-method-of-a-windows-management-instrumentation-wmi-object"></a>タスク 3: Windows Management Instrumentation (WMI) オブジェクトのメソッドを実行する

1. **Windows PowerShell** コンソール以外のすべてのアプリケーションを閉じます。
1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-WmiObject –Class Win32_OperatingSystem -EnableAllPrivileges
   ```

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-WmiObject –Class Win32_OperatingSystem -EnableAllPrivileges | 
   Get-Member
   ```

   > **注:** **Reboot** メソッドに注目してください。

   > **注:** 次のコマンドでは、実行しているコンピューターを再起動します。

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-WmiObject –Class Win32_OperatingSystem -EnableAllPrivileges | 
   ForEach Reboot
   ```

### <a name="exercise-1-results"></a>演習 1 の結果

この演習を完了すると、パイプライン内の複数のオブジェクトを操作するコマンドを作成したことになります。

## <a name="exercise-2-converting-objects"></a>演習 2:オブジェクトの変換

### <a name="task-1-update-active-directory-user-information"></a>タスク 1: Active Directory のユーザー情報を更新する

> **注:** このラボでは、通常、長いコマンドがいくつかの行に表示されます。 これは、コマンドの途中で意図しない改行が行われないようにするのに役立ちます。 しかし、これらのコマンドを入力するときは、1 行として入力する必要があります。 その行は、画面上で複数の行に折り返される場合がありますが、コマンドは引き続き機能します。 コマンド全体を入力した後にのみ、Enter キーを押します。

1. パスワード **Pa55w.rd** を使用して、**Adatum\\Administrator** として **LON-CL1** にサインインします。
1. **LON-CL1** で、**[スタート]** を選択して、「**powersh**」と入力します。
1. 検索結果で、 **[Windows PowerShell]** を右クリックするか、そのコンテキスト メニューをアクティブにしてから、 **[管理者として実行]** を選択します。
1. **[管理者:Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-ADUser -Filter * -Properties Department,City | Where {$PSItem.Department -eq ‘IT’ -and $PSItem.City -eq ‘London’} | Select-Object -Property Name,Department,City| Sort Name
   ```

1. **[管理者:Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-ADUser -Filter * -Properties Department,City | Where {$PSItem.Department -eq ‘IT’ -and $PSItem.City -eq ‘London’} | Set-ADUser -Office ‘LON-A/1000’
   ```

1. **[管理者:Windows PowerShell]** ウィンドウで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-ADUser -Filter * -Properties Department,City,Office | Where {$PSItem.Department -eq ‘IT’ -and $PSItem.City -eq ‘London’} | Select-Object -Property Name,Department,City,Office | Sort Name
   ```

### <a name="task-2-generate-files-listing-the-active-directory-users-in-the-it-department"></a>タスク 2:IT 部門内の Active Directory ユーザーを一覧表示するファイルを生成する

1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   help ConvertTo-Html –ShowWindow
   ```

1. **[ConvertTo-Html ヘルプ]** ウィンドウを閉じます。  
1. コンソールで、次のコマンドを入力して Enter キーを押します。

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

1. コンソールで次のコマンドを入力し、Enter キーを押します (これにより、Web ブラウザー ウィンドウが自動的に開きます)。

   ```powershell
   Get-ADUser -Filter * -Properties Department,City,Office | 
   Where {$PSItem.Department -eq 'IT' -and $PSItem.City -eq 'London'} | 
   Sort Name | 
   Select-Object -Property Name,Department,City,Office |
   Export-Clixml E:\UserReport.xml
   ```

1. Web ブラウザー ページに表示されたレポートを確認し、Web ブラウザー ウィンドウを閉じます。 
1. コンソールで、次のコマンドを入力して Enter キーを押します。

   ```powershell
   Get-ADUser -Filter * -Properties Department,City,Office | 
   Where {$PSItem.Department -eq 'IT' -and $PSItem.City -eq 'London'} | 
   Sort Name | 
   Select-Object -Property Name,Department,City,Office |
   Export-Csv E:\UserReport.csv
   ```

1. エクスプローラーを開き、エクスプローラー ウィンドウで、**E:\\** に移動し、**UserReport.csv** を右クリックするか、そのコンテキスト メニューをアクティブにして、 **[プログラムから開く]** を選択してから、 **[メモ帳]** を選びます。
1. [メモ帳] ウィンドウで、ファイルの内容を確認します。

### <a name="exercise-2-results"></a>演習 2 の結果

この演習を完了すると、Active Directory ユーザーに対してクエリを実行し、そのユーザーに関する情報を変更し、さらに Active Directory ユーザー オブジェクトをさまざまなデータ形式に変換しています。

