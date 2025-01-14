---
title: 認証フロー (Microsoft Authentication Library) | Azure
description: Microsoft Authentication Library (MSAL) で使用される認証フロー/許可について説明します。
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/25/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: cb9a6f162a10408469669cf40b29efc6d2903944
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/11/2019
ms.locfileid: "65546040"
---
# <a name="authentication-flows"></a>認証フロー

この記事では、Microsoft Authentication Library (MSAL) によって提供されるさまざまな認証フローについて説明します。  これらのフローは、さまざまなアプリケーション シナリオで使用できます。

| Flow | 説明 | 使用される場所|  
| ---- | ----------- | ------- | 
| [対話](#interactive) | ブラウザーまたはポップアップ ウィンドウでユーザーに資格情報を求める対話型プロセスによりトークンを取得します。 | [デスクトップ アプリ](scenario-desktop-overview.md)、[モバイル アプリ](scenario-mobile-overview.md) |
| [暗黙的な許可](#implicit-grant) | バックエンド サーバーと資格情報をやり取りせずに、アプリはトークンを取得できます。 これにより、アプリは、ユーザーのサインイン、セッションの維持、他の Web API へのトークンの取得をすべてクライアント JavaScript コード内で実行できます。| [シングル ページ アプリケーション (SPA)](scenario-spa-overview.md) |
| [承認コード](#authorization-code) | デバイスにインストールされているアプリで、Web API などの保護されたリソースにアクセスするために使用されます。 これにより、モバイル アプリおよびデスクトップ アプリにサインインおよび API アクセスを追加できます。 | [デスクトップ アプリ](scenario-desktop-overview.md)、[モバイル アプリ](scenario-mobile-overview.md)、[Web アプリ](scenario-web-app-call-api-overview.md) | 
| [On-Behalf-Of](#on-behalf-of) | アプリケーションでサービス/Web API を呼び出し、それがさらに別のサービス/Web API を呼び出す必要があります。 その考え方は、委任されたユーザー ID とアクセス許可を要求チェーン経由で伝達するというものです。 | [Web API](scenario-web-api-call-api-overview.md) |
| [クライアントの資格情報](#client-credentials) | アプリケーションの ID を使って Web でホストされているリソースにアクセスできます。 バックグラウンドでの実行が必要なサーバー間の相互作用に使用され、ユーザーとの即時の相互動作は必要ありません。 | [デーモン アプリ](scenario-daemon-overview.md) |
| [デバイス コード](#device-code) | ユーザーは、スマート TV、IoT デバイス、プリンターなどの入力制限のあるデバイスにサインインできます。 | [デスクトップ/モバイル アプリ](scenario-desktop-acquire-token.md#command-line-tool-without-web-browser) |
| [統合 Windows 認証](scenario-desktop-acquire-token.md#integrated-windows-authentication) | ドメインまたは Azure AD に参加しているコンピューターのアプリケーションは、サイレントで (ユーザーからの UI 操作なしで) トークンを取得できます。| [デスクトップ/モバイル アプリ](scenario-desktop-acquire-token.md#integrated-windows-authentication) |
| [ユーザー名/パスワード](scenario-desktop-acquire-token.md#username--password) | アプリケーションはパスワードを直接処理することによって、ユーザーをサインインさせることができます。 このフローは推奨されません。 | [デスクトップ/モバイル アプリ](scenario-desktop-acquire-token.md#username--password) | 

## <a name="interactive"></a>Interactive
MSAL では、対話形式でユーザーにサインインのための資格情報の入力を要求し、それらの資格情報を使ってトークンを取得する機能がサポートされています。

![対話型フロー](media/msal-authentication-flows/interactive.png)

特定のプラットフォームで MSAL.NET を使ってトークンを対話形式で取得する方法について詳しくは、以下をご覧ください。
- [Xamarin Android](msal-net-xamarin-android-considerations.md)
- [Xamarin iOS](msal-net-xamarin-ios-considerations.md)
- [ユニバーサル Windows プラットフォーム](msal-net-uwp-considerations.md)

MSAL.js での対話型の呼び出しについて詳しくは、「[Prompt behavior in MSAL.js interactive requests (MSAL.js の対話型要求でのプロンプトの動作)](msal-js-prompt-behavior.md)」をご覧ください

## <a name="implicit-grant"></a>Implicit grant (暗黙的な付与)

MSAL でサポートされている [OAuth 2 の暗黙的な許可のフロー](v2-oauth2-implicit-grant-flow.md)では、バックエンド サーバーとの資格情報交換を実行しなくても、アプリが Microsoft ID プラットフォームからトークンを取得できます。 これにより、アプリは、ユーザーのサインイン、セッションの維持、他の Web API へのトークンの取得をすべてクライアント JavaScript コード内で実行できます。

![暗黙的な許可のフロー](media/msal-authentication-flows/implicit-grant.svg)

数多くの最新の Web アプリケーションは、JavaScript やAngular、Vue.js、React.js などの SPA フレームワークを使用して記述されたクライアント側のシングルページ アプリケーションとして構築されます。 これらのアプリケーションは、Web ブラウザーで実行され、従来のサーバー側 Web アプリケーションとは別の認証特性を持ちます。 Microsoft ID プラットフォームにより、シングルページ アプリケーションでユーザーをサインインさせ、暗黙的な許可のフローを使用してバックエンド サービスまたは Web API にアクセスするトークンを取得できます。 暗黙的フローにより、アプリケーションから、ID トークンが認証されたユーザーを表す ID トークンを取得し、保護された API の呼び出しに必要なトークンにもアクセスできます。

この認証フローには、Electron や React-Native などのクロスプラットフォーム JavaScript フレームワークを使用するアプリケーションのシナリオは含まれません。これらでは、さらにネイティブ プラットフォームと対話する機能が必要であるためです。

## <a name="authorization-code"></a>Authorization code (承認コード)
MSAL では、デバイスにインストールされているアプリに、Web API など、保護されているリソースにアクセスする権利を与えるために使用できる、[OAuth 2.0 認証コード付与](v2-oauth2-auth-code-flow.md)がサポートされています。 これにより、モバイル アプリおよびデスクトップ アプリにサインインおよび API アクセスを追加できます。 

ユーザーが Web アプリケーション (Web サイト) にサインインするとき、Web アプリケーションは承認コードを受け取ります。  承認コードと引き換えに、Web API を呼び出すためのトークンを取得します。 ASP.NET/ASP.NET Core Web アプリでは、`AcquireTokenByAuthorizationCode` の唯一の目的は、`AcquireTokenSilent` を使用する API のためだけにトークンを取得するアプリケーション (通常はコントローラー) で使用できるように、トークン キャッシュにトークンを追加することだけです。

![承認コード フロー](media/msal-authentication-flows/authorization-code.png)

1. アクセス トークンと引き換えられる承認コードを要求します。
2. アクセス トークンを使用して Web API を呼び出します。

### <a name="considerations"></a>考慮事項
- 承認コードは、トークンの引き換えに 1 回だけ使用できます。 同じ承認コードで何回もトークンを取得しないでください (プロトコル標準の仕様で明確に禁止されています)。 意図的に何回もコードを引き換えた場合、またはフレームワークでも自動的に行われていたことを知らなかった場合は、次のエラーが発生します。 `AADSTS70002: Error validating credentials. AADSTS54005: OAuth2 Authorization code was already redeemed, please retry with a new valid code or use an existing refresh token.`

- ASP.NET/ASP.NET Core アプリケーションを作成する場合、承認コードを既に引き換えたことを ASP.NET/Core フレームワークに通知しないと、これが発生する可能性があります。 その場合は、`AuthorizationCodeReceived` イベント ハンドラーの `context.HandleCodeRedemption()` メソッドを呼び出す必要があります。

- アクセス トークンを ASP.NET と共有しないでください。増分同意の正しい処理が妨げられる可能性があります。  詳細については、[問題 #693](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/693) を参照してください。

## <a name="on-behalf-of"></a>On-Behalf-Of

MSAL では、[OAuth 2 の On-Behalf-Of 認証フロー](v2-oauth2-on-behalf-of-flow.md)がサポートされています。  このフローは、アプリケーションでサービス/Web API を呼び出し、それがさらに別のサービス/Web API を呼び出す必要がある場合に、使用されます。 その考え方は、委任されたユーザー ID とアクセス許可を要求チェーン経由で伝達するというものです。 中間層サービスがダウンストリーム サービスに認証済み要求を発行するには、そのサービスは Microsoft ID プラットフォームからのアクセス トークンをユーザーに代わってセキュリティ保護する必要があります。

![On-Behalf-Of フロー](media/msal-authentication-flows/on-behalf-of.png)

1. Web API 用のアクセス トークンを取得します
2. クライアント (Web、デスクトップ、モバイル、シングルページ アプリケーション) が保護された Web API を呼び出し、HTTP 要求の認証ヘッダーのベアラー トークンとしてアクセス トークンを追加します。 Web API がユーザーを認証します。
3. クライアントが Web API を呼び出すと、Web API はユーザーの代わりに別のトークンを要求します。  
4. 保護された Web API は、このトークンを使用して、ユーザーの代わりにダウンストリームの Web API を呼び出します。  Web API は、他のダウンストリーム API のトークンを (ただし、ここでも同じユーザーの代わりとして) 後で要求することもできます。

## <a name="client-credentials"></a>クライアントの資格情報

MSAL では、[OAuth 2 のクライアント資格情報フロー](v2-oauth2-client-creds-grant-flow.md)がサポートされています。 これにより、アプリケーションの ID を使って Web でホストされているリソースにアクセスできます。 この種類の許可は、バックグラウンドでの実行が必要なサーバー間の相互作用に使用され、ユーザーとの即時の相互動作は必要ありません。 これらのアプリケーションは、デーモンまたはサービス アカウントと呼ばれます。 

クライアント資格情報許可フローでは、Web サービス (Confidential クライアント) が別の Web サービスを呼び出すときに、ユーザーを偽装する代わりに、独自の資格情報を使用して認証することができます。 このシナリオでは、クライアントは通常、中間層の Web サービス、デーモン サービス、または Web サイトです。 高いレベルの保証では、Microsoft ID プラットフォームにより、呼び出し元サービスが、資格情報として (共有シークレットではなく) 証明書を使用することもできます。

> [!NOTE]
> モバイル プラットフォーム (UWP、Xamarin.iOS、Xamarin.Android) では、パブリック クライアント アプリケーションのみがサポートされているため、機密性の高いクライアント フローは利用できません。  パブリック クライアント アプリケーションでは、ID プロバイダーに対してアプリケーションの ID を証明する方法がわかりません。 Web アプリまたは Web API バックエンドでは、証明書をデプロイすることで、セキュリティ保護された接続を実現できます。

MSAL.NET では、2 種類のクライアント資格情報がサポートされています。 これらのクライアント資格情報は、Azure AD に登録する必要があります。 資格情報は、コード内の機密クライアント アプリケーションのコンストラクターに渡されます。

### <a name="application-secrets"></a>アプリケーション シークレット 

![パスワードを使用する機密クライアント](media/msal-authentication-flows/confidential-client-password.png)

1. アプリケーションのシークレット/パスワード資格情報を使用してトークンを取得します。
2. トークンを使用してリソースの要求を行います。

### <a name="certificates"></a>証明書 

![証明書を使用する機密クライアント](media/msal-authentication-flows/confidential-client-certificate.png)

1. 証明書の資格情報を使用してトークンを取得します。
2. トークンを使用してリソースの要求を行います。

これらのクライアント資格情報では以下のことが必要です。
- Azure AD に登録されている。
- コード内の機密クライアント アプリケーションのコンストラクターに渡される。


## <a name="device-code"></a>デバイス コード
MSAL では、ユーザーがスマート TV、IoT デバイス、プリンターなどの入力制限のあるデバイスにサインインできるようにする [OAuth 2 デバイス コード フロー](v2-oauth2-device-code.md)がサポートされています。 Azure AD による対話型認証には Web ブラウザーが必要です。 Web ブラウザーを提供しないデバイスまたはオペレーティング システムでは、ユーザーはデバイス コード フローによって別のデバイス (たとえば、別のコンピューターや携帯電話) を使用して対話形式でサインインできます。

デバイス コード フローを使用すると、アプリケーションでは、これらのデバイス/OS 用に特別に設計された 2 ステップ プロセスを通じてトークンを取得します。 このようなアプリケーションには、iOT デバイスで実行されているアプリケーションやコマンドライン ツール (CLI) などがあります。 

![デバイス コード フロー](media/msal-authentication-flows/device-code.png)

1. ユーザー認証が必要になるたびに、アプリがコードを提供し、ユーザーに別のデバイス (インターネットに接続されたスマートフォンなど) を使用して特定の URL (たとえば、 https://microsoft.com/devicelogin)) に移動するよう求めます。ここで、ユーザーはコードの入力を求められます。 完了すると、Web ページから通常の認証エクスペリエンス (必要に応じて、同意のプロンプトや多要素認証を含む) が実行されます。

2. 認証が成功すると、コマンド ライン アプリはバック チャネル経由で必要なトークンを受信し、そのトークンを使用して必要な Web API の呼び出しを実行します。

### <a name="constraints"></a>制約

- デバイス コード フローは、パブリック クライアント アプリケーションでのみ使用できます。
- パブリック クライアント アプリケーションを構築するときに渡される機関は、次のいずれかである必要があります。
  - テナント化 (`https://login.microsoftonline.com/{tenant}/` の形式。`{tenant}` は、テナント ID を表す GUID またはテナントに関連付けられているドメイン)。
  - 任意の職場または学校アカウント (`https://login.microsoftonline.com/organizations/`)。
- Microsoft の個人用アカウントは、Azure AD v2.0 エンドポイントではまだサポートされていません (`/common` または `/consumers` テナントを使用することはできません)。

## <a name="integrated-windows-authentication"></a>統合 Windows 認証
MSAL では、ドメイン参加済みまたは Azure AD 参加済みの Windows コンピューターで実行されているデスクトップまたはモバイル アプリケーションに対して統合 Windows 認証 (IWA) がサポートされています。 IWA を使用して、これらのアプリケーションはトークンをサイレントに (ユーザーからの UI 操作なしで) 取得できます。 

![統合 Windows 認証](media/msal-authentication-flows/integrated-windows-authentication.png)

1. 統合 Windows 認証を使用してトークンを取得します。
2. トークンを使用してリソースの要求を行います。

### <a name="constraints"></a>制約

IWA では**フェデレーション** ユーザーのみがサポートされています。  Active Directory で作成され、Azure Active Directory によって支援されているユーザーです。 Azure AD で直接作成され、AD による支援のないユーザー (**マネージド** ユーザー) はこの認証フローを使用できません。 この制限は、[ユーザー名/パスワードのフロー](#usernamepassword)には影響しません。

IWA は、.NET Framework、.NET Core、およびユニバーサル Windows プラットフォーム用に記述されたアプリを対象としています。

IWA では MFA (多要素認証) はバイパスされません。 MFA が構成されている状況では、MFA チャレンジが必要な場合に IWA が失敗する可能性があります。これは、MFA でユーザーの操作が必要になるためです。 これには注意が必要です。 IWA は非対話型ですが、2 要素認証 (2FA) にはユーザーの操作が必要です。 ID プロバイダーが 2FA の実行を要求するタイミングの制御は、ユーザーではなくテナント管理者が行います。 当社の観測によると、2FA が必要なのは、他の国からログインする場合と VPN 経由で企業ネットワークに接続されていない場合です。ただし、VPN 経由で接続されている場合であっても 2FA が必要になる可能性があります。 確定的なルール セットを想定しないでください。Azure Active Directory では AI を使用して、2FA が必要かどうかを継続的に学習します。 IWA が失敗した場合は、ユーザー プロンプト (https://aka.ms/msal-net-interactive) にフォールバックする必要があります。

パブリック クライアント アプリケーションを構築するときに渡される機関は、次のいずれかである必要があります。
- テナント化 (`https://login.microsoftonline.com/{tenant}/` の形式。`tenant` は、テナント ID を表す GUID またはテナントに関連付けられているドメイン)。
- 任意の職場または学校アカウント用 (`https://login.microsoftonline.com/organizations/`)。 Microsoft の個人用アカウントはサポートされていません (`/common` テナントまたは `/consumers` テナントを使用することはできません)。

IWA はサイレント フローであるため、次のようになります。
- アプリケーションのユーザーが、アプリケーションの使用に事前に同意しておく必要があります。 
- または、テナント管理者が、テナント内のすべてのユーザーによるアプリケーションの使用に事前に同意しておく必要があります。
- これは、次のことを意味します。
    - 開発者が自分用に Azure portal 上の **[許可]** ボタンをクリックしておきます。 
    - または、テナント管理者がアプリケーションの登録ページの **[API のアクセス許可]** タブにある **[{テナント ドメイン} の管理者の同意を付与/取り消す]** ボタンをクリックしておきます (「[Web API にアクセスするためのアクセス許可を追加する](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)」を参照)
    - または、ユーザーがアプリケーションに同意する方法を指定しておきます (「[個々のユーザーの同意を要求する](v2-permissions-and-consent.md#requesting-individual-user-consent)」を参照)
    - または、テナント管理者がアプリケーションに同意する方法を指定しておきます ([管理者の同意](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant)に関するセクションを参照)

IWA フローは、.NET デスクトップ、.NET Core、および Windows ユニバーサル プラットフォームの各アプリに対して有効です。 .NET Core では、ユーザー名を受け取るオーバーロードだけを使用できます。これは、.NET Core プラットフォームで OS に対してユーザー名を問い合わせることができないためです。
  
同意の詳細については、[v2.0 のアクセス許可と同意](v2-permissions-and-consent.md)に関するページを参照してください。

## <a name="usernamepassword"></a>ユーザー名/パスワード 
MSAL では、[OAuth 2 のリソース所有者のパスワード資格情報許可](v2-oauth-ropc.md)がサポートされています。これにより、アプリケーションでは、ユーザーのパスワードを直接処理することでユーザーをサインインさせることができます。 デスクトップ アプリケーションでは、ユーザー名/パスワードのフローを使ってサイレントにトークンを取得できます。 アプリケーションを使用するときに UI は必要ありません。

![ユーザー名/パスワードのフロー](media/msal-authentication-flows/username-password.png)

1. ID プロバイダーにユーザー名とパスワードを送信することによって、トークンを取得します。
2. トークンを使用して Web API を呼び出します。

> [!WARNING]
> このフローは、高度な信頼とユーザーの公開が必要であるため、**お勧めしません**。  他のもっと安全なフローを使用できない場合にのみ、このフローを使用する必要があります。 この問題の詳細については、[この記事](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/)を参照してください。 

Windows ドメイン参加済みマシン上でトークンを自動的に取得するために推奨されるフローは、[統合 Windows 認証](#integrated-windows-authentication)です。 それ以外の場合は、[デバイス コード フロー](#device-code)を使用することもできます

これが役立つケース (DevOps のシナリオ) もありますが、独自の UI を提供する対話形式のシナリオでユーザー名/パスワードを使用する場合は、別の方法について検討してください。 ユーザー名/パスワードを使用すると、いくつかのことを利用できなくなります。
- 最新の ID のコア テナント: パスワードがフィッシングされて再生されます。 インターセプトできる共有シークレットのこの概念を使用しているためです。
これは、パスワードなしとは互換性がありません。
- MFA を実行する必要のあるユーザーがサインインできない (対話式操作がないため)
- ユーザーがシングル サインオンできない

### <a name="constraints"></a>制約

[統合 Windows 認証の制約](#integrated-windows-authentication)以外に、次の制約も適用されます。

- ユーザー名/パスワードのフローは、条件付きアクセスおよび多要素認証との互換性がありません。その結果、テナント管理者が多要素認証を必要とする Azure AD テナントでアプリを実行する場合は、このフローを使用できません。 これは多くの組織に該当します。
- 職場および学校アカウント (MSA ではありません) にのみ有効です
- そのフローは、.NET デスクトップと .NET Core では使用できますが、ユニバーサル Windows プラットフォームでは使用できません。

### <a name="azure-ad-b2c-specifics"></a>Azure AD B2C での詳細

MSAL.NET と Azure AD B2C の使用について詳しくは、[Using ROPC with Azure AD B2C (MSAL.NET) (Azure AD B2C での ROPC の使用 (MSAL.NET))](msal-net-aad-b2c-considerations.md#resource-owner-password-credentials-ropc-with-azure-ad-b2c) に関する記事をご覧ください。
