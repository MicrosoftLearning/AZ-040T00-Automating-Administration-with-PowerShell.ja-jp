---
lab:
  title: 'ラボ: PowerShell での変数、配列、ハッシュ テーブルの使用'
  module: 'Module 6: Working with variables, arrays, and hash tables'
---

# ラボ: PowerShell での変数、配列、ハッシュ テーブルの使用

## シナリオ

組織内のサーバー管理を自動化するスクリプトを作成する準備をしています。 開始する前に、変数、配列、およびハッシュ テーブルを使用した操作を練習することもできます。

## 目標

このラボを完了すると、次のことができるようになります。

- 変数の型を操作する。
- 配列を使用する。
- ハッシュ テーブルを使用する。

## 予想所要時間: 45 分

## ラボのセットアップ

仮想マシン: **AZ-040T00A-LON-DC1**、**AZ-040T00A-LON-SVR1**、および **AZ-040T00A-LON-CL1**

ユーザー名: **Adatum\\Administrator**

パスワード: **Pa55w.rd**

このラボでは、提供されている仮想マシン環境を使用します。 ラボを開始する前に、次の手順を行ってください。

1. **LON-DC1** を開き、パスワード **Pa55w.rd** を使って**Adatum\\Administrator** としてサインインします。
1. **LON-SVR1** と **LON-CL1** に対して手順 1 を繰り返します。

## 演習 1:変数型の使用

### 演習のシナリオ 1

まず、さまざまな型の変数を使用した操作を練習します。

この演習の主なタスクは次のとおりです。

1. 文字列変数を使用する。
1. DateTime 変数を使用する。

### タスク 1: 文字列変数を使用する

1. **LON-CL1** で、Windows PowerShell プロンプトを開きます。
1. **C:\logs** を含む変数 `$logPath` を作成します。\.
1. `$logPath` では、変数の型と使用可能なプロパティおよびメソッドを識別します。
1. **log.txt** を含む変数 `$logFile` を作成します。
1. `$logPath` を更新して、`$logFile` の内容を含めます。
1. **C** ドライブではなく **D** ドライブを使用するように、`$logPath` に格納されているパスを更新します。
1. 次のタスクのために、Windows PowerShell プロンプトを開いたままにします。

### タスク 2: DateTime 変数を使用する

1. Windows PowerShell プロンプトで、今日の日付を含む変数 `$today` を作成します。
1. 変数 `$today` では、変数の型と使用可能なプロパティおよびメソッドを識別します。
1. `$today` のプロパティを使用して **Year-Month-Day-Hour-Minute.txt** 形式で文字列を作成し、値を `$logFile` に格納します。
1. 今日から **30** 日前の日付を含む変数 `$cutOffDay` を作成します。
1. **Get-ADUser** を使用して、`$cutOffDay` 以降にサインインしたユーザー アカウントをクエリします。 **LastLogonDate** プロパティを使用してフィルター処理します。
1. 次の演習のために、Windows PowerShell プロンプトを開いたままにします。

## 演習 2:配列の使用

### 演習のシナリオ 2

さまざまな型の変数を使用する練習ができたので、次は配列を操作します。

この演習の主なタスクは次のとおりです。

1. 配列を使用してユーザーの部署を更新する。
1. 配列リストを使用する。

### タスク 1: 配列を使用してユーザーの部署を更新する

1. **Marketing** 部門のすべての Active Directory Domain Services (AD DS) ユーザーに対してクエリを実行し、`$mktgUsers` という名前の変数に配置します。 **Department** プロパティを結果に含めます。
1. マーケティング部門のユーザー数を特定するために `$mktgUsers` を使用します。
1. `$mktgUsers` で最初のユーザーを表示し、**Department** プロパティが一覧に表示されていることを確認します。
1. パイプを使用して `$mktgUsers` のユーザーを **Set-ADUser** に渡し、部署を **Business Development** に更新します。
1. `$mktgUsers` の **Department** 属性を確認して、更新されたかどうかを確認します。
1. **Marketing** 部門のすべての AD DS ユーザーをクエリして、存在しないことを確認します。
1. **Business Development** 部門のすべての AD DS ユーザーにクエリを実行して、マーケティング部門の前の数と一致していることを確認します。
1. 次のタスクのために、Windows PowerShell プロンプトを開いたままにします。

### タスク 2: 配列リストを使用する

1. **LON-SRV1**、**LON-SRV2**、および **LON-DC1** の値を指定して、`$computers` という名前の配列リストを作成します。
1. `$computers` のサイズが固定されていないことを確認します。
1. **LON-DC2** を `$computers` に追加します。
1. `$computers` から **LON-SRV2** を削除します。
1. `$computers` の内容を表示します。
1. 次の演習のために、Windows PowerShell プロンプトを開いたままにします。

## 演習 3:ハッシュ テーブルを使用する

### 演習のシナリオ 3

変数と配列を使用した後は、ハッシュ テーブルを使用した操作を練習します。 ハッシュ テーブルを使用した操作方法は、配列や配列リストとは異なります。

この演習の主なタスクは次のとおりです。

- ハッシュ テーブルを使用する。

### タスク 1: ハッシュ テーブルを使用する

1. 次のユーザーと電子メール アドレスを使用して、`$mailList` という名前のハッシュ テーブルを作成します。

   - 電子メール アドレス **Frank@fabrikam.com** を持つユーザー **Frank**
   - 電子メール アドレス **LHayward@contoso.com** を持つユーザー **Libby**
   - 電子メール アドレス **MStojanov@tailspintoys.com** を持つユーザー **Matej**

1. `$mailList` の内容を表示します。
1. **Libby** の電子メール アドレスを表示します。
1. **Libby** の電子メール アドレスを **Libby.Hayward@contoso.com** に更新します。
1. **Stela** の新しい電子メール アドレス **Stela.Sahiti@treyresearch.net** を追加します。
1. `$mailList` の項目の数を確認します。
1. `$mailList` から **Frank** を削除します。
1. **Frank** が削除されていることを確認します。
1. Windows PowerShell プロンプトを閉じます。
