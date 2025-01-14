---
title: FTP サーバーへの接続 - Azure Logic Apps
description: Azure Logic Apps を使用して FTP サーバー上のファイルを作成、監視、管理する
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: divswa, LADocs
ms.topic: article
ms.date: 10/15/2018
tags: connectors
ms.openlocfilehash: e5aeaa707c7a839483484c524e982204d6fe055c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "60408557"
---
# <a name="create-monitor-and-manage-ftp-files-by-using-azure-logic-apps"></a>Azure Logic Apps を使用して FTP ファイルを作成、監視、および管理する

Azure Logic Apps と FTP コネクタを使用すると、FTP サーバー上のアカウント経由でファイルを作成、監視、送信、および受信する自動化されたタスクやワークフローを作成できます。その他、次のようなアクションを実行できます。

* ファイルの追加または変更を監視します。
* ファイルの取得、作成、コピー、更新、一覧、および削除を行います。
* ファイルの内容とメタデータを取得します。
* アーカイブをフォルダーに抽出します。

FTP サーバーからの応答を取得するトリガーを使用し、その出力を他のアクションから使用可能にすることができます。 ロジック アプリから実行アクションを使用して、FTP サーバー上のファイルを管理できます。 また、FTP アクションからの出力を他のアクションで使用するようにもできます。 たとえば、FTP サーバーから定期的にファイルを取得する場合は、Office 365 Outlook コネクタまたは Outlook.com コネクタを使用して、これらのファイルとその内容に関する電子メールを送信できます。 ロジック アプリを初めて使用する場合は、「[Azure Logic Apps とは](../logic-apps/logic-apps-overview.md)」を参照してください。

## <a name="limits"></a>制限

* FTP アクションでサポートできるファイルは、[メッセージのチャンク](../logic-apps/logic-apps-handle-large-messages.md)を使用しない限り、"*50 MB 以下*" に限られます (メッセージのチャンクを使用すると、この制限を回避できます)。 現在、FTP トリガーではチャンクはサポートされていません。

* FTP コネクタは明示的 FTP over SSL (FTPS) のみをサポートし、暗黙的 FTPS とは互換性がありません。

## <a name="prerequisites"></a>前提条件

* Azure サブスクリプション。 Azure サブスクリプションがない場合は、<a href="https://azure.microsoft.com/free/" target="_blank">無料の Azure アカウントにサインアップ</a>してください。 

* FTP ホスト サーバー アドレスとアカウントの資格情報

  FTP コネクタでは、FTP サーバーがインターネットからアクセス可能であり、かつ*パッシブ* モードで動作するように設定されている必要があります。 資格情報があることで、ロジック アプリは接続を作成し、FTP アカウントにアクセスすることができます。

* [ロジック アプリの作成方法](../logic-apps/quickstart-create-first-logic-app-workflow.md)に関する基本的な知識

* FTP アカウントにアクセスするためのロジック アプリ。 FTP トリガーから開始するには、[空のロジック アプリを作成します](../logic-apps/quickstart-create-first-logic-app-workflow.md)。 FTP アクションを使用するには、 **[繰り返し]** トリガーなどの別のトリガーでロジック アプリを起動します。

