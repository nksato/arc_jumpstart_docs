[![Stale Branches](https://github.com/Azure/arc_jumpstart_docs/actions/workflows/stale-branches.yml/badge.svg)](https://github.com/Azure/arc_jumpstart_docs/actions/workflows/stale-branches.yml) [![Check Broken Links](https://github.com/Azure/arc_jumpstart_docs/actions/workflows/url-checker.yml/badge.svg?branch=main)](https://github.com/Azure/arc_jumpstart_docs/actions/workflows/url-checker.yml)

# ⚡ Arc Jumpstart ドキュメント

Arc Jumpstart ドキュメントリポジトリへようこそ！このリポジトリは、Azure Arc に関する詳細なガイド、ベストプラクティス、および詳細なドキュメントの情報源です。Azure Arc の基礎を探求する初心者から、デプロイメントを最適化する経験豊富なユーザーまで、あらゆるニーズに応える貴重な情報をご提供しています。このリポジトリは [ソースコードリポジトリ](https://aka.ms/JumpstartGitHubCode) を補完し、[Arc Jumpstart](https://aka.ms/arcjumpstart) ウェブサイトのドキュメントソースリポジトリとして機能します。

<p align="center">
  <img src="/img/logo/jumpstart.png" alt="Arc Jumpstart ロゴ" width="320">
</p>

**注意:** このリポジトリには、Arc Jumpstart の自動化スクリプトおよびツールのソースコードは含まれていません。Arc Jumpstart のソースコードは、別の [専用リポジトリ](https://aka.ms/JumpstartGitHubCode) にあります。

## 📦 このリポジトリに含まれるもの

- **ドキュメント:** シナリオやソリューションで使用される Arc Jumpstart の Markdown ファイル。包括的な技術情報を提供します。
- **ビジュアルリソース:** ドキュメントの理解を助ける明確で簡潔なスクリーンショット。
- **補助ドキュメントおよびファイル:** [Arc Jumpstart](https://aka.ms/ArcJumpstart) ウェブサイト全体で使用される追加リソース。さまざまなコンテキストをサポートし、補足情報を提供します。

## 🛠️ このリポジトリの活用方法

このドキュメントリポジトリはコントリビューター向けに設計されており、[ソースコードリポジトリ](https://aka.ms/JumpstartGitHubCode) と連携して機能します。必須ではありませんが、コントリビューターは Arc Jumpstart に効果的に貢献するために、両リポジトリをクローンする必要がある場合がほとんどです。

始める前に、包括的な [コントリビューションガイドライン](https://aka.ms/JumpstartContribution) をご確認ください。これらのガイドラインは、ドキュメント全体の一貫性と品質を確保するための標準と実践方法を概説しています。

今後の貢献に迷いがある場合は、遠慮なく [GitHub ディスカッション](https://aka.ms/JumpstartDiscussions) を開始してください。ここは質問、アイデアの共有、潜在的な貢献に関するフィードバックを得るための場所です。私たちのコミュニティはサポートの準備ができており、あらゆるレベルの経験を歓迎しています。

コントリビューションをお楽しみください！

## 🌿 ブランチガイダンス

Arc Jumpstart ドキュメントリポジトリは、ほとんどのコードリポジトリと同様にブランチを管理しています。2 つの主要なブランチが維持されており、それぞれ特定のウェブサイトスロット（本番/カナリア）に対応しています。

現在維持されているブランチは次のとおりです:

| ブランチ                                                      | ウェブサイト                   | 説明                                                                                      |
| ------------------------------------------------------------ | -------------------------- | ------------------------------------------------------------------------------------------------ |
| [main](https://github.com/Azure/arc_jumpstart_docs) (プライマリ) | https://jumpstart.azure.com/ | 最新の Arc Jumpstart リリースドキュメント。これは本番スロットにデプロイされた最新のドキュメントです。 |
| [canary](https://github.com/Azure/arc_jumpstart_docs/tree/canary) (カナリア) | https://bvt.test.arcjumpstart.azure.com/ | プレリリースドキュメント。ドキュメントの更新は、メインブランチにマージする前にカナリアブランチにマージしてプレビュー検証を行う必要があります。 |

## 📥 リポジトリのクローン

貢献するには、このリポジトリと [ソースコードリポジトリ](https://github.com/Azure/arc_jumpstart_docs) の両方をクローンする必要があります。次のコマンドを使用してください:

```bash
git clone https://github.com/Azure/arc_jumpstart_docs.git
git clone https://github.com/microsoft/azure_arc.git
```

Arc Jumpstart を継続的に改善・拡張しているため、リポジトリのローカルクローンを最新の状態に保つことをお勧めします。メインブランチから最新の変更をプルすることで、これを行えます:

```bash
git pull origin main
```

## 🙌 貢献とフィードバック

皆さまのご意見は非常に重要です！提案、フィードバック、または貴重な洞察がある場合は、遠慮なく Issue を開いてください。皆さまのご貢献がコミュニティ全体のドキュメントの改善に役立ちます。

このプロジェクトはコントリビューションと提案を歓迎しています。ほとんどのコントリビューションでは、コントリビューターライセンス契約（CLA）への同意が必要で、あなたがコントリビューションを行う権利を持ち、実際に Arc Jumpstart チームにコントリビューションを使用する権利を付与することを宣言します。詳細については、https://cla.opensource.microsoft.com をご覧ください。

プルリクエストを提出すると、CLA ボットが自動的に CLA を提供する必要があるかどうかを判断し、PR を適切に修飾します（例: ステータスチェック、コメント）。ボットが提供する指示に従ってください。これは Microsoft の CLA を使用するすべてのリポジトリで一度だけ行う必要があります。

このプロジェクトは [Microsoft オープンソース行動規範](https://opensource.microsoft.com/codeofconduct/) を採用しています。

詳細については、[行動規範 FAQ](https://opensource.microsoft.com/codeofconduct/faq/) をご覧いただくか、追加の質問やコメントは [opencode@microsoft.com](mailto:opencode@microsoft.com) までお問い合わせください。

## ✍️ ライティングガイドライン

このリポジトリでは、技術ライターとコントリビューターが Microsoft のライティングスタイルガイドラインを遵守できるよう、[vale.sh](https://vale.sh/) を使用しています。**Vale** は、一連のスタイルルールに対して文法やスペルのエラーを確認できるコマンドラインツールです。

Vale の設定、ローカル使用、および GitHub CI アクションの詳細については、[Wiki - Vale Integration](https://github.com/Azure/arc_jumpstart_docs/wiki/Vale.sh-Integration) をご確認ください。

## 📑 リポジトリポリシー

自動化されたプルリクエストと Issue 管理ポリシーの詳細については、[REPO_POLICIES.md](./REPO_POLICIES.md) をご覧ください。

## 🏷️ 商標

このプロジェクトには、プロジェクト、製品、またはサービスの商標やロゴが含まれる場合があります。Microsoft の商標またはロゴの許可された使用は、[Microsoft の商標およびブランドガイドライン](https://www.microsoft.com/legal/intellectualproperty/trademarks/usage/general) に従う必要があります。

このプロジェクトの改変バージョンで Microsoft の商標またはロゴを使用することで、混乱が生じたり、Microsoft のスポンサーシップを暗示したりしてはなりません。
第三者の商標またはロゴの使用は、それぞれの第三者のポリシーに従います。
