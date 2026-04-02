---
type: docs
linkTitle: "VM 管理"
weight: 6
---
# LocalBox での Azure Arc による仮想マシン プロビジョニング

Azure Local は [Azure ポータルでの VM プロビジョニング](https://learn.microsoft.com/azure/azure-local/manage/manage-arc-virtual-machines)をサポートしています。すべての Azure Local インスタンスと同様に、LocalBox インスタンスには Azure ポータルを通じた VM 管理に必要なコンポーネントが事前に構成されています。このガイドに従って、Marketplace イメージから基本的な VM を構成してください。

## Azure Marketplace から仮想マシン イメージを作成する

Azure ポータルから Azure Local インスタンスに仮想マシンを作成する前に、ベースとして使用できる VM イメージを作成する必要があります。これらのイメージは Azure Marketplace からインポートするか、ユーザーが直接提供できます。このユースケースでは Azure Marketplace からイメージを作成します。

- LocalBox リソース グループ内のインスタンス リソースに移動してクリックします。

  ![インスタンス リソースを示すスクリーンショット](./az_local_cluster_rg.png)

- メニューの「VM イメージ」をクリックし、「VM イメージの追加」ドロップダウンをクリックして「Azure Marketplace から」を選択します。

  ![VM の作成を示すスクリーンショット](./add_image_from_marketplace.png)

- イメージの一覧からイメージを選択します。VM イメージに名前を付け、ドロップダウンからデフォルトのカスタム ロケーションを選択し、ストレージ パスは「自動的に選択」のままにします。準備ができたら「確認と作成」をクリックします。

  ![VM イメージ詳細の作成を示すスクリーンショット](./create_vm_detail_win11.png)

- Azure Marketplace からインスタンスへの VM イメージのダウンロードには時間がかかります。リソース グループの VM イメージ リソースにアクセスしてリソースのプロパティを確認することで進行状況を監視できます。

  ![VM イメージのプロパティのスクリーンショット](./monitor_vm_image_progress.png)

- ダウンロードが完了するまで必要に応じてイメージを監視します。待っている間に、インスタンスの論理ネットワークを作成する次のセクションに進みます。

## Azure Local インスタンスに論理ネットワークを作成する

LocalBox のネットワークには VLAN200 にタグ付けされた 192.168.200.0/24 サブネットが含まれています。このネットワークは LocalBox 上の Arc 対応 VM での使用を目的としています。この事前に構成されたネットワークを使用するには、このサブネットにマップする論理ネットワーク リソースを作成する必要があります。

  | ネットワーク詳細 | 値 |
  | ---------- | --------------------- |
  | サブネット   | 192.168.200.0/24      |
  | ゲートウェイ  | 192.168.200.1         |
  | VLAN ID   | 200                   |
  | DNS サーバー | 192.168.1.254         |

- _LocalBox-Client_ VM 内部からエクスプローラーを開き、_C:\LocalBox_ に移動します。_Configure-VMLogicalNetwork.ps1_ PowerShell ファイルを右クリックして「PowerShell で実行」を選択します。必要に応じて VSCode でファイルを確認することもできます。

- _LocalBox-Client_ 仮想マシンでエクスプローラーを開き、_C:\LocalBox_ フォルダーに移動します。「Configure-VMLogicalNetwork.ps1」を右クリックし、「Visual Studio Code で開く」を選択します。

- 「実行」ボタンをクリックします：

  ![Configure-VMLogicalNetwork.ps1 ファイルの実行方法を示すスクリーンショット](./run_with_powershell.png)

- スクリプトが完了したら、リソース グループを確認して新しく作成された論理ネットワーク リソースを確認できます。

  ![Azure ポータルの論理ネットワークを示すスクリーンショット](./logical_network.png)

## 仮想マシンを作成する

- VM イメージ リソースを開いて、VM イメージのダウンロードが完了していることを確認します。

  ![VM イメージ完了を示すスクリーンショット](./monitor_vm_image_available.png)

- Azure Local インスタンス リソースを開いて「仮想マシン」ブレードを開き、「仮想マシンの作成」ボタンをクリックします。

  ![VM 作成の概要を示すスクリーンショット](./create_vm.png)

- LocalBox リソース グループを選択し、VM に名前を付け、セキュリティの種類に「Standard」を選択し、先ほど作成した VM イメージをイメージとして選択します。プロセッサ数を 2、メモリを 8192 に設定します。「次へ」をクリックし、もう一度「次へ」をクリックしてネットワーク タブに進みます。

  ![VM 詳細の作成を示すスクリーンショット](./create_vm_detail_win11.png)

- 「ネットワーク インターフェイスの追加」をクリックし、インターフェイスに名前を付けてドロップダウンから先ほど作成したネットワークを選択します。割り当て方法は「自動」のままにします。ネットワーク カードを追加してから「次へ」をクリックします。

  ![VM ネットワーク カードの作成を示すスクリーンショット](./create_vm_detail_vnic.png)

  ![VM ネットワーク カードの作成を示すスクリーンショット](./create_vm_detail_add_vnic.png)

- 仮想マシンの詳細を確認し、準備ができたら「確認と作成」をクリックします。

  ![VM 作成の最終ステップを示すスクリーンショット](./vm_image_review_create.png)

- Azure Local VM リソースを開いて Arc への接続とその他の詳細を確認します。

  ![VM リソースを示すスクリーンショット](./vm_resource_detail.png)

## 次のステップ

追加情報については、[Azure Local VM 管理](https://learn.microsoft.com/azure/azure-local/manage/azure-arc-vm-management-overview?view=azloc-2504#what-is-azure-arc-resource-bridge)のドキュメントを確認してください。
