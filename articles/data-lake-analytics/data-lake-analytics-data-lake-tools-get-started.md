---
title: Visual Studio で Azure Data Lake Analytics を使い始める
description: Data Lake Tools for Visual Studio のインストール方法と、U-SQL スクリプトの開発およびテスト方法について説明します。
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.topic: conceptual
ms.date: 08/13/2018
ms.openlocfilehash: 2af1cf1c41ddacd973b3e1dc0f2dd7a8b6d9563a
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/27/2018
ms.locfileid: "43045957"
---
# <a name="develop-u-sql-scripts-by-using-data-lake-tools-for-visual-studio"></a>Data Lake Tools for Visual Studio を使用して U-SQL スクリプトを開発する
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Visual Studio を使用して Azure Data Lake Analytics アカウントを作成し、[U-SQL](data-lake-analytics-u-sql-get-started.md) でジョブを定義し、Data Lake Analytics サービスにジョブを送信する方法について説明します。 Data Lake Analytics の詳細については、「 [Azure Data Lake Analytics の概要](data-lake-analytics-overview.md)」を参照してください。

>[!IMPORTANT]
> Microsoft では、Azure Data Lake Tools for Visual Studio バージョン 2.3.3000.4 以降にアップグレードすることをお勧めしています。 以前のバージョンはダウンロードできなくなり、非推奨になっています。 
>
>**必要な操作**
>
>1. Azure Data Lake Tools for Visual Studio 2.3.3000.4 より前のバージョンを使用していないかどうかを確認します。 
>   
>   ![ツールのバージョンを確認する](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-about-data-lake.png)
> 
>2. バージョンが 2.3.3000.4 より前の場合は、ダウンロード センターにアクセスして、Azure Data Lake Tools for Visual Studio を更新します。 
>    - [Visual Studio 2017 の場合](https://marketplace.visualstudio.com/items?itemName=ADLTools.AzureDataLakeandStreamAnalyticsTools)
>    - [Visual Studio 2013 および 2015 の場合](https://www.microsoft.com/en-us/download/details.aspx?id=49504)


## <a name="prerequisites"></a>前提条件

* **Visual Studio**: Express を除くすべてのエディションがサポートされます。
    * Visual Studio 2017
    * Visual Studio 2015
    * Visual Studio 2013
* **Microsoft Azure SDK for .NET** バージョン 2.7.1 以上。  [Web プラットフォーム インストーラー](http://www.microsoft.com/web/downloads/platform.aspx)を使用してインストールします。
* **Data Lake Analytics** アカウント。 アカウントを作成するには、「[Azure Portal で Azure Data Lake Analytics の使用を開始する](data-lake-analytics-get-started-portal.md)」を参照してください。

## <a name="install-azure-data-lake-tools-for-visual-studio"></a>Azure Data Lake Tools for Visual Studio をインストールする

このチュートリアルでは、Data Lake Tools for Visual Studio がインストールされている必要があります。 [インストール手順](data-lake-analytics-data-lake-tools-install.md)に従ってください。

## <a name="connect-to-an-azure-data-lake-analytics-account"></a>Azure Data Lake Analytics アカウントに接続する

1. Visual Studio を開きます。

2. **[表示]** > **[サーバー エクスプローラー]** を選択して、サーバー エクスプローラーを開きます。

3. **[Azure]** を右クリックします。 **[Microsoft Azure サブスクリプションへの接続]** をクリックし、指示に従います。

4. サーバー エクスプローラーで、**[Azure]** > **[Data Lake Analytics]** を選択します。 Data Lake Analytics アカウントの一覧が表示されます。

## <a name="write-your-first-u-sql-script"></a>最初の U-SQL スクリプトを記述する

次のテキストは、単純な U-SQL スクリプトです。 それは、小さなデータセットを定義し、そのデータセットを `/data.csv` という名前のファイルとして既定の Data Lake Store に書き込みます。

```
USE DATABASE master;
USE SCHEMA dbo;
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

## <a name="submit-a-data-lake-analytics-job"></a>Data Lake Analytics ジョブの送信

1. **[ファイル]** > **[新規作成]** > **[プロジェクト]** の順に選択します。

2. プロジェクトの種類として **[U-SQL プロジェクト]** を選択し、**[OK]** をクリックします。 Visual Studio によって、**Script.usql** ファイルを使用するソリューションが作成されます。

3. 前のスクリプトを **Script.usql** ウィンドウに貼り付けます。

4. **Script.usql** ウィンドウの左上隅に、Data Lake Analytics アカウントを指定します。

    ![U-SQL Visual Studio プロジェクトの送信](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

5. **Script.usql** ウィンドウの左上隅の **[送信]** を選択します。

6. ジョブの送信後、**[ジョブ ビュー]** タブが開き、ジョブの進行状況が表示されます。 最新のジョブの状態を表示して画面を更新するには、**[更新]** をクリックします。

    ![U-SQL Visual Studio Data Lake Analytics ジョブ パフォーマンス グラフ](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

   * **ジョブの概要**: ジョブの概要を示します。   
   * **ジョブ グラフ**: ジョブの進行状況を視覚化します。
   * **メタデータ操作**: U-SQL カタログに対して実行されたすべてのアクションを示します。
   * **データ**: すべての入力と出力を示します。
   * **状態の履歴**: タイムラインと状態の詳細を示します。
   * **AU 分析**: ジョブで使用された AU の数を示し、異なる AU 割り当て方法のシミュレーションを検討します。
   * **診断**: ジョブの実行とパフォーマンスを最適化するための高度な分析を提供します。

## <a name="check-job-status"></a>ジョブの状態の確認

1. サーバー エクスプローラーで、**[Azure]** > **[Data Lake Analytics]** を選択します。

2. Azure Data Lake Analytics アカウントの名前を展開します。

3. **[ジョブ]** をダブルクリックします。

4. 先ほど送信したジョブを選択します。

## <a name="see-the-job-output"></a>ジョブの出力の表示

1. サーバー エクスプローラーで、送信したジョブを参照します。

2. **[データ]** タブをクリックします。

3. **[ジョブの出力]** タブで、`"/data.csv"` ファイルを選択します。

## <a name="next-steps"></a>次の手順

* [ご使用のワークステーションでのテストおよびデバッグのための U-SQL スクリプトの実行](data-lake-analytics-data-lake-tools-local-run.md)
* [Azure Data Lake Tools for Visual Studio Code を使用した U-SQL ジョブの C# コードのデバッグ](data-lake-tools-for-vscode-local-run-and-debug.md)
* [Azure Data Lake Tools for Visual Studio Code の使用](data-lake-analytics-data-lake-tools-for-vscode.md)
