---
type: docs
linkTitle: "トラブルシューティング"
weight: 10
---
# LocalBox のトラブルシューティング

## デプロイのトラブルシューティング

LocalBox のデプロイが途中で失敗することがあります。デプロイが失敗する一般的な原因としては以下が挙げられます：

- ターゲット Azure リージョンで利用可能な vCPU クォータが不足している — vCPU クォータを確認して、少なくとも 32 が利用可能であることを確認してください。詳細については、[前提条件](../getting_started/#prerequisites) セクションを参照してください。
- LocalBox VHD ファイルのダウンロード中の破損によりデプロイが中断される可能性があります。LocalBox はこれが発生した場合に自動的に停止します。_C:\LocalBox\LocalBoxLogonScript.ps1_ で PowerShell スクリプトを再実行すると、多くの場合この問題を修復できます。
- RBAC 権限 — 所有者権限が、所有者ロールを制約する可能性のある[条件](https://learn.microsoft.com/azure/role-based-access-control/delegate-role-assignments-portal?tabs=condition-editor)なしでデプロイを実行するユーザーに割り当てられているかどうかを確認してください。

LocalBox のデプロイで解決できない問題が発生した場合は、[GitHub リポジトリ](https://github.com/microsoft/azure_arc/issues)にイシューを送信してください。

## Pester テストを使用したデプロイの確認

LocalBox は、インフラストラクチャが正しくデプロイされたことを確認するために Pester テスト スイートを自動的に実行します。これらのテストはデプロイ プロセスの最後とユーザー ログオン時に実行されます。

### テストが確認する内容

Pester テスト スイートには主に 2 つのテスト ファイルが含まれています：

- **共通テスト** (`common.tests.ps1`): LocalBox リソース グループに 25 以上の期待されるリソースが含まれていることを確認します
- **Azure Local テスト** (`azlocal.tests.ps1`): 以下を確認します：
  - 仮想マシンが存在し実行中であること
  - Azure Arc 接続済みマシンが適切に登録・接続されていること
  - 自動デプロイが有効な場合、Azure Local クラスターが存在し「Connected」状態であること

### テスト結果の確認

テスト結果は複数の場所に表示されます：

1. **デスクトップ壁紙**: テスト結果は BGInfo を使用してデスクトップ壁紙に自動的に追加され、合格・失敗したテスト数が表示されます
2. **Azure リソース タグ**: テスト結果はリソース グループと LocalBox-Client VM の両方にタグとして保存されます：
   - `DeploymentStatus`: テスト数を表示（例：「Tests succeeded: 15 Tests failed: 0」）
   - `DeploymentProgress`: 全体的なステータスを表示（「Completed」または「Failed」）

### テスト ログの確認

特定のテストが失敗した場合、詳細なテスト出力を表示するには：

1. ログ フォルダーに移動します：`C:\LocalBox\Logs\`
2. `DeploymentStatus.log` を開きます — これには失敗（ある場合）および合格したテストの詳細な Pester テスト出力が含まれています
3. 失敗したテストには、検証が失敗した原因を説明する詳細なエラー メッセージが表示されます

### テストを手動で実行する

テストを手動で再実行する必要がある場合：

1. LocalBox-Client VM で管理者として PowerShell を開きます
2. 実行します：`& "C:\LocalBox\Tests\Invoke-Test.ps1"`

テストが実行され、壁紙の表示と Azure リソース タグの両方が現在の結果で更新されます。

### 一般的なテスト失敗とその解決策

- **Azure Arc 接続済みマシン テストが失敗する**: Azure Arc エージェントが適切にインストールされており、VM が Azure に接続できることを確認してください
- **クラスター接続テストが失敗する**: Azure Local インスタンスのデプロイが正常に完了しており、インスタンスが Azure に適切に登録されていることを確認してください。詳細については、LocalBox リソース グループに移動し、「デプロイ」をクリックして「localcluster-validate」および「localcluster-deploy」デプロイのエラーを確認してください。
- **リソース数テストが失敗する**: 期待されるすべての Azure リソースがリソース グループに作成されたことを確認してください

### _LocalBox-Client_ 仮想マシンからのログの確認

デプロイが失敗した場合、_LocalBox-Client_ 仮想マシン上で実行されたスクリプトのログ出力を確認する必要がある場合があります。トラブルシューティングを容易にするために、LocalBox デプロイ スクリプトは _LocalBox-Client_ の _C:\LocalBox\Logs_ フォルダーにすべての関連ログを収集します。ログとその目的の簡単な説明を以下の一覧に示します：

| ログ ファイル                                        | 説明                                                                                                                               |
| --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| _C:\LocalBox\Logs\Bootstrap.log_              | _LocalBox-Client_ で実行される最初のブートストラップ スクリプトの出力。                                                              |
| _C:\LocalBox\Logs\New-LocalBoxCluster.log_    | Hyper-V ホストを構成して Azure Local インスタンス、管理 VM、その他の構成を構築する _New-LocalBoxCluster.ps1_ の出力。 |
| _C:\LocalBox\Logs\Generate-ARM-Template.log_  | _azlocal.json_ および _azlocal.parameters.json_ ファイルを構築するスクリプトのログ出力。                                                        |
| _C:\LocalBox\Logs\LocalBoxLogonScript.log_    | プロビジョニング プロセスを管理するオーケストレーター スクリプトからのログ出力。                          |
| _C:\LocalBox\Logs\DeploymentStatus.log_    | _Invoke-Test.ps1 スクリプト_ からのログ出力 |
| _C:\LocalBox\Logs\Tools.log_                  | ブートストラップ中のツール インストールからのログ出力。                                                                                       |

  ![LocalBox-Client の LocalBox ログ フォルダーを示すスクリーンショット](./troubleshoot_logs.png)

それでも LocalBox のデプロイで問題が発生する場合は、GitHub に[イシューを送信](https://aka.ms/JumpstartIssue)し、問題の詳細な説明とデプロイ先の Azure リージョンを含めてください。_C:\LocalBox\Logs_ フォルダー内には、Jumpstart チームによるレビューのためにログを Azure ストレージ アカウントにアップロードする手順も記載されています。
