---
title: Azure Backup のレポートを構成する
description: Recovery Services コンテナーを使用して Azure Backup の Power BI レポートを構成します。
services: backup
author: adigan
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: adigan
ms.openlocfilehash: e3004a44958d75d18d608a2fbed7ccc44a00dc93
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "60642680"
---
# <a name="configure-azure-backup-reports"></a>Azure Backup のレポートを構成する
この記事では、Recovery Services コンテナーを使用して Azure Backup のレポートを構成するために実行する手順を示します。 また、Power BI を使用してレポートにアクセスする方法についても説明します。 これらの手順を完了したら、Power BI に直接移動してレポートを表示、カスタマイズ、および作成できます。

> [!IMPORTANT]
> 2018 年 11 月 1 日以降、Power BI で Azure Backup アプリのデータを読み込んでいるときに問題が発生する場合があり、"JSON 入力の末尾に余分な文字が見つかり、 IDataReader インターフェイスによって例外が発生した" ことが通知される可能性があります。
これは、データがストレージ アカウントに読み込まれるときの形式の変更によるものです。
この問題を回避するには、最新のアプリ (バージョン 1.8) をダウンロードしてください。
>
>

## <a name="supported-scenarios"></a>サポートされるシナリオ
- Azure Backup レポートは、Azure Recovery Services エージェントを使用したクラウドへの Azure 仮想マシンのバックアップやファイルおよびフォルダーのバックアップに対してサポートされます。
- この時点では、Azure SQL Database、Azure File Shares、Data Protection Manager、および Azure Backup サーバーのレポートはサポートされていません。
- 各コンテナーに対して同じストレージ アカウントが構成されている場合は、コンテナーおよびサブスクリプションにまたがるレポートを表示できます。 選択されるストレージ アカウントは、Recovery Services コンテナーと同じリージョンに存在する必要があります。
- Power BI での、レポートのスケジュールされた更新の頻度は 24 時間ごとです。 また、Power BI でレポートのアドホック更新を実行することもできます。 この場合は、顧客のストレージ アカウント内の最新のデータを使用してレポートが描画されます。

