---
type: docs
linkTitle: "LocalBox FAQ"
weight: 7
---

# Jumpstart LocalBox よくある質問（FAQ）

## Jumpstart LocalBox とは何ですか？

Jumpstart LocalBox は、仮想環境で Azure Local の機能とハイブリッド クラウド統合を探索するための完全なサンドボックスをターンキーで提供するソリューションです。LocalBox は単一の Azure サブスクリプションとリソース グループ内に完全に自己完結するよう設計されており、物理ハードウェアを用意することなく Azure Local や Azure Arc テクノロジを実際に体験できます。

> **注:** Arc Jumpstart に関する一般的な質問については、[Jumpstart FAQ](../../faq/) を参照してください。

## LocalBox のデプロイに必要なものは何ですか？

LocalBox のデプロイには、Azure サブスクリプションに対して所有者ロールベース アクセス制御（RBAC）を持つユーザーが必要で、Azure Bicep を使用してデプロイできます。この割り当てには、所有者ロールを制約する可能性のある[条件](https://learn.microsoft.com/azure/role-based-access-control/delegate-role-assignments-portal?tabs=condition-editor)が含まれていないことが必要です。Bicep テンプレートによってマネージド ID がプロビジョニングされ、LocalBox の機能をデプロイ・構成する自動化スクリプトで Azure への認証に使用されます。マネージド ID の使用方法は、[公開 GitHub リポジトリ](https://github.com/microsoft/azure_arc)で LocalBox のコードを確認することで参照できます。

## LocalBox はどの Azure リージョンにデプロイできますか？

LocalBox は、選択した VM SKU（Standard E32s v5 または v6）に対して十分なコンピューティング容量（vCPU クォータ）があるリージョンであればどこにでもデプロイできます。

LocalBox は以下の Azure リージョンでテスト済みです：

- 米国東部
- オーストラリア東部
- カナダ中部
- ノルウェー東部
- スウェーデン中部
- 北ヨーロッパ
- 西ヨーロッパ

一部の LocalBox リソースは、必要なサービスが利用可能な[特定のリージョン](https://learn.microsoft.com/en-us/azure/azure-local/concepts/system-requirements-23h2?view=azloc-2505&tabs=azure-public#azure-requirements)にデプロイされます：

- 米国東部
- 西ヨーロッパ
- オーストラリア東部
- 東南アジア
- インド中部
- カナダ中部
- 東日本
- 米国中南部

これは `azureLocalInstanceLocation` デプロイ パラメーターで指定することで制御できます。

## LocalBox の利用コストはどれくらいですか？

LocalBox では、仮想マシンやストレージなどのさまざまな Azure リソースに対して通常の Azure 消費料金が発生します。

コンピューティング コストを削減するために [Azure スポット VM](https://learn.microsoft.com/azure/virtual-machines/spot-vms) の使用を検討してください。ただし、Azure が容量を必要とするときに削除される可能性があります。`enableAzureSpotPricing` デプロイ パラメーターに `true` を指定することで有効にできます。

以下のリンクで LocalBox のコスト見積もり例を確認できます。

- [LocalBox コスト見積もり](https://aka.ms/LocalBoxCost)

## LocalBox はどのバージョンの Azure Local をサポートしていますか？

LocalBox は [Azure Local OS](https://learn.microsoft.com/azure/azure-local/deploy/operating-system?view=azloc-2505) の 24H2 ビルドを使用しています。

## LocalBox のデプロイや使用で問題が発生した場合はどこに連絡すればよいですか？

困った場合は、Jumpstart GitHub リポジトリに[イシューを送信](https://github.com/microsoft/azure_arc/issues/new/choose)してください。Jumpstart チームがサポートします。
