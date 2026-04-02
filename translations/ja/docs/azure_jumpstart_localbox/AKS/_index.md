---
type: docs
linkTitle: "Azure Local 上の AKS"
weight: 7
---

# Azure Local LocalBox 上の Azure Kubernetes Service

Azure Local は [AKS enabled by Azure Arc](https://learn.microsoft.com/azure/aks/aksarc/aks-overview) のホスト インフラストラクチャを提供できます。

## Azure Local 上の AKS を探索する

LocalBox には AKS デプロイ専用のネットワーク サブネットが事前に構成されています。サブネットの詳細は以下のとおりです：

  | ネットワーク詳細 | 値 |
  | ---------- | --------------------- |
  | サブネット   | 10.10.0.0/24          |
  | ゲートウェイ  | 10.10.0.1             |
  | VLAN ID   | 110                   |
  | DNS サーバー | 192.168.1.254         |

AKS ワークロード クラスターを作成する前に、ローカル仮想ネットワーク オブジェクトを作成する必要があります。LocalBox には、事前に構成されたネットワークを使用してこのタスクを完了するスクリプトが含まれています。スクリプトは新しい AKS ワークロード クラスターを作成します。

- LocalBox リソース グループを開いて _jumpstart_ カスタム ロケーション リソースをクリックし、「Arc 対応サービス」をクリックします。ここでは hybridaksextension サービスがクラスターで利用可能であることが表示されます。これは Azure Local インスタンスにデフォルトでインストールされており、Azure Local 上で AKS ワークロード クラスターを作成するために必要です。

  ![AKS 拡張機能を示すスクリーンショット](./custom_location_resources.png)

AKS クラスターへのアクセスは [Azure RBAC を通じて管理](https://learn.microsoft.com/azure/aks/hybrid/aks-create-clusters-cli#before-you-begin)されます。この演習のために、メンバーである新しい Microsoft Entra グループを作成するか、既存のグループを使用します。

- [Microsoft Entra グループを作成](https://learn.microsoft.com/entra/fundamentals/how-to-manage-groups)するか、既存のグループを使用します。

- Microsoft Entra グループのオブジェクト ID を取得して、次の手順で使用するためにメモしておきます。

  ![Entra グループ ID を示すスクリーンショット](./entra_group_id.png)

- _LocalBox-Client_ 仮想マシンでエクスプローラーを開き、_C:\LocalBox_ フォルダーに移動します。「Configure-AksWorkloadCluster.ps1」を右クリックし、「Visual Studio Code で開く」を選択します。

  ![VSCode での編集を示すスクリーンショット](./open_with_code.png)

- スクリプトの 6 行目のコメントを解除し、_$aadgroupID_ パラメーターのプレースホルダーの値を Microsoft Entra グループのオブジェクト ID に編集します。スクリプトを保存します（Ctrl + S）。

  ![スクリプトの編集を示すスクリーンショット](./edit_script.png)

- スクリプトを保存したら、「実行」ボタンをクリックします。

  ![スクリプト実行を示すスクリーンショット](./run_with_powershell.png)

- スクリプトが完了するまで待ち、「currentState」プロパティの「Succeeded」値を確認します：

  ![スクリプト実行を示すスクリーンショット](./run_configure_aks.png)

- 完了すると、LocalBox リソース グループに _localbox-aks_ という名前の AKS ワークロード クラスターが作成されます。

  ![リソース グループ内の AKS を示すスクリーンショット](./aks_in_resource_group.png)

- AKS クラスターをクリックして、Kubernetes バージョンなどの詳細を確認します。クラスターが Azure に完全に接続されるまで「ステータス」が接続中と表示される場合があります。

  ![クラスターの詳細を示すスクリーンショット](./cluster_detail.png)

- _LocalBox-Client_ 仮想マシンの Visual Studio Code ターミナル ウィンドウから以下を実行して、以前に構成した Entra ID グループのメンバーであるユーザーで認証します：

  ```powershell
  az logout
  az login --use-device-code --tenant $env:tenantId
  ```

![Azure CLI ログインを示すスクリーンショット](./az_login.png)

コンプライアント デバイスからのサインインを強制する条件付きアクセス ポリシーがある場合は、ローカル マシンのブラウザーでデバイス認証を行うことができます。

- 前の手順と同じターミナル ウィンドウから以下を実行して AKS クラスターへのプロキシ接続を設定し、2 番目のターミナル セッションを開くためにハイライトされた +-ボタンをクリックします：

  ```powershell
  az connectedk8s proxy -n localbox-aks -g $env:resourceGroup
  ```

  ![connectedk8s の実行を示すスクリーンショット](./kubectl_proxy.png)

- 新しいターミナルから、クラスターへの kubectl アクセスが可能です。kubectl コマンドをいくつか実行してみてください。

  ![kubectl プロキシ アクセスを示すスクリーンショット](./kubectl.png)

- LocalBox クライアント VM に事前インストールされている Visual Studio Code の Kubernetes 拡張機能など、他の Kubernetes ツールを使用することもできます：

  ![kubectl プロキシ アクセスを示すスクリーンショット](./k8s_vs_code.png)


## 次のステップ

Azure Local 上の Azure Kubernetes Service には、ここでは直接取り上げていない多くの機能があります。[AKS enabled by Azure Arc](https://learn.microsoft.com/azure/aks/aksarc/aks-overview) で旅を続けるためにドキュメントを確認してください。
