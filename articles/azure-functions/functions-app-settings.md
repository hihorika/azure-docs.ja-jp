---
title: Azure Functions のアプリケーション設定のリファレンス
description: Azure Functions のアプリケーション設定または環境変数の参照ドキュメントです。
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 08/22/2018
ms.author: glenga
ms.openlocfilehash: 9f6746f1bf8fb65e39933afa00b74a2b8266a1a9
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/07/2018
ms.locfileid: "44095438"
---
# <a name="app-settings-reference-for-azure-functions"></a>Azure Functions のアプリケーション設定のリファレンス

関数アプリのアプリケーション設定には、その関数アプリのすべての関数に影響するグローバル構成オプションが含まれています。 ローカルで実行する場合、これらの設定は環境変数にあります。 この記事では、関数アプリで使用できるアプリケーション設定の一覧を紹介します。

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md]

[host.json](functions-host-json.md) ファイルと [local.settings.json](functions-run-local.md#local-settings-file) ファイルには、他のグローバル構成オプションもあります。

## <a name="appinsightsinstrumentationkey"></a>APPINSIGHTS_INSTRUMENTATIONKEY

Application Insights を使用している場合の Application Insights インストルメンテーション キーです。 「[Azure Functions を監視する](functions-monitoring.md)」を参照してください。

|キー|値の例|
|---|------------|
|APPINSIGHTS_INSTRUMENTATIONKEY|5dbdd5e9-af77-484b-9032-64f83bb83bb|

## <a name="azurewebjobsdashboard"></a>AzureWebJobsDashboard

ログの保存と、それらをポータルの **[モニター]** タブに表示する、オプションのストレージ アカウントの接続文字列です。 このストレージ アカウントは、blob、キュー、およびテーブルをサポートする汎用的なものである必要があります。 「[ストレージ アカウント](functions-infrastructure-as-code.md#storage-account)」および「[ストレージ アカウントの要件](functions-create-function-app-portal.md#storage-account-requirements)」を参照してください。

|キー|値の例|
|---|------------|
|AzureWebJobsDashboard|DefaultEndpointsProtocol=https;AccountName=[name];AccountKey=[key]|

## <a name="azurewebjobsdisablehomepage"></a>AzureWebJobsDisableHomepage

`true` は、関数アプリのルート URL 用に表示される既定のランディング ページを無効にすることを意味します。 既定値は `false`です。

|キー|値の例|
|---|------------|
|AzureWebJobsDisableHomepage|true|

このアプリ設定を省略するか、`false` に設定した場合、URL `<functionappname>.azurewebsites.net` の応答に対し、次の例のようなものが表示されます。

![関数アプリのランディング ページ](media/functions-app-settings/function-app-landing-page.png)

## <a name="azurewebjobsdotnetreleasecompilation"></a>AzureWebJobsDotNetReleaseCompilation

`true` は、.NET コードのコンパイルにリリース モードを使用することを意味し、`false` は、デバッグ モードを使用することを意味します。 既定値は `true` です。

|キー|値の例|
|---|------------|
|AzureWebJobsDotNetReleaseCompilation|true|

## <a name="azurewebjobsfeatureflags"></a>AzureWebJobsFeatureFlags

有効にするベータ機能のコンマ区切りの一覧です。 これらのフラグで有効となるベータ機能は本番には適しませんが、公開前の実験的な使用には有効にすることができます。

|キー|値の例|
|---|------------|
|AzureWebJobsFeatureFlags|feature1,feature2|

## <a name="azurewebjobsscriptroot"></a>AzureWebJobsScriptRoot

*host.json* ファイルと関数フォルダーがあるルート ディレクトリへのパスです。 関数アプリでの既定は、`%HOME%\site\wwwroot` です。

|キー|値の例|
|---|------------|
|AzureWebJobsScriptRoot|%HOME%\site\wwwroot|

## <a name="azurewebjobssecretstoragetype"></a>AzureWebJobsSecretStorageType

キーの保存に使用するリポジトリまたはプロバイダーを指定します。 現時点では、blob ("Blob") およびファイル システム ("disabled") のリポジトリがサポートされています。 既定のファイル システムは ("disabled") です。

|キー|値の例|
|---|------------|
|AzureWebJobsSecretStorageType|disabled|

## <a name="azurewebjobsstorage"></a>AzureWebJobsStorage

Azure Functions ランタイムは、HTTP によってトリガーされる関数を除くすべての関数で、このストレージ アカウントの接続文字列を使用します。 このストレージ アカウントは、blob、キュー、およびテーブルをサポートする汎用的なものである必要があります。 「[ストレージ アカウント](functions-infrastructure-as-code.md#storage-account)」および「[ストレージ アカウントの要件](functions-create-function-app-portal.md#storage-account-requirements)」を参照してください。

|キー|値の例|
|---|------------|
|AzureWebJobsStorage|DefaultEndpointsProtocol=https;AccountName=[name];AccountKey=[key]|

## <a name="azurewebjobstypescriptpath"></a>AzureWebJobs_TypeScriptPath

Typescript で使用されるコンパイラへのパスです。 必要に応じて、既定値はオーバーライドできます。

|キー|値の例|
|---|------------|
|AzureWebJobs_TypeScriptPath|%HOME%\typescript|

## <a name="functionappeditmode"></a>FUNCTION\_APP\_EDIT\_MODE

有効な値は "readwrite" および "readonly" です。

|キー|値の例|
|---|------------|
|FUNCTION\_APP\_EDIT\_MODE|readonly|

## <a name="functionsextensionversion"></a>FUNCTIONS\_EXTENSION\_VERSION

この関数アプリで使用する Azure Functions ランタイムのバージョンです。 メジャー バージョンのチルダ (例: "~1") は、そのメジャー バージョンの最新バージョンを使用することを意味します。 同じメジャー バージョンの新しいバージョンが使用できる場合、それらは関数アプリに自動的にインストールされています。 特定のバージョンにアプリを固定するには、完全なバージョン番号 (例: "1.0.12345") を使用します。 既定は "~1" です。

|キー|値の例|
|---|------------|
|FUNCTIONS\_EXTENSION\_VERSION|~1|

## <a name="websitecontentazurefileconnectionstring"></a>WEBSITE_CONTENTAZUREFILECONNECTIONSTRING

従量課金プラン用のみです。 関数アプリのコードと構成が格納されているストレージ アカウントの接続文字列です。 「[Function App を作成する](functions-infrastructure-as-code.md#create-a-function-app)」を参照してください。

|キー|値の例|
|---|------------|
|WEBSITE_CONTENTAZUREFILECONNECTIONSTRING|DefaultEndpointsProtocol=https;AccountName=[name];AccountKey=[key]|

## <a name="websitecontentshare"></a>WEBSITE\_CONTENTSHARE

従量課金プラン用のみです。 関数アプリ コードと構成へのファイル パスです。 WEBSITE_CONTENTAZUREFILECONNECTIONSTRING と共に使用されます。 既定は、関数アプリ名で始まる一意文字列です。 「[Function App を作成する](functions-infrastructure-as-code.md#create-a-function-app)」を参照してください。

|キー|値の例|
|---|------------|
|WEBSITE_CONTENTSHARE|functionapp091999e2|

## <a name="websitemaxdynamicapplicationscaleout"></a>WEBSITE\_MAX\_DYNAMIC\_APPLICATION\_SCALE\_OUT

関数アプリがスケールアウトできる最大のインスタンス数です。 既定は無制限です。

> [!NOTE]
> この設定は、プレビュー機能用です。

|キー|値の例|
|---|------------|
|WEBSITE\_MAX\_DYNAMIC\_APPLICATION\_SCALE\_OUT|10|

## <a name="websitenodedefaultversion"></a>WEBSITE\_NODE\_DEFAULT_VERSION

既定は "6.5.0" です。

|キー|値の例|
|---|------------|
|WEBSITE\_NODE\_DEFAULT_VERSION|6.5.0|

## <a name="websiterunfromzip"></a>WEBSITE\_RUN\_FROM\_ZIP

マウントされたパッケージ ファイルから関数アプリを実行できるようにします。

> [!NOTE]
> この設定は、プレビュー機能用です。

|キー|値の例|
|---|------------|
|WEBSITE\_RUN\_FROM\_ZIP|1|

有効な値は、展開パッケージ ファイルの場所に解決される URL、または `1` です。 `1` に設定した場合、パッケージは `d:\home\data\SitePackages` フォルダーに存在する必要があります。 この設定で zip デプロイを使用すると、パッケージは自動的にこの場所にアップロードされます。  詳細については、[パッケージ ファイルからの関数の実行](run-functions-from-deployment-package.md)に関するページを参照してください。

## <a name="next-steps"></a>次の手順

[アプリケーション設定の更新方法](functions-how-to-use-azure-function-app-settings.md#manage-app-service-settings)

[host.json ファイルのグローバル設定を参照する](functions-host-json.md)

[App Service アプリの他のアプリ設定を参照する](https://github.com/projectkudu/kudu/wiki/Configurable-settings)
