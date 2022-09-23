---
lab:
  title: 'ラボ: WMI と CIM を使って情報についてのクエリを実行する'
  module: 'Module 5: Querying management information by using CIM and WMI'
ms.openlocfilehash: e32bb5ff582e809e039a15af7a47cbb4b60018c9
ms.sourcegitcommit: 9c31a6ab628c30fac88ec9070c3d807f2a9bbfdb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2022
ms.locfileid: "146824954"
---
# <a name="lab-querying-information-by-using-wmi-and-cim"></a>ラボ: WMI と CIM を使って情報についてのクエリを実行する

## <a name="scenario"></a>シナリオ

複数のコンピューターに管理情報のクエリを実行する必要があります。 まず、環境内のローカル コンピューターと 1 つのテスト コンピューターにクエリを実行します。

## <a name="objectives"></a>目標

このラボを完了すると、次のことができるようになります。

- Windows Management Instrumentation (WMI) コマンドを使用して情報についてのクエリを実行する。
- Common Information Model (CIM) コマンドを使用して情報についてのクエリを実行する。
- WMI および CIM コマンドを使用してメソッドを呼び出します。

## <a name="estimated-time-45-minutes"></a>予想所要時間: 45 分

## <a name="lab-setup"></a>ラボのセットアップ

仮想マシン: **AZ-040T00A-LON-DC1**、**AZ-040T00A-LON-CL1**

ユーザー名: **Adatum\\Administrator**

パスワード: **Pa55w.rd**

このラボでは、使用可能な仮想マシン環境を使います。 ラボを開始する前に、次の手順を行ってください。

1. **LON-DC1** を開き、パスワード **Pa55w.rd** を使って **Adatum\\Administrator** としてサインインします。
1. **LON-CL1** について手順 1 を繰り返します。

## <a name="exercise-1-querying-information-by-using-wmi"></a>演習 1: WMI を使って情報についてのクエリを実行する

### <a name="scenario-1"></a>シナリオ 1

この演習では、リポジトリ クラスを検出し、WMI コマンドを使用してこれに対するクエリを実行します。

この演習の主なタスクは次のとおりです。

1. IP アドレスのクエリを実行する。
1. オペレーティング システムのバージョン情報のクエリを実行する。
1. コンピューター システムのハードウェア情報のクエリを実行する。
1. サービス情報のクエリを実行する。

### <a name="task-1-query-ip-addresses"></a>タスク 1: IP アドレスのクエリを実行する

1. **LON-CL1** で、Windows PowerShell を管理者として開始します。
1. IP アドレスは、ネットワーク アダプターの構成の一部です。 キーワード **configuration** を使用して、ローカル コンピューターで使用されている IP アドレスを一覧表示するリポジトリ クラスを検索します。
1. WMI コマンドと、前の手順で検出したクラスを使用して、静的に構成された IP アドレスの一覧を表示します。

### <a name="task-2-query-operating-system-version-information"></a>タスク 2: オペレーティング システムのバージョン情報のクエリを実行する

1. キーワード **operating** を使用して、オペレーティング システムのバージョン情報を一覧表示するリポジトリ クラスを検索します。 これを **name** プロパティで並べ替えます。
1. 前の手順で検出したクラスのプロパティの一覧を表示します。
1. オペレーティング システムのバージョン、Service Pack のメジャー バージョン、オペレーティング システムのビルド番号を含むプロパティをメモします。
1. WMI コマンドと手順 1 で検出したクラスを使用して、ローカル オペレーティング システムのバージョン、Service Pack のメジャー バージョン、オペレーティング システムのビルド番号を表示します。

### <a name="task-3-query-computer-system-hardware-information"></a>タスク 3: コンピューター システムのハードウェア情報のクエリを実行する

1. キーワード **system** を使って、コンピューター システム情報を含むリポジトリ クラスを検索します。
1. 前の手順で検出したクラスのプロパティとプロパティ値の一覧を表示します。
1. プロパティの一覧と WMI コマンドを使用して、ローカル コンピューターの製造元、モデル、物理メモリの合計容量を表示します。 物理メモリの合計容量の列に **RAM** というラベルを付けます。

