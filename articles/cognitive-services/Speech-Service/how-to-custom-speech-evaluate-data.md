---
title: カスタム音声の正確性を評価する - Speech Services
titlesuffix: Azure Cognitive Services
description: このドキュメントでは、Microsoft の音声テキスト変換モデルまたはカスタム モデルの品質を定量的に測定する方法を説明します。 正確性をテストするには、オーディオ + ヒューマン ラベル付け文字起こしデータが必要であり、30 分から 5 時間の典型的な音声を用意する必要があります。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 7c1b3602b09c1a129923180c4b1d4a5f8293de2c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/13/2019
ms.locfileid: "65025695"
---
# <a name="evaluate-custom-speech-accuracy"></a>カスタム音声の正確性を評価する

このドキュメントでは、Microsoft の音声テキスト変換モデルまたはカスタム モデルの品質を定量的に測定する方法を説明します。 正確性をテストするには、オーディオ + ヒューマン ラベル付け文字起こしデータが必要であり、30 分から 5 時間の典型的な音声を用意する必要があります。

## <a name="what-is-word-error-rate-wer"></a>ワード エラー率 (WER) とは

モデルの正確性を測定するための業界標準は、"*ワード エラー率*" (WER) です。 WER では、認識中に識別された誤った単語の数を数え、ヒューマン ラベル付けトランスクリプトで提供された単語の総数で割ります。 最後に、その数に 100% を掛けて WER を計算します。

![WER の数式](./media/custom-speech/custom-speech-wer-formula.png)

誤りと識別された単語は、次の 3 つのカテゴリに分類されます。

* 挿入 (I): 仮説トランスクリプトに誤って追加された単語
* 削除 (D): 仮説トランスクリプトで検出されなかった単語
* 置換 (S): 参照と仮説の間で置き換えられた単語

次に例を示します。

![誤って識別された単語の例](./media/custom-speech/custom-speech-dis-words.png)

## <a name="resolve-errors-and-improve-wer"></a>エラーの解決と WER の向上

アプリ、ツール、または製品で使用しているモデルの品質を評価するために、機械認識結果の WER を使用できます。 WER が 5% から 10% であれば、良質であると見なして、使用できます。 WER が 20% であれば許容範囲内ですが、追加のトレーニングが必要な場合もあるでしょう。 WER が 30% 以上であれば、低品質であることを示しており、カスタマイズとトレーニングが必要です。

エラーがどのような分布になっているかが重要です。 多くの削除エラーが発生する場合は、通常、音声信号の強度が弱いことが原因です。 この問題を解決するには、ソースに近い場所で音声データを収集する必要があります。 挿入エラーは、音声が雑音の多い環境で録音され、クロストークが存在するために認識の問題が発生している可能性があることを意味します。 置換エラーは、ドメイン固有の用語のサンプルが、ヒューマン ラベル付け文字起こしまたは関連テキストとして十分に提供されていない場合にしばしば発生します。

個々のファイルを分析することで、どの種類のエラーが存在し、どのエラーが特定のファイルに固有であるかを判断できます。 ファイル レベルで問題を理解すると、改善の対象の特定に役立ちます。

## <a name="create-a-test"></a>テストの作成

Microsoft の音声テキスト変換ベースライン モデルまたはトレーニングしたカスタム モデルの品質をテストする場合は、2 つのモデルを並べて比較し、正確性を評価できます。 比較には、WER と認識結果が含まれます。 通常、カスタム モデルは、Microsoft のベースライン モデルと比較されます。

モデルを並べて評価するには、次のように操作します。

1. **[音声テキスト変換]、[Custom Speech]、[Testing]\(テスト\)** の順に移動します。
2. **[テストの追加]** をクリックします。
3. **[Evaluate accuracy]\(正確性の評価\)** を選択します。 テストの名前と説明を設定し、オーディオ + ヒューマン ラベル付け文字起こしデータセットを選択します。
4. テストするモデルを最大で 2 つ選択します。
5. **Create** をクリックしてください。

テストが正常に作成されたら、結果を並べて比較できます。

## <a name="side-by-side-comparison"></a>並べて比較

テストが完了すると、ステータスが "*成功*" に変わり、テストに含まれている両方のモデルの WER 数値が表示されます。 テストの名前をクリックして、テスト結果のページを表示します。 この詳細ページには、データセット内のすべての発話が一覧表示され、送信されたデータセットからの文字起こしと共に、2 つのモデルの認識結果が示されます。 並べた比較を検査しやすくするために、挿入、削除、置換を含むさまざまなエラーの種類を切り替えることができます。 音声を聞きながら各列の認識結果を比較すると、ヒューマン ラベル付け文字起こしと 2 つの音声テキスト変換モデルの結果が表示されるため、どちらのモデルがニーズに合い、どのような追加のトレーニングと改善が必要かを判断できます。

## <a name="next-steps"></a>次の手順

* [モデルをトレーニングする](how-to-custom-speech-train-model.md)
* [モデルをデプロイする](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>その他のリソース

* [データを準備してテストする](how-to-custom-speech-test-data.md)
* [データを調査する](how-to-custom-speech-inspect-data.md)
