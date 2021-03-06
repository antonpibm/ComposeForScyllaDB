---

Copyright:
  years: 2017,2018
lastupdated: "2018-05-07"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# サービス概要

{{site.data.keyword.cloud}} Compose データベースに関する情報は_「概要」_ページに表示されます。 概要には、必要不可欠な識別情報と現在のリソース使用率が含まれます。 ツールで使用する接続ストリングのセクションもあります。接続ストリングを使用してツールからデータベースに接続することもできます。

## デプロイメントの詳細

_「デプロイメントの詳細 (Deployment Details)」_パネルには、サービスの詳細が表示されます。

![デプロイメントの詳細](./images/scylla-deployment-details.png "「デプロイメントの詳細 (Deployment Details)」パネルの表示")

### タイプ

サービスによって提供されるデータベースのタイプ、およびサービスが使用するデータベースのバージョン。 より新しいバージョンのデータベースが利用可能な場合は、通知が表示され、サービス・ダッシュボードの[「バージョンのアップグレード」](/docs/services/ComposeForScyllaDB/dashboard-settings.html#upgrade-version)セクションへのリンクが示されます。

### ID

サービスの内部 ID。

### 使用法

データベース・サイズとご使用のサービス・プランで利用可能なストレージ容量。

## 現行ジョブ

サービスに関する管理上の変更 (スケーリングや手動バックアップなど) を行うと、ジョブが開始されます。 ジョブの実行中に、_「概要 (Overview)」_ページに_「現行ジョブ (Current Jobs)」_パネルが表示され、ジョブ名と進行状況表示バーが示されます。 ジョブが完了すると、_「現行ジョブ (Current Jobs)」_パネルは_「概要 (Overview)」_ページに表示されなくなります。

## 接続ストリング (Connection Strings)

_「接続ストリング」_パネルの各タブで、サービスで使用できるそれぞれの接続ストリングを確認できます。

### HTTPS

いくつかのクライアント・ライブラリーで使用できる URI フォーマットの接続ストリングで、他のライブラリーが接続するために必要なすべての情報を含んでいます。 接続ストリングを使用して接続する方法については、[外部アプリケーションの接続](./connecting-external.html)を参照してください。

### コマンド・ライン

**コマンド・ライン**は、正しいパラメーターを使用して `cqlsh` を呼び出すために予め形式設定されたコマンドです。 表示されるコマンドには、必須フラグ (`--ssl` および `--cqlversion`) が含まれています。  これを使用するには、ローカル・システムに PostgreSQL クライアント・ツールをインストールする必要があります。 その方法については、 [外部アプリケーションの接続](./connecting-external.html)を参照してください。

### マップ
これらのアドレス変換マップは、オートディスカバリーを使用してクラスター内のノードを検出できる、高可用性が必要なアプリケーションで使用できます。 これらのアプリケーションは、この機能を使用するクライアント・ドライバーのために、外部ポータル・アドレスを内部クラスター・アドレスに変換します。

### Socks と nodetool
nodetool で Scylla を管理できるようにするために、このサービスには、SOCKS プロキシーとして構成された SSH カプセルが用意されています。 そのプロキシーを使用するには、デプロイメントに SSH 鍵を追加する必要があります。 詳細については、[nodetool の使用](./scylla-nodetool.html)を参照してください。


## インスタンス管理 API

{{site.data.keyword.cloud_notm}} Compose API で {{site.data.keyword.composeForScyllaDB}} サービスを管理できます。

### ファウンデーション・エンドポイント

ファウンデーション・エンドポイントは、サービスが存在する地域とサービス・インスタンス ID で構成されます。 これは、すべてのエンドポイントの先頭に配置されます。

### デプロイメント ID

デプロイメント ID はほとんどの呼び出しで必要であり、特定のデプロイメント・インスタンスを識別します。

### 参照

{{site.data.keyword.cloud_notm}} Compose サービス全体にわたる {{site.data.keyword.cloud_notm}} Compose API の使用についての詳しい資料やリファレンスは、[ {{site.data.keyword.cloud_notm}} Compose API](https://www.compose.com/articles/the-ibm-cloud-compose-api/) を参照してください。
