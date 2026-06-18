---
lab:
  title: 'ラボ: PowerShell でのスクリプトの使用'
  type: Answer Key
  module: 'Module 7: Windows PowerShell scripting'
  description: ループ、条件付きロジック、CSV によるアカウント作成を含む PowerShell スクリプトを作成して実行します。 コード署名と実行ポリシーの概念を適用した後、リモート ディスク クエリのための代替資格情報のサポートを追加します。
  duration: 150 minutes
  level: 300
  islab: true
  primarytopics:
    - PowerShell Scripting
    - Code Signing
    - CSV
    - Credential Management
---

# PowerShell でのスクリプトの使用

このラボは完了するまで、約 **150** 分かかります。

## シナリオ

組織での管理を簡略化するために、Windows PowerShell スクリプトの開発を開始しました。 実行するタスクは複数あり、それぞれに Windows PowerShell スクリプトを作成します。

## 目標

このラボを完了すると、次のことができるようになります。

- スクリプトにデジタル署名する。
- ForEach を使用して配列を処理する。
- If ステートメントを使用して項目を処理する。
- CSV ファイルに基づいてユーザー アカウントを作成する。
- リモート コンピューターからのディスク情報のクエリを実行する。
- 代替の資格情報を使用するようにスクリプトを更新する。

## ラボのセットアップ

仮想マシン: **LON-DC1**、**LON-SVR1**、および **LON-CL1**

ユーザー名: **Adatum\\Administrator**

パスワード: **Pa55w.rd**

このラボでは、提供されている仮想マシン環境を使用します。 ラボを開始する前に、次の手順を行ってください。

1. **LON-DC1** を開き、パスワード **Pa55w.rd** を使って**Adatum\\Administrator** としてサインインします。
1. **LON-SVR1** と **LON-CL1** に対して手順 1 を繰り返します。

## 演習 1:スクリプトへの署名

### 演習のシナリオ 1

セキュリティを強化するために、環境内のすべての Windows PowerShell スクリプトにデジタル署名するという要件を検討しています。 この要件を実装する前に、プロセスをテストしたいと考えています。

この演習の主なタスクは次のとおりです。

1. コード署名証明書をインストールします。
1. スクリプトにデジタル署名する。
1. 実行ポリシーを設定します。

### タスク 1: コード署名証明書をインストールする

1. **LON-CL1** で **[スタート]** を選び、「**mmc.exe**」と入力し、"**mmc.exe**" を選びます。
1. **MMC** コンソールで **[ファイル]** を選び、**[スナップインの追加と削除]** を選びます。
1. **[スナップインの追加と削除]** ウィンドウで **[証明書]** を選び、**[追加]** を選びます。
1. **[証明書スナップイン]** ダイアログ ボックスで **[ユーザー アカウント]** を選択し、**[完了]** をクリックします。
1. **[スナップインの追加と削除]** ウィンドウで **[OK]** を選びます。
1. **MMC** コンソールで **[証明書 - 現在のユーザー]** を展開し、**[個人用]** を選びます。
1. **[個人用]** を右クリックするか、コンテキスト メニューを起動し、**[すべてのタスク]** にカーソルを合わせて **[新しい証明書の要求]** を選びます。
1. **[証明書の登録]** ウィザードの **[開始する前に]** ページで **[次へ]** を選びます。
1. **[証明書の登録ポリシーの選択]** ページで、**[Active Directory の登録ポリシー]** を選び、**[次へ]** を選びます。
1. **[証明書の要求]** ページで、**[Adatum Code Signing]** チェックボックスを選び、**[登録]** を選びます。
1. **[証明書のインストール結果]** ページで **[完了]** を選びます。
1. **MMC** コンソールで **[個人用]** を展開し、**[証明書]** を選び、新しいコード署名証明書が存在することを確認します。
1. **MMC** コンソールを閉じ、プロンプトで **[いいえ]** を選び、コンソールの設定を保存します。

### タスク 2: スクリプトにデジタル署名する

