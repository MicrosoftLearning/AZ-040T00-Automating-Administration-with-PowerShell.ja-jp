---
lab:
  title: 'ラボ: PowerShell での変数、配列、ハッシュ テーブルの使用'
  type: Answer Key
  module: 'Module 6: Working with variables, arrays, and hash tables'
---

# ラボの回答キー: PowerShell で変数、配列、ハッシュ テーブルを使用する

## 演習 1:変数型の使用

### タスク 1: 文字列変数を使用する

1. **LON-CL1** で、**[スタート]** を選択して、「**powersh**」と入力します。
1. 結果リストで、**[Windows PowerShell]** を右クリックするか、そのコンテキスト メニューをアクティブにしてから、**[管理者として実行]** を選択します。
1. `$logPath` 変数を設定するには、Windows PowerShell プロンプトで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $logPath = "C:\Logs\"
   ```

1. `$logPath` の変数型を表示するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $logPath.GetType()
   ```

1. `$logPath` 変数のプロパティとメソッドを確認するには、次のコマンドを入力し、Enter キーを押します。

    ```powershell
    $logPath | Get-Member
    ```

1. `$logFile` の変数を設定するには、次のコマンドを入力し、Enter キーを押します。

    ```powershell
    $logFile = "log.txt"
    ```

1. `$logPath` 変数に `$logFile` の変数を設定するには、次のコマンドを入力し、Enter キーを押します。

    ```powershell
    $logPath += $logFile
    ```

1. `$logPath` 変数の内容を確認するには、次のコマンドを入力し、Enter キーを押します。

    ```powershell
    $logPath
    ```

1. **C:** を `$logPath` 値の **D:** に置き換えるには、次のコマンドを入力し、Enter キーを押します。

    ```powershell
    $logPath.Replace("C:","D:")
    ```

1. **C:** を `$logPath` の **D:** に置き換えるには、次のコマンドを入力し、Enter キーを押します。

    ```powershell
    $logPath = $logPath.Replace("C:","D:")
    ```

1. `$logPath` 変数の内容を確認するには、次のコマンドを入力し、Enter キーを押します。

     ```powershell
     $logPath
     ```

1. 次のタスクのために、Windows PowerShell プロンプトを開いたままにします。

### タスク 2: DateTime 変数を使用する

1. `$today` 変数を今日の日付に等しい値に設定するには、Windows PowerShell プロンプトで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $today = Get-Date
   ```

1. `$today` 変数の変数型を確認するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $today.GetType()
   ```

1. `$today` 変数のプロパティとメソッドを確認するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $today | Get-Member
   ```

1. 日付に基づいてログ ファイル名を設定するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $logFile = [string]$today.Year + "-" + $today.Month + "-" + $today.Day + "-" + $today.Hour + "-" + $today.Minute + ".txt"
   ```

1. 今日の **30** 日前の日付を計算するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $cutOffDate = $today.AddDays(-30)
   ```

1. 過去 30 日間にサインインしたユーザーを確認するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-ADUser -Properties LastLogonDate -Filter {LastLogonDate -gt $cutOffDate}
   ```

1. 次の演習のために、Windows PowerShell プロンプトを開いたままにします。

## 演習 2:配列の使用

### タスク 1: 配列を使用してユーザーの部署を更新する

1. **Marketing** 部門のすべてのユーザーについてクエリを実行するには、Windows PowerShell プロンプトで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $mktgUsers = Get-ADUser -Filter {Department -eq "Marketing"} -Properties Department
   ```

1. `$mktgUsers` 変数に含まれるユーザーの数を特定するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $mktgUsers.count
   ```

1. `$mktgUsers` の最初のユーザーを確認するには、次のコマンドを入力してから、Enter キーを押します。

   ```powershell
   $mktgUsers[0]
   ```

1. 部署を **Business Development** に変更するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $mktgUsers | Set-ADUser -Department "Business Development"
   ```

1. `$mktgUsers` 変数内のユーザーの **Name** と **Department** を確認し、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $mktgUsers | Format-Table Name,Department
   ```

1. 出力を確認し、`$mktgUsers` 変数の Department の値が変更されていないことを確認します。

1. **Marketing** 部門のすべてのユーザーについてクエリを実行するには、Windows PowerShell プロンプトで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-ADUser -Filter {Department -eq "Marketing"}
   ```

1. **Business Development** 部門のすべてのユーザーについてクエリを実行するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-ADUser -Filter {Department -eq "Business Development"}
   ```

1. 次のタスクのために、Windows PowerShell プロンプトを開いたままにします。

### タスク 2: 配列リストを使用する

1. コンピューター名の配列リストを作成するには、Windows PowerShell プロンプトで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   [System.Collections.ArrayList]$computers="LON-SRV1","LON-SRV2","LON-DC1"
   ```

1. `$computers` 配列リストが固定サイズでないことを確認するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $computers.IsFixedSize
   ```

1. `$computers` 配列リストにコンピューター名を追加するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $computers.Add("LON-DC2")
   ```

1. `$computers` 配列リストからコンピューター名を削除するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $computers.Remove("LON-SRV2")
   ```

1. `$computers` 配列リストの項目を確認するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $computers
   ```

1. 次の演習のために、Windows PowerShell プロンプトを開いたままにします。

## 演習 3:ハッシュ テーブルを使用する

### タスク 1: ハッシュ テーブルを使用する

1. 名前とメール アドレスを含むハッシュ テーブルを作成するには、Windows PowerShell プロンプトで次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $mailList=@{"Frank"="Frank@fabriakm.com";"Libby"="LHayward@contso.com";"Matej"="MSTaojanov@tailspintoys.com"}
   ```

1. `$mailList` ハッシュ テーブルの内容を確認するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $mailList
   ```

1. **Libby** のメール アドレスを確認するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $mailList.Libby
   ```

1. **Libby** のメール アドレスを更新するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $mailList.Libby="Libby.Hayward@contoso.com"
   ```

1. ハッシュ テーブルに新しい名前とメールアドレスを追加するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $mailList.Add("Stela","Stela.Sahiti")
   ```

1. ハッシュ テーブルから **Frank** を削除するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $mailList.Remove("Frank")
   ```

1. `$mailList` ハッシュ テーブルの内容を確認するには、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $mailList
   ```

1. Windows PowerShell プロンプトを閉じます。
