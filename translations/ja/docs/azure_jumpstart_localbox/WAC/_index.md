---
type: docs
linkTitle: "Windows Admin Center"
weight: 9
toc_hide: true
---

## Windows Admin Center の操作

Windows Admin Center（プレビュー）は Azure ポータル内から利用でき、LocalBox で使用できます。

## Azure ポータルの Windows Admin Center

Windows Admin Center は Azure ポータルから直接使用できます。LocalBox クラスター用にセットアップするには、次の手順に従います：

- _LocalBox-Cluster_ リソースに移動して、Windows Admin Center ブレードを選択します。

  ![ポータルで WAC を開くスクリーンショット](../../../../../translated_images/ja/azure_jumpstart_localbox/WAC/wac_portal_setup_1.png)

- ポートをデフォルト値のままにして「インストール」をクリックします。インストールが完了するまで数分かかります。

  ![ポータルで WAC をインストールするスクリーンショット](../../../../../translated_images/ja/azure_jumpstart_localbox/WAC/wac_portal_setup_2.png)

- インストールが完了したら、クラスター リソースの Windows Admin Center ブレードから「接続」をクリックします。

  ![ポータルで WAC に接続するスクリーンショット](../../../../../translated_images/ja/azure_jumpstart_localbox/WAC/wac_connect.png)

- Windows Admin Center に接続するには、ユーザーを「Windows Admin Center 管理者ログイン」Azure RBAC ロールに追加する必要があります。

  ![ロールへのユーザー追加 1 を示すスクリーンショット](../../../../../translated_images/ja/azure_jumpstart_localbox/WAC/wac_add_role_assignment_role.png)

  ![ロールへのユーザー追加 2 を示すスクリーンショット](../../../../../translated_images/ja/azure_jumpstart_localbox/WAC/wac_add_role_assignment_member.png)

- 完了したら、Azure ポータルの Windows Admin Center を使用してクラスターに接続できます。

  ![Azure ポータルの Windows Admin Center を示すスクリーンショット](../../../../../translated_images/ja/azure_jumpstart_localbox/WAC/wac_portal.png)

## 次のステップ

Windows Admin Center の追加操作については、[公式ドキュメント](https://learn.microsoft.com/windows-server/manage/windows-admin-center/overview)を確認してください。