### <a name="task-4-query-service-information"></a>タスク 4: サービス情報のクエリを実行する

1. キーワード **service** を使って、サービス情報を含むリポジトリ クラスを検索します。
1. 前の手順で検出したクラスのプロパティとプロパティ値の一覧を表示します。
1. プロパティの一覧と WMI コマンドを使用して、名前が **S** で始まるすべてのサービスのサービス名、状態 (実行中または停止済み)、サインイン名を表示します。

## <a name="exercise-2-querying-information-by-using-cim"></a>演習 2: CIM を使って情報についてのクエリを実行する

### <a name="scenario-2"></a>シナリオ 2

この演習では、新しいリポジトリ クラスを検出し、CIM コマンドを使用してクエリを実行します。

この演習の主なタスクは次のとおりです。

1. ユーザー アカウントのクエリを実行する。
1. BIOS 情報のクエリを実行する。
1. ネットワーク アダプターの構成情報のクエリを実行する。
1. ユーザー グループ情報のクエリを実行する。

### <a name="task-1-query-user-accounts"></a>タスク 1: ユーザー アカウントのクエリを実行する

1. CIM コマンドとキーワード **user** を使用して、ユーザー アカウントを一覧表示するリポジトリ クラスを検索します。
1. CIM コマンドを使って、前の手順で検出したクラスのプロパティの一覧を表示します。
1. CIM コマンドとプロパティ リストを使用して、テーブル内のユーザー アカウントの一覧を表示します。 アカウント キャプション、ドメイン、セキュリティ ID、フル ネーム、名前の列を含めます。 一部またはすべてのアカウントで、フル ネームの列が空白になる場合があります。

### <a name="task-2-query-bios-information"></a>タスク 2: BIOS 情報のクエリを実行する

1. キーワード **bios** と CIM コマンドを使用して、BIOS 情報を含むリポジトリ クラスを検索します。 
1. CIM コマンドと、前の手順で検出したクラスを使用して、使用可能なすべての BIOS 情報の一覧を表示します。

### <a name="task-3-query-network-adapter-configuration-information"></a>タスク 3: ネットワーク アダプターの構成情報のクエリを実行する

1. CIM コマンドを使用して、`Win32_NetworkAdapterConfiguration` クラスのすべてのローカル インスタンスを表示します。
1. CIM コマンドを使用して、**LON-DC1** に存在する `Win32_NetworkAdapterConfiguration` クラスのすべてのインスタンスを表示します。

### <a name="task-4-query-user-group-information"></a>タスク 4: ユーザー グループ情報のクエリを実行する

1. CIM コマンドとキーワード **group** を使用して、ユーザー グループを一覧表示するクラスを検索します。
1. CIM コマンドを使用して、**LON-DC1** に存在するユーザー グループの一覧を表示します。

## <a name="exercise-3-invoking-methods"></a>演習 3: メソッドの呼び出し

### <a name="scenario-3"></a>シナリオ 3

この演習では、WMI および CIM コマンドを使用して、リポジトリ オブジェクトのメソッドを呼び出します。

この演習の主なタスクは次のとおりです。

1. CIM メソッドを呼び出す。
1. WMI メソッドを呼び出す。

### <a name="task-1-invoke-a-cim-method"></a>タスク 1: CIM メソッドを呼び出す

- CIM コマンドと `Win32_OperatingSystem` の `Reboot` メソッドを使用して、**LON-CL1** からリモートで **LON-DC1** を再起動します。

### <a name="task-2-invoke-a-wmi-method"></a>タスク 2: WMI メソッドを呼び出す

1. `Get-Service` コマンドレットを使用して、**WinRM** サービスの **StartType** プロパティを確認します。
1. WMI コマンドと `Win32_Service` の `ChangeStartMode` メソッドを使用して、**WinRM** サービスの開始モードを **[自動]** に変更します。
1. `Get-Service` コマンドレットを使用して、**WinRM** サービスの **StartType** プロパティが **[自動]** に更新されたことを確認します。