## <a name="connect-to-ftp"></a>FTP に接続する

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure portal](https://portal.azure.com) にサインインし、ロジック アプリ デザイナーでロジック アプリを開きます (まだ開いていない場合)。

1. 空のロジック アプリの場合は、検索ボックスに、フィルターとして「ftp」と入力します。 トリガーの一覧で、目的のトリガーを選択します。

   または

   既存のロジック アプリの場合は、アクションを追加する最後のステップの下で **[新しいステップ]** を選択してから、 **[アクションを追加する]** を選択します。 
   検索ボックスに、フィルターとして「ftp」と入力します。 
   アクションの一覧で、目的のアクションを選択します。

   ステップの間にアクションを追加するには、ステップ間の矢印の上にポインターを移動します。 
   表示されるプラス記号 ( **+** ) を選択し、 **[アクションの追加]**  を選択します。

1. 接続で必要な詳細を指定し、 **[作成]** を選択します。

1. 選択したトリガーまたはアクションのために必要な詳細を指定し、ロジック アプリのワークフローの構築を続けます。

ファイルの内容を要求するとき、トリガーでは 50 MB より大きいファイルは取得されません。 50 MB より大きいファイルを取得するには、次のパターンに従います。

* **ファイルが追加または変更されたとき (プロパティのみ)** といった、ファイルのプロパティを返すトリガーを使用します。

* **パスを使用してファイルの内容を取得する**といった、完全なファイルを読み取るアクションを使用するトリガーに従い、アクションで[メッセージ チャンク](../logic-apps/logic-apps-handle-large-messages.md)を使用します。

## <a name="examples"></a>例

<a name="file-added-modified"></a>

### <a name="ftp-trigger-when-a-file-is-added-or-modified"></a>FTP トリガー: When a file is added or modified (ファイルの追加または変更時)

このトリガーは、FTP サーバー上でファイルがいつ追加または変更されたかを検出すると、ロジック アプリ ワークフローを開始します。 そのため、たとえばファイルの内容をチェックし、内容が特定の条件を満たしているかどうか基づいてその内容を取得するかどうかを決定する条件を追加できます。 最後に、ファイルの内容を取得するアクションを追加し、その内容を SFTP サーバー上のフォルダーに格納できます。 

**企業での使用例**: このトリガーを使用して、FTP フォルダーに顧客からの注文を表す新しいファイルがあるかどうかを監視できます。 その後、さらに処理を行うために注文の内容を取得し、その注文を注文データベースに格納できるように、 **[ファイルの内容を取得する]** などの FTP アクションを使用できます。

ファイルの内容を要求するとき、トリガーでは 50 MB より大きいファイルは取得されません。 50 MB より大きいファイルを取得するには、次のパターンに従います。 

* **ファイルが追加または変更されたとき (プロパティのみ)** といった、ファイルのプロパティを返すトリガーを使用します。

* **パスを使用してファイルの内容を取得する**といった、完全なファイルを読み取るアクションを使用するトリガーに従い、アクションで[メッセージ チャンク](../logic-apps/logic-apps-handle-large-messages.md)を使用します。

有効でかつ機能するロジック アプリには、1 つのトリガーと少なくとも 1 つのアクションが必要です。 そのため、トリガーを追加したら、必ずアクションを追加するようにしてください。

トリガー **ファイルの追加または変更時**

1. [Azure portal](https://portal.azure.com) にサインインし、ロジック アプリ デザイナーでロジック アプリを開きます (まだ開いていない場合)。

1. 空のロジック アプリの場合は、検索ボックスに、フィルターとして「ftp」と入力します。 トリガーの一覧で、トリガー **[When a filed is added or modified - FTP]\(ファイルの追加または変更時 - FTP\)** を選択します。

   ![FTP トリガーを見つけて選択する](./media/connectors-create-api-ftp/select-ftp-trigger.png)  

1. 接続で必要な詳細を指定し、 **[作成]** を選択します。

   既定では、このコネクタからファイルがテキスト形式で転送されます。 
   エンコードが使用されている状況など、バイナリ形式でファイルを転送する場合は、 **[バイナリ転送]** を選択してください。

   ![FTP サーバーの接続を作成する](./media/connectors-create-api-ftp/create-ftp-connection-trigger.png)  

1. **[フォルダー]** ボックスの横にあるフォルダー アイコンを選択すると、一覧が表示されます。 新しいファイルまたは編集されたファイルを監視するフォルダーを見つけるには、右向き矢印 ( **>** ) を選択し、そのフォルダーを参照して選択します。

   ![監視するフォルダーを見つけて選択する](./media/connectors-create-api-ftp/select-folder.png)  

   選択されたフォルダーが **[フォルダー]** ボックスに表示されます。

   ![選択されたフォルダー](./media/connectors-create-api-ftp/selected-folder.png)  

これで、ロジック アプリにトリガーが設定されたので、ロジック アプリが新しいファイルまたは編集されたファイルを見つけたときに実行するアクションを追加します。 この例の場合は、新しい内容または更新された内容を取得する FTP アクションを追加できます。

<a name="get-content"></a>

### <a name="ftp-action-get-content"></a>FTP アクション: コンテンツを取得する

このアクションは、ファイルが追加または更新されたときに FTP サーバー上のそのファイルの内容を取得します。 たとえば、前の例のトリガーと、ファイルが追加または編集された後にそのファイルの内容を取得するアクションを追加できます。 

ファイルの内容を要求するとき、トリガーでは 50 MB より大きいファイルは取得されません。 50 MB より大きいファイルを取得するには、次のパターンに従います。 

* **ファイルが追加または変更されたとき (プロパティのみ)** といった、ファイルのプロパティを返すトリガーを使用します。

* **パスを使用してファイルの内容を取得する**といった、完全なファイルを読み取るアクションを使用するトリガーに従い、アクションで[メッセージ チャンク](../logic-apps/logic-apps-handle-large-messages.md)を使用します。

次のアクションの例を以下に示します:**コンテンツを取得する**

1. トリガーまたはその他の任意のアクションで、 **[新しいステップ]** を選択します。 

1. 検索ボックスに、フィルターとして「ftp」と入力します。 アクションの一覧で、アクション **[Get file content - FTP]\(ファイルの内容を取得する - FTP\)** を選択します。

   ![FTP アクションを選択する](./media/connectors-create-api-ftp/select-ftp-action.png)  

1. 既に FTP サーバーへの接続とアカウントがある場合は、次の手順に進みます。 それ以外の場合は、その接続に必要な詳細を指定し、 **[作成]** を選択します。 

   ![FTP サーバーの接続を作成する](./media/connectors-create-api-ftp/create-ftp-connection-action.png)

1. **[ファイルの内容を取得する]** アクションが開いたら、 **[ファイル]** ボックスの内部をクリックして動的コンテンツ リストを表示します。 これで、前の手順の出力のプロパティを選択できるようになります。 動的コンテンツ リストから、 **[ファイル コンテンツ]** プロパティを選択します。ここには、追加または更新されたファイルの内容が含まれています。  

   ![ファイルを見つけて選択する](./media/connectors-create-api-ftp/ftp-action-get-file-content.png)

   **[ファイル コンテンツ]** プロパティが **[ファイル]** ボックスに表示されるようになります。

   ![選択された [ファイル コンテンツ] プロパティ](./media/connectors-create-api-ftp/ftp-action-selected-file-content-property.png)

1. ロジック アプリを保存し、 ワークフローをテストするには、ロジック アプリが現在監視しているファイルを FTP フォルダーに追加します。

## <a name="connector-reference"></a>コネクタのレファレンス

コネクタの OpenAPI (以前の Swagger) の説明に記載されているトリガー、アクション、および制限に関する技術的な詳細については、[コネクタのリファレンス ページ](/connectors/ftpconnector/)を参照してください。

## <a name="get-support"></a>サポートを受ける

* 質問がある場合は、[Azure Logic Apps フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps)にアクセスしてください。
* 機能のアイデアについて投稿や投票を行うには、[Logic Apps のユーザー フィードバック サイト](https://aka.ms/logicapps-wish)にアクセスしてください。

## <a name="next-steps"></a>次の手順

* 他の[Logic Apps コネクタ](../connectors/apis-list.md)を確認します。
