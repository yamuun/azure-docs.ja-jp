---
title: Azure CDN でカスタム HTTPS を有効にするために許可された認証機関 | Microsoft Docs
description: 独自の証明書を使用してカスタム ドメインで HTTPS を有効にする場合、その証明書を作成するためには許可された証明機関 (CA) を使用する必要があります。
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2018
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: 8ce141fbf3d767159d16495bff5a93d791224c17
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/22/2019
ms.locfileid: "67331895"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Azure CDN でカスタム HTTPS を有効にするために許可された認証機関

**Azure CDN Standard from Microsoft** エンドポイント上の Azure Content Delivery Network (CDN) カスタム ドメインでは、[独自の証明書を使用して HTTPS 機能を有効にする](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#ssl-certificates)場合、許可された証明機関 (CA) を使用して SSL 証明書を作成する必要があります。 許可されていない CA や自己署名証明書を使用すると、要求が拒否されます。

> [!NOTE]
> 独自の証明書を使用してカスタム HTTPS を有効にするオプションは、**Azure CDN Standard from Microsoft** プロファイルでのみ利用できます。 
>

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]