1. **[スタート]** ボタンを選び、「**Powersh**」と入力し、**[Windows PowerShell]** を選びます。
1. [Windows PowerShell] プロンプトで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-ChildItem Cert:\CurrentUser\My\ -CodeSigningCert
   ```

1. [Windows PowerShell] プロンプトで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   $cert = Get-ChildItem Cert:\CurrentUser\My\ -CodeSigningCert
   ```

1. [Windows PowerShell] プロンプトで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Set-Location E:\Mod07\Labfiles
   ```

1. [Windows PowerShell] プロンプトで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Rename-Item HelloWorld.txt HelloWorld.ps1
   ```

1. [Windows PowerShell] プロンプトで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Set-AuthenticodeSignature -FilePath HelloWorld.ps1 -Certificate $cert
   ```

### タスク 3: 実行ポリシーを設定する

1. [Windows PowerShell] プロンプトで、次のコマンドを入力し、Enter キーを押します。 プロンプトで「**Y**」と入力し、Enter キーを押します。

   ```powershell
   Set-ExecutionPolicy AllSigned
   ```

2. [Windows PowerShell] プロンプトで、次のコマンドを入力し、Enter キーを押します。 信頼されていない発行元のソフトウェアを実行するかどうかを尋ねられることがあります。 「**A**」と入力して、Enter キーを押します。

   ```powershell
   .\HelloWorld.ps1
   ```

3. [Windows PowerShell] プロンプトで、次のコマンドを入力し、Enter キーを押します。 プロンプトで「**Y**」と入力し、Enter キーを押します。

   ```powershell
   Set-ExecutionPolicy Unrestricted
   ```

4. Windows PowerShell プロンプトを閉じます。

## 演習 2:ForEach ループを使用する配列の処理

### シナリオ 2

Adatum Corporation では、新しいボイス オーバー IP 通話 (VoIP) およびビデオ会議システムをテストしています。 このシステムをサポートするには、テスト ユーザーのグループに **ipPhone** 属性を設定する必要があります。 **ipPhone** 属性に対して選択されている名前付け規則は、**FirstName.LastName@adatum.com** です。

この演習の主なタスクは次のとおりです。

1. テスト グループを作成します。
1. `ipPhone` 属性を構成するためのスクリプトを作成します。

### タスク 1: テスト グループを作成する

1. **LON-CL1** で **[スタート]** を選択し、「**powersh**」と入力し、**[Windows PowerShell]** を選択します。

1. [Windows PowerShell] プロンプトで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   New-ADGroup -Name IPPhoneTest -GroupScope Universal -GroupCategory Security
   ```

1. [Windows PowerShell] プロンプトで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Move-ADObject "CN=IPPhoneTest,CN=Users,DC=Adatum,DC=com" -TargetPath "OU=IT,DC=Adatum,DC=com"
   ```

1. [Windows PowerShell] プロンプトで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Add-ADGroupMember IPPhoneTest -Members Abbi,Ida,Parsa,Tonia
   ```

### タスク 2: ipPhone 属性を構成するためのスクリプトを作成する

- このタスクを実行するスクリプトは、**E:\\Mod07\\Labfiles\\AZ-040_Mod07_Ex2_LAK.txt** にあります。

## 演習 3:If ステートメントを使用した項目の処理

組織内の一部のサーバーには、サーバーの再起動時に正常に開始されないサービスがあります。 指定されたリストのサービスを開始するために使用できるスクリプトを作成したいと考えています。 十分なテストを行ったら、スクリプトを実行するスケジュールされたタスクを構成する予定です。 テスト フェーズ中は、Windows タイムおよび印刷スプーラー サービスを使用します。

この演習の主なタスクは次のとおりです。

1. サービス名を含む services.txt を作成します。
1. 停止されたサービスを開始するスクリプトを作成します。

### タスク 1: サービス名を含む services.txt を作成する

