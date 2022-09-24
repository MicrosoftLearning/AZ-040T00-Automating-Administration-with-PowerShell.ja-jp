---
lab:
  title: 'ラボ B:PowerShell パイプラインの使用'
  module: 'Module 3: Working with the Windows PowerShell pipeline'
---

# <a name="lab-using-powershell-pipeline"></a>ラボ: PowerShell パイプラインの使用

## <a name="scenario"></a>シナリオ

Adatum Corporation の管理タスクの 1 つは、高度な PowerShell スクリプトを構成することです。 オブジェクトの並べ替え、フィルター処理、列挙、および変換を行うことによって、PowerShell パイプラインの操作の基礎を理解する必要があります。

## <a name="objectives"></a>目標

このラボを完了すると、次のことができるようになります。

- 基本および高度な構文を使用してパイプライン オブジェクトを列挙する。
- オブジェクトを別の形式に変換する。

## <a name="estimated-time-60-minutes"></a>予測される所要時間:60 分

## <a name="lab-setup"></a>ラボのセットアップ

仮想マシン: **AZ-040T00A-LON-DC1**、**AZ-040T00A-LON-SVR1**、および **AZ-040T00A-LON-CL1**

ユーザー名: **Adatum\\Administrator**

パスワード: **Pa55w.rd**

## <a name="lab-startup"></a>ラボのスタートアップ

1. **[LON-DC1]** を選択します。
1. 次の資格情報を使用してサインインします。
   - ユーザー名: **Administrator**
   - パスワード: **Pa55w.rd**
   - ドメイン: **Adatum**

1. **LON-CL1** と **LON-SVR1** についても同じ手順を繰り返します。

## <a name="exercise-1-enumerating-objects"></a>演習 1:オブジェクトの列挙

### <a name="exercise-scenario-1"></a>演習のシナリオ 1

この演習では、パイプライン内の複数のオブジェクトを操作するコマンドを作成します。 一部のタスクでは、リストを使用する必要があります。 その他のタスクでは、リストを使用する必要はありません。 各タスクに最適なアプローチを決定します。

この演習の主なタスクは次のとおりです。

1. コンピューターのドライブ **E** にあるファイルのリストを表示する。
1. リストを使用して 100 の乱数を生成する。
1. Windows Management Instrumentation (WMI) オブジェクトのメソッドを実行する。

### <a name="task-1-display-a-list-of-files-on-drive-e-of-your-computer"></a>タスク 1: コンピューターのドライブ E にファイルのリストを表示する

1. **LON-CL1** で、管理資格情報を使用して Windows PowerShell を起動します。
1. ドライブ **E** のすべての項目のディレクトリ一覧を表示します。一覧にはサブフォルダーを含めます。
1. ディレクトリ名を表示せずに、ドライブ **E** のすべてのファイルのリストを表示します。

### <a name="task-2-use-enumeration-to-produce-100-random-numbers"></a>タスク 2: リストを使用して 100 の乱数を生成する

1. **random** などのキーワードを使用して、乱数を生成するコマンドを検索します。
1. コマンドのヘルプを確認します。
1. **1..100** を実行して、100 の数値オブジェクトをパイプラインに配置します。
1. コマンドを再実行します。 数値オブジェクトごとに、シードとして数値オブジェクトを使用する乱数を生成します。

### <a name="task-3-run-a-method-of-a-windows-management-instrumentation-wmi-object"></a>タスク 3: Windows Management Instrumentation (WMI) オブジェクトのメソッドを実行する

1. **Windows PowerShell コンソール**以外のすべてのアプリケーションを閉じます。
1. コマンド **Get-WmiObject -Class Win32_OperatingSystem -EnableAllPrivileges** を実行します。
1. 前のコマンドで生成されたオブジェクトのメンバーを表示します。
1. メンバーのリストで、コンピューターを再起動するメソッドを検索します。
1. コマンドを再実行し、リストを使用して、コンピューターを再起動するメソッドを実行します。

   > **注:** このコマンドを実行すると、実行しているコンピューターが再起動します。

### <a name="exercise-1-results"></a>演習 1 の結果

この演習を完了すると、パイプライン内の複数のオブジェクトを操作するコマンドを作成したことになります。

## <a name="exercise-2-converting-objects"></a>演習 2:オブジェクトの変換

### <a name="exercise-scenario-2"></a>演習のシナリオ 2

この演習では、Active Directory ユーザーに対してクエリを実行し、それらに関する情報を変更するコマンドを作成します。 また、ユーザー データをさまざまなファイル形式で保存するコマンドも作成します。 次に、ファイルを確認して、最も役に立つと思われるデータ形式を判断します。

この演習の主なタスクは次のとおりです。

1. Active Directory のユーザー情報を更新する。
1. IT 部門内の Active Directory ユーザーを一覧表示するファイルを生成する。

### <a name="task-1-update-active-directory-user-information"></a>タスク 1: Active Directory のユーザー情報を更新する

1. パスワード **Pa55w.rd** を使い、**Adatum\\Administrator** として **LON-CL1** にサインインします。
1. **Windows PowerShell** を管理者として開始します。
1. ロンドンにある IT 部署のすべてのユーザーの名前、部署、および市区町村を名前ごとにアルファベット順で表示します。
1. すべてのユーザーの **Office** の場所を **LON-A/1000** に設定します。
1. 各ユーザーのオフィスの割り当てを含めて、ユーザーのリストを再度表示します。

### <a name="task-2-generate-files-listing-the-active-directory-users-in-the-it-department"></a>タスク 2:IT 部門内の Active Directory ユーザーを一覧表示するファイルを生成する

1. **ConvertTo-Html** のヘルプを確認します。
1. 同じリストを再度表示し、そのリストを HTML ページに変換します。 HTML データを **E:\\UserReport.html** に格納します。 ユーザーのリストの前に **Users** という単語が表示されるようにします。
1. Internet Explorer を使用して **UserReport.html** を確認します。
1. 同じリストを再度表示し、XML に変換します。
1. Internet Explorer を使用して **UserReport.xml** を確認します。
1. コンマ区切り値 (CSV) ファイル内のすべての Active Directory ユーザーのすべてのプロパティのリストを表示します。
1. メモ帳で CSV ファイルを開きます。
1. Microsoft Excel で CSV ファイルを開きます。

### <a name="exercise-2-results"></a>演習 2 の結果

この演習を完了すると、Active Directory ユーザーに対してクエリを実行し、そのユーザーに関する情報を変更し、さらに Active Directory ユーザー オブジェクトをさまざまなデータ形式に変換しています。
