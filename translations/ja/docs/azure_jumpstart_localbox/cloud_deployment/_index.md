---
type: docs
linkTitle: "LocalBox に接続する"
isGettingStarted: false
weight: 4
---
# LocalBox に接続する

## デプロイ後の自動化を開始する

Bicep デプロイが完了したら、Azure ポータルを開いてリソース グループ内の初期 LocalBox リソースを確認できます。次のデプロイ フェーズを続行するには、_LocalBox-Client_ VM にリモート接続する必要があります。

  ![リソース グループ内のデプロイ済みリソースを示すスクリーンショット](./deployed_resources.png)

   > **注:** LocalBox のデプロイでは、RDP（3389）および SSH（22）ポートはデフォルトで開放されていません。VM にネットワーク アクセスするには、ポート 3389 を許可するネットワーク セキュリティ グループ（NSG）ルールを作成するか、[Azure Bastion](https://learn.microsoft.com/azure/bastion/bastion-overview) または [Just-in-Time（JIT）](https://learn.microsoft.com/azure/defender-for-cloud/just-in-time-access-usage?tabs=jit-config-asc%2Cjit-request-asc) アクセスを使用する必要があります。

## LocalBox クライアント仮想マシンへの接続

> **注:** Azure Local VM が存在するサブネットは 2 つ目のネスト レイヤーにあるため、それらの VM に接続するには AzLMGMT マシンを経由する必要があります。
> Azure Local VM サブネットは Azure 仮想ネットワークにルーティングできないため、Azure Bastion を使用して Azure Local インスタンス上の仮想マシンに接続することはできません。
>
> Azure Local VM への接続に問題がある場合：
> LocalBox-Client から `mstsc /v:192.168.1.11` を実行して AzLMGMT ネスト VM に接続できます。
> そこから `mstsc /v:192.168.200.x`（x はデプロイした VM の IP アドレス）を実行して Azure Local VM に接続します。

_LocalBox-Client_ VM への接続にはいくつかの方法があり、デプロイ時に指定したパラメーターによって異なります。

- [RDP](#rdp-で直接接続する) - _Arc-App-Client-NSG_ のポート 3389 へのアクセスを構成するか、[Just-in-Time アクセス（JIT）](#just-in-time-アクセスjit-を使用して接続する)を有効にした後に使用できます。
- [Azure Bastion](#azure-bastion-を使用して接続する) - デプロイ時に _`deployBastion`_ パラメーターの値を *`true`* にした場合に利用できます。

### RDP で直接接続する

設計上、LocalBox はネットワーク セキュリティ グループでポート 3389 を開放していません。そのため、インバウンド 3389 を許可する NSG ルールを作成する必要があります。

- Azure ポータルで _LocalBox-NSG_ リソースを開き、「追加」をクリックして新しいルールを追加します。

  ![RDP がブロックされた LocalBox-Client NSG を示すスクリーンショット](./rdp_nsg_blocked.png)

  ![新しいインバウンド セキュリティ ルールの追加を示すスクリーンショット](./nsg_add_rule.png)

- 接続元の IP アドレスを指定し、サービスとして RDP を選択して、アクションを「許可」に設定します。パブリック IP アドレスは [https://icanhazip.com](https://icanhazip.com) または [https://whatismyip.com](https://whatismyip.com) で確認できます。

  <img src="./nsg_add_rdp_rule.png" alt="RDP インバウンド許可ルール追加を示すスクリーンショット" width="400">

  ![すべてのインバウンド セキュリティ ルールを示すスクリーンショット](./rdp_nsg_all_rules.png)

  ![RDP を使用した VM への接続を示すスクリーンショット](./rdp_connect.png)

### Azure Bastion を使用して接続する

- デプロイで Azure Bastion のデプロイを選択した場合は、それを使用して VM に接続します。

  ![Bastion を使用した VM への接続を示すスクリーンショット](./bastion_connect.png)

  > **注:** Azure Bastion を使用する場合、デスクトップの背景画像は表示されません。そのため、Azure Bastion で _LocalBox-Client_ に接続している場合、このガイドの一部のスクリーンショットと表示が異なる場合があります。

### Just-in-Time アクセス（JIT）を使用して接続する

サブスクリプションで [Microsoft Defender for Cloud](https://learn.microsoft.com/azure/defender-for-cloud/just-in-time-access-usage?tabs=jit-config-asc%2Cjit-request-asc) を有効にしていて、JIT を使用してクライアント VM にアクセスする場合は、次の手順に従います。

- クライアント VM の構成ペインで Just-in-time を有効にします。これにより既定の設定が有効になります。

  ![Microsoft Defender for Cloud ポータルでクライアント VM の RDP を許可するスクリーンショット](./jit_allowing_rdp.png)

  ![RDP を使用した VM への接続を示すスクリーンショット](./rdp_connect.png)

  ![JIT を使用した VM への接続を示すスクリーンショット](./jit_rdp_connect.png)

### ログオン スクリプト

- _LocalBox-Client_ VM にログインすると、PowerShell スクリプトが開いて実行が開始されます。このスクリプトの完了には**約 4 ～ 5 時間**かかり、完了するとスクリプト ウィンドウは自動的に閉じます。Azure Local の更新プログラムが利用可能な場合は、推定デプロイ時間にさらに 1 時間追加する必要があります。この時点でインフラストラクチャのデプロイが完了します。

  ![_LocalBox-Client_ を示すスクリーンショット](./automation.png)

- Azure ポータルで、両方の Azure Local マシン（AzLHOST1 と AzLHOST2）が Arc 対応サーバーとして作成されていることを確認します。

## Azure ポータルから Azure Local インスタンスの検証とデプロイを行う

デプロイ時に `autoDeployClusterResource` パラメーターを `false` に設定した場合は、[手動デプロイ](../manual_deployment/)ガイドを参照して Azure Local インスタンスを手動で検証およびデプロイしてください。このパラメーターをオーバーライドしていない場合は、以下の「[デプロイ完了](#デプロイ完了)」セクションに進んでください。

デプロイの問題については、必要に応じて [トラブルシューティング](../troubleshooting/) を参照してください。

## デプロイ完了

LocalBox リソース グループでクラスター リソース `localboxcluster` を開き、`設定` -> `デプロイ` に移動して、すべての手順が正常に完了していることを確認します。

  ![インスタンスのデプロイ進行状況を示すスクリーンショット](./cluster_deployment_complete.png)

LocalBox インスタンスのデプロイが完了したら、さまざまな LocalBox の機能を探索し始めましょう。次の手順については [LocalBox の使用](../using_localbox/) ガイドに進んでください。

  ![デプロイ済みインスタンスのスクリーンショット](./cluster_detail.png)