1. **[スタート]** を選び、「**powersh**」と入力し、**Windows PowerShell** を選びます。
1. [Windows PowerShell] プロンプトで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Set-Location E:\Mod07\Labfiles
   ```

1. [Windows PowerShell] プロンプトで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   New-Item services.txt -ItemType File
   ```

1. [Windows PowerShell] プロンプトで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-Service "Print Spooler" | Select -ExpandProperty Name | Out-File services.txt -Append
   ```

1. [Windows PowerShell] プロンプトで、次のコマンドを入力し、Enter キーを押します。

   ```powershell
   Get-Service "Windows Time" | Select -ExpandProperty Name | Out-File services.txt -Append
   ```

### タスク 2: 停止されたサービスを開始するスクリプトを作成する

- このタスクを実行するスクリプトは、**E:\\Mod07\\Labfiles\\AZ-040_Mod07_Ex3_LAK.txt** にあります。

## 演習 4:CSV ファイルに基づくユーザーの作成

### 演習のシナリオ 4

Adatum のヘルプデスクでは、人事部によって提供されたデータに基づいて、週に 1 回ユーザー アカウントを作成しています。 このデータは CSV ファイルで提供されます。

新しいユーザー アカウントが正しい情報で作成されていないインスタンスが複数あります。 ヘルプデスクでは、CSV ファイルを参照として使用し、グラフィカル ツールを使ってユーザー アカウントを作成しています。 あなたは、これらのエラーを回避するためにこのプロセスを自動化したいと考えています。

この演習の主なタスクは次のとおりです。

- CSV ファイルから AD DS を作成します。

### タスク 1: CSV ファイルから AD DS ユーザーを作成する

- このタスクを実行するスクリプトは、**E:\\Mod07\\Labfiles\\AZ-040_Mod07_Ex4_LAK.txt** にあります。

## エクササイズ 5:リモート コンピューターからのディスク情報のクエリの実行

### 演習のシナリオ 5

Adatum では、すべてのコンピューターの論理ディスク構成を文書化していません。 ドキュメントの情報収集の一環として、論理ディスク情報を収集するためのスクリプトを作成します。

ディスク情報のスクリプトには、次の要件があります。

- リモート コンピューター名をパラメーターとして受け入れます。
- コンピューター名がパラメーターとして指定されていない場合は、ユーザーにコンピューター名の入力を求める必要があります。
- 情報のクエリでは、分散コンポーネント オブジェクト モデル (DCOM) ではなく、Web Services-Management (WS-MAN) を使用する必要があります。
- 物理ディスク情報ではなく、論理ディスク情報 (ボリューム) を表示します。
- ローカル ディスク (ハード ドライブ) の情報のみを含める必要があります。

この演習の主なタスクは次のとおりです。

- 現在の資格情報でディスク情報のクエリを実行するスクリプトを作成します。

### タスク 1: 現在の資格情報でディスク情報のクエリを実行するスクリプトを作成する

1. **LON-CL1** を使ってすべての手順を実行します。
1. このタスクを実行するスクリプトは、**E:\\Mod07\\Labfiles\\AZ-040_Mod07_Ex5_LAK.txt** にあります。

## 演習 6: 別の資格情報を使うスクリプトの更新

### 演習のシナリオ 6

リモート コンピューターからのディスク情報のクエリを実行するスクリプトを実行したいと考えています。 リモート サーバー上のディスク情報のクエリを実行するアクセス許可がユーザーにないシナリオを考慮するために、代替の資格情報が指定されている場合はそれらを受け入れるようにスクリプトを更新します。

このスクリプトは次の要件を満たす必要があります。

- 代替の資格情報が必要かどうかを示すスイッチ パラメーターを受け入れます。
- 代替の資格情報が必要な場合は、これらの資格情報を収集して使用します。

この演習の主なタスクは次のとおりです。

- 代替の資格情報を使用するようにスクリプトを更新します。

### タスク 1: 代替の資格情報を使用するようにスクリプトを更新する

- このタスクを実行するスクリプトは、**E:\\Mod07\\Labfiles\\AZ-040_Mod07_Ex6_LAK.txt** にあります。