## <a name="prerequisites"></a>前提条件
- レポート用に構成する [Azure ストレージ アカウントを作成](../storage/common/storage-quickstart-create-account.md)します。 このストレージ アカウントは、レポート関連のデータを格納するために使用されます。
- Power BI ポータルを使用して独自のレポートを表示、カスタマイズ、および作成するための [Power BI アカウントを作成します](https://powerbi.microsoft.com/landing/signin/)。
- まだ登録されていない場合は、リソース プロバイダー **Microsoft.insights** を登録します。 レポート データがストレージ アカウントに流入できるように、ストレージ アカウントと Recovery Services コンテナー用のサブスクリプションを使用します。 この手順を実行するには、Azure Portal に移動して **[サブスクリプション]**  >  **[リソース プロバイダー]** を選択し、このプロバイダーを確認して登録します。

## <a name="configure-storage-account-for-reports"></a>レポート用のストレージ アカウントを構成する
次の手順に従って、Azure Portal を使用して Recovery Services コンテナー用のストレージ アカウントを構成します。 これは 1 回限りの構成です。 ストレージ アカウントが構成されたら、Power BI に直接移動してコンテンツ パックを表示したり、レポートを使用したりできます。

1. Recovery Services コンテナーが既に開かれている場合は、次の手順に進みます。 Recovery Services コンテナーが開かれていない場合は、Azure Portal で **[すべてのサービス]** を選択します。

   * リソースの一覧で、「**Recovery Services**」と入力します。
   * 入力を始めると、入力内容に基づいて、一覧がフィルター処理されます。 **[Recovery Services コンテナー]** が表示されたら、それを選択します。
   * Recovery Services コンテナーの一覧が表示されます。 Recovery Services コンテナーの一覧で、コンテナーを選択します。

     選択したコンテナーのダッシュボードが開きます。
2. コンテナーの下に表示される項目の一覧から、 **[監視およびレポート]** セクションの下の **[バックアップ レポート]** を選択して、レポート用のストレージ アカウントを構成します。

      ![[バックアップ レポート] メニュー項目を選択する (手順 2)](./media/backup-azure-configure-reports/backup-reports-settings.PNG)
3. **[バックアップ レポート]** ブレードで、 **[診断設定]** リンクを選択します。 このリンクにより、顧客のストレージ アカウントにデータをプッシュするために使用される **[診断設定]** UI が開かれます。

      ![診断を有効化する (手順 3)](./media/backup-azure-configure-reports/backup-azure-configure-reports.png)
4. **[診断を有効にする]** を選択して、ストレージ アカウントを構成するために使用する UI を開きます。

      ![診断を有効にする (手順 4)](./media/backup-azure-configure-reports/enable-diagnostics.png)
5. **[名前]** ボックスに、設定名を入力します。 レポート データがストレージ アカウントへの流入を開始できるように、 **[ストレージ アカウントへのアーカイブ]** チェック ボックスをオンにします。

      ![診断を有効化する (手順 5)](./media/backup-azure-configure-reports/select-setting-name.png)
6. **[ストレージ アカウント]** を選択し、レポート データを格納するための一覧から関連するサブスクリプションとストレージ アカウントを選択して、 **[OK]** を選択します。

      ![ストレージ アカウントを選択する (手順 6)](./media/backup-azure-configure-reports/select-subscription-sa.png)
7. **[ログ]** セクションの下の **[AzureBackupReport]** チェック ボックスをオンにします。 スライダーを動かして、このレポート データの保有期間を選択します。 ストレージ アカウント内のレポート データは、このスライダーを使用して選択された期間だけ保持されます。

      ![ストレージ アカウントを保存する (手順 7)](./media/backup-azure-configure-reports/save-diagnostic-settings.png)
8. すべての変更を確認し、 **[保存]** を選択します。 このアクションにより、すべての変更が確実に保存され、ストレージ アカウントはレポート データを格納するように構成されました。

9. **[診断設定]** の表には、このコンテナーに対して有効になった新しい設定が表示されるようになります。 表示されない場合は、この表を更新して、更新された設定が表示されるようにします。

      ![診断設定を表示する (手順 9)](./media/backup-azure-configure-reports/diagnostic-setting-row.png)

> [!NOTE]
> ストレージ アカウントを保存することによってレポートを構成した後、初期のデータ プッシュが完了するまで*24 時間待ってください*。 その後でのみ、Power BI で Azure Backup アプリをインポートします。 詳細については、[FAQ のセクション](#frequently-asked-questions)を参照してください。
>
>

## <a name="view-reports-in-power-bi"></a>Power BI でレポートを表示する
Recovery Services コンテナーを使用してレポート用のストレージ アカウントを構成した後、レポート データが流入を開始するには約 24 時間かかります。 ストレージ アカウントを設定してから 24 時間経った後、次の手順に従って Power BI でレポートを表示します。
レポートをカスタマイズして共有したい場合は、ワークスペースを作成して次の手順を実行します。

1. Power BI に[サインイン](https://powerbi.microsoft.com/landing/signin/)します。
2. **[データの取得]** を選択します。 **[独自のコンテンツを作成する他の方法]** で、 **[サービス コンテンツ パック]** を選択します。 [サービスに接続するための Power BI のドキュメント](https://powerbi.microsoft.com/documentation/powerbi-content-packs-services/)にある手順に従います。

3. **[検索]** バーに「**Azure Backup**」と入力し、 **[今すぐ入手する]** を選択します。

      ![コンテンツパックを入手する](./media/backup-azure-configure-reports/content-pack-get.png)
4. 前の手順 5. で構成されたストレージ アカウントの名前を入力し、 **[次へ]** を選択します。

    ![ストレージ アカウント名を入力する](./media/backup-azure-configure-reports/content-pack-storage-account-name.png)    
5. [Authentication method]\(認証方法) で [Key]\(キー) を使用し、このストレージ アカウントのストレージ アカウント キーを入力します。 [ストレージ アクセス キーを表示してコピーする](../storage/common/storage-account-manage.md#access-keys)には、Azure Portal でストレージ アカウントに移動します。

     ![ストレージ アカウントを入力する](./media/backup-azure-configure-reports/content-pack-storage-account-key.png) <br/>

6. **[サインイン]** をクリックします。 サインインが成功すると、データのインポート中の通知が表示されます。

    ![コンテンツ パックのインポート](./media/backup-azure-configure-reports/content-pack-importing-data.png) <br/>

    インポートが完了すると、 **[成功]** の通知が表示されます。 ストレージ アカウント内のデータ量が多い場合は、コンテンツ パックのインポートに少し時間かかることがあります。

    ![コンテンツ パックのインポート成功](./media/backup-azure-configure-reports/content-pack-import-success.png) <br/>

7. データが正常にインポートされると、ナビゲーション ウィンドウの **[アプリ]** に **Azure Backup** コンテンツ パックが表示されます。 **[ダッシュ ボード]** 、 **[Reports]** \(レポート)、および **[データセット]** の下に Azure Backup の一覧が表示されます。

8. **[ダッシュボード]** の下の **[Azure Backup]** を選択します。これにより、一連のピン留めされた主要なレポートが表示されます。

      ![Azure Backup のダッシュボード](./media/backup-azure-configure-reports/azure-backup-dashboard.png) <br/>
9. レポートの完全なセットを表示するには、ダッシュボードでいずれかのレポートを選択します。

      ![Azure Backup のジョブの正常性](./media/backup-azure-configure-reports/azure-backup-job-health.png) <br/>
10. レポートの各タブを選択すると、その領域内のレポートが表示されます。

      ![Azure Backup レポートのタブ](./media/backup-azure-configure-reports/reports-tab-view.png)


## <a name="frequently-asked-questions"></a>よく寄せられる質問

### <a name="how-do-i-check-if-reporting-data-has-started-flowing-into-a-storage-account"></a>レポート データがストレージ アカウントへの流入を開始したかどうかはどのように確認できますか?
構成したストレージ アカウントに移動し、コンテナーを選択します。 コンテナーに insights-logs-azurebackupreport のエントリーがある場合は、レポート データのフローが既に開始されています。

### <a name="what-is-the-frequency-of-data-push-to-a-storage-account-and-the-azure-backup-content-pack-in-power-bi"></a>Power BI でのストレージ アカウントと Azure Backup コンテンツ パックへのデータ プッシュの頻度はどれくらいですか?
  初日のユーザーの場合は、ストレージ アカウントにデータをプッシュするまでに約 24 時間かかります。 この初期のプッシュが完了した後、データは次の図に示されている頻度で更新されます。

  * **ジョブ**、**アラート**、**バックアップ項目**、**コンテナー**、**保護されているサーバー**、および**ポリシー**に関連したデータは、それがログに記録された時点で顧客のストレージ アカウントにプッシュされます。

  * **ストレージ**に関連したデータは、24 時間ごとに顧客のストレージ アカウントにプッシュされます。

       ![Azure Backup のレポートのデータ プッシュ頻度](./media/backup-azure-configure-reports/reports-data-refresh-cycle.png)

  * Power BI では、[1 日 1 回のスケジュールされた更新](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/#what-can-be-refreshed)が行われます。 Power BI でコンテンツ パックのデータを手動で更新することもできます。

### <a name="how-long-can-i-retain-reports"></a>レポートはどれだけの期間保持できますか?
ストレージ アカウントを構成するとき、ストレージ アカウント内のレポート データの保有期間を選択できます。 「[レポート用のストレージ アカウントを構成する](backup-azure-configure-reports.md#configure-storage-account-for-reports)」セクションの手順 6. に従います。 また、[Excel でレポートを分析し](https://powerbi.microsoft.com/documentation/powerbi-service-analyze-in-excel/)、必要に応じてより長い保有期間にわたってレポートを保存することもできます。

### <a name="will-i-see-all-my-data-in-reports-after-i-configure-the-storage-account"></a>ストレージ アカウントを構成した後は、すべてのデータがレポートに表示されますか?
 ストレージ アカウントを構成した後に生成されたすべてのデータがストレージ アカウントにプッシュされ、レポートで使用できます。 進行中のジョブはレポートにプッシュされません。 そのジョブは、完了するか、または失敗した後にレポートに送信されます。

### <a name="if-i-already-configured-the-storage-account-to-view-reports-can-i-change-the-configuration-to-use-another-storage-account"></a>レポートを表示するためのストレージ アカウントを既に構成している場合は、別のストレージ アカウントを使用するように構成を変更できますか?
はい、別のストレージ アカウントをポイントするように構成を変更できます。 Azure Backup コンテンツ パックに接続するときに、新しく構成されたストレージ アカウントを使用します。 また、別のストレージ アカウントが構成された後、新しいデータはこのストレージ アカウントに流入します。 以前の (構成を変更する前の) データは、引き続き前のストレージ アカウント内に残ります。

### <a name="can-i-view-reports-across-vaults-and-subscriptions"></a>コンテナーおよびサブスクリプションにまたがるレポートを表示できますか?
はい、複数のコンテナーに対して同じストレージ アカウントを構成して、複数のコンテナーにわたるレポートを表示できます。 また、複数のサブスクリプションの複数のコンテナーに対して同じストレージ アカウントを構成することもできます。 その後、Power BI で Azure Backup コンテンツ パックに接続するときに、このストレージ アカウントを使用してレポートを表示できます。 選択されるストレージ アカウントは、Recovery Services コンテナーと同じリージョンに存在する必要があります。

## <a name="troubleshooting-errors"></a>エラーのトラブルシューティング
| Error Details | 解決策 |
| --- | --- |
| バックアップ レポート用のストレージ アカウントを設定した後も、 **[ストレージ アカウント]** が引き続き **[構成されていません]** と表示される。 | ストレージ アカウントを正常に構成している場合は、この問題に関係なく、レポート データは流入します。 この問題を解決するには、Azure Portal に移動し、 **[すべてのサービス]**  >  **[診断設定]**  >  **[Recovery Services コンテナー]**  >  **[設定の編集]** を選択します。 以前に構成された設定を削除し、同じブレードに新しい設定を作成します。 今回は、 **[名前]** ボックスで **[サービス]** を選択します。 これで、構成されたストレージ アカウントが表示されます。 |
|Power BI で Azure Backup コンテンツ パックをインポートした後、"404- コンテナーが見つかりません" というエラー メッセージが表示される。 | 先に説明したように、Recovery Services コンテナーでレポートを構成した後、それらのレポートが Power BI で正しく表示されるまでに 24 時間待つ必要があります。 24 時間の前にこれらのレポートにアクセスしようとすると、有効なレポートを表示するための完全なデータがまだ存在しないため、このエラー メッセージが表示されます。 |

## <a name="next-steps"></a>次の手順
ストレージ アカウントを構成し、Azure Backup コンテンツ パックをインポートしたら、次の手順ではレポートをカスタマイズしたり、レポート データ モデルを使用してレポートを作成したりします。 詳細については、次の記事を参照してください。

* [Azure Backup レポート データ モデルを使用する](backup-azure-reports-data-model.md)
* [Power BI でレポートをフィルター処理する](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
* [Power BI でレポートを作成する](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)
