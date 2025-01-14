---
title: 'チュートリアル: Azure Active Directory と Mimecast Personal Portal の統合 | Microsoft Docs'
description: Azure Active Directory と Mimecast Personal Portal の間でシングル サインオンを構成する方法について説明します。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 345b22be-d87e-45a4-b4c0-70a67eaf9bfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ed7d8d1a2c2f8c515fa520f6701068e4655cad35
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "65871906"
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a>チュートリアル: Azure Active Directory と Mimecast Personal Portal の統合

このチュートリアルでは、Mimecast Personal Portal と Azure Active Directory (Azure AD) を統合する方法について説明します。
Mimecast Personal Portal と Azure AD の統合には、次の利点があります。

* Mimecast Personal Portal にアクセスする Azure AD ユーザーを制御できます。
* ユーザーが自分の Azure AD アカウントを使用して Mimecast Personal Portal に自動的にサインイン (シングル サインオン) できるようにすることができます。
* 1 つの中央サイト (Azure Portal) でアカウントを管理できます。

SaaS アプリと Azure AD の統合の詳細については、「 [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)」を参照してください。
Azure サブスクリプションをお持ちでない場合は、開始する前に[無料アカウントを作成](https://azure.microsoft.com/free/)してください。

## <a name="prerequisites"></a>前提条件

Azure AD と Mimecast Personal Portal の統合を構成するには、次のアイテムが必要です。

* Azure AD サブスクリプション。 Azure AD の環境がない場合は、[こちら](https://azure.microsoft.com/pricing/free-trial/)から 1 か月の評価版を入手できます
* Mimecast Personal Portal でのシングル サインオンが有効なサブスクリプション

## <a name="scenario-description"></a>シナリオの説明

このチュートリアルでは、テスト環境で Azure AD のシングル サインオンを構成してテストします。

* Mimecast Personal Portal では、**SP** によって開始される SSO がサポートされます

## <a name="adding-mimecast-personal-portal-from-the-gallery"></a>ギャラリーからの Mimecast Personal Portal の追加

Azure AD への Mimecast Personal Portal の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に Mimecast Personal Portal を追加する必要があります。

**ギャラリーから Mimecast Personal Portal を追加するには、次の手順を実行します。**

1. **[Azure Portal](https://portal.azure.com)** の左側のナビゲーション ウィンドウで、**[Azure Active Directory]** アイコンをクリックします。

    ![Azure Active Directory のボタン](common/select-azuread.png)

2. **[エンタープライズ アプリケーション]** に移動し、**[すべてのアプリケーション]** オプションを選択します。

    ![[エンタープライズ アプリケーション] ブレード](common/enterprise-applications.png)

3. 新しいアプリケーションを追加するには、ダイアログの上部にある **[新しいアプリケーション]** をクリックします。

    ![[新しいアプリケーション] ボタン](common/add-new-app.png)

4. 検索ボックスに「**Mimecast Personal Portal**」と入力し、結果ウィンドウで **[Mimecast Personal Portal]** を選択し、**[追加]** をクリックして、アプリケーションを追加します。

     ![結果リストの Mimecast Personal Portal](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成とテスト

このセクションでは、**Britta Simon** というテスト ユーザーに基づいて、Mimecast Personal Portal で Azure AD のシングル サインオンを構成し、テストします。
シングル サインオンを機能させるには、Azure AD ユーザーと Mimecast Personal Portal 内の関連ユーザーとの間にリンク関係が確立されている必要があります。

Mimecast Personal Portal で Azure AD のシングル サインオンを構成してテストするには、次の手順を完了する必要があります。

1. **[Azure AD シングル サインオンの構成](#configure-azure-ad-single-sign-on)** - ユーザーがこの機能を使用できるようにします。
2. **[Mimecast Personal Portal のシングル サインオンの構成](#configure-mimecast-personal-portal-single-sign-on)** - アプリケーション側でシングル サインオン設定を構成します。
3. **[Azure AD のテスト ユーザーの作成](#create-an-azure-ad-test-user)** - Britta Simon で Azure AD のシングル サインオンをテストします。
4. **[Azure AD テスト ユーザーの割り当て](#assign-the-azure-ad-test-user)** - Britta Simon が Azure AD シングル サインオンを使用できるようにします。
5. **[Mimecast Personal Portal テスト ユーザーの作成](#create-mimecast-personal-portal-test-user)** - Mimecast Personal Portal で Britta Simon に対応するユーザーを作成し、Azure AD の Britta Simon にリンクします。
6. **[シングル サインオンのテスト](#test-single-sign-on)** - 構成が機能するかどうかを確認します。

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成

このセクションでは、Azure portal 上で Azure AD のシングル サインオンを有効にします。

Mimecast Personal Portal で Azure AD シングル サインオンを構成するには、次の手順に従います。

1. [Azure portal](https://portal.azure.com/) の **Mimecast Personal Portal** アプリケーション統合ページで、**[シングル サインオン]** を選択します。

    ![シングル サインオン構成のリンク](common/select-sso.png)

2. **[シングル サインオン方式の選択]** ダイアログで、**[SAML/WS-Fed]** モードを選択して、シングル サインオンを有効にします。

    ![シングル サインオン選択モード](common/select-saml-option.png)

3. **[SAML でシングル サインオンをセットアップします]** ページで、**[編集]** アイコンをクリックして **[基本的な SAML 構成]** ダイアログを開きます。

    ![基本的な SAML 構成を編集する](common/edit-urls.png)

4. **[基本的な SAML 構成]** セクションで、次の手順を実行します。

    ![[Mimecast Personal Portal のドメインと URL] のシングル サインオン情報](common/sp-identifier-reply.png)

    a. **[サインオン URL]** ボックスに、次の URL を入力します。 

    | リージョン  |  値 | 
    | --------------- | --------------- | 
    | ヨーロッパ          | `https://eu-api.mimecast.com/login/saml`|
    | 米国   | `https://us-api.mimecast.com/login/saml`|
    | 南アフリカ    | `https://za-api.mimecast.com/login/saml`|
    | オーストラリア       | `https://au-api.mimecast.com/login/saml`|
    | オフショア        | `https://jer-api.mimecast.com/login/saml`|

    b. **[識別子]** ボックスに、次の形式で URL を入力します。

    | リージョン  |  値 | 
    | --------------- | --------------- |
    | ヨーロッパ          | `https://eu-api.mimecast.com/sso/<accountcode>`|
    | 米国   | `https://us-api.mimecast.com/sso/<accountcode>`|    
    | 南アフリカ    | `https://za-api.mimecast.com/sso/<accountcode>`|
    | オーストラリア       | `https://au-api.mimecast.com/sso/<accountcode>`|
    | オフショア        | `https://jer-api.mimecast.com/sso/<accountcode>`|

    c. **[応答 URL]** ボックスに、URL を入力します。 

    | リージョン  |  値 | 
    | --------------- | --------------- | 
    | ヨーロッパ          | `https://eu-api.mimecast.com/login/saml`|
    | 米国   | `https://us-api.mimecast.com/login/saml`|
    | 南アフリカ    | `https://za-api.mimecast.com/login/saml`|
    | オーストラリア       | `https://au-api.mimecast.com/login/saml`|
    | オフショア        | `https://jer-api.mimecast.com/login/saml`|

    > [!NOTE]
    > この識別子の値は実際のものではありません。 この値を実際の識別子で更新してください。 この値を取得するには、[Mimecast Personal Portal クライアント サポート チーム](https://www.mimecast.com/customer-success/technical-support/)にお問い合わせください。 Azure portal の **[基本的な SAML 構成]** セクションに示されているパターンを参照することもできます。

4. **[SAML でシングル サインオンをセットアップします]** ページの **[SAML 署名証明書]** セクションで、**[ダウンロード]** をクリックして要件のとおりに指定したオプションからの**証明書 (Base64)** をダウンロードして、お使いのコンピューターに保存します。

    ![証明書のダウンロードのリンク](common/certificatebase64.png)

6. **[Mimecast Personal Portal のセットアップ]** セクションで、要件に従って適切な URL をコピーします。

    ![構成 URL のコピー](common/copy-configuration-urls.png)

    a. ログイン URL

    b. Azure AD 識別子

    c. ログアウト URL

### <a name="configure-mimecast-personal-portal-single-sign-on"></a>Mimecast Personal Portal のシングル サインオンの構成

1. 別の Web ブラウザー ウィンドウで、Mimecast Personal Portal の企業サイトに管理者としてログインします。

2. **[サービス] \> [アプリケーション]** に移動します。
   
    ![アプリケーション](./media/mimecast-personal-portal-tutorial/ic794998.png "Applications")

3. **[認証プロファイル]** をクリックします。
   
    ![Authentication Profiles](./media/mimecast-personal-portal-tutorial/ic794999.png "Authentication Profiles")

4. **[新しい認証プロファイル]** をクリックします。
   
    ![新しい認証プロファイル](./media/mimecast-personal-portal-tutorial/ic795000.png "新しい認証プロファイル")

5. **[認証プロバイダー]** セクションで、次の手順に従います。
   
    ![Authentication Profile](./media/mimecast-personal-portal-tutorial/ic795001.png "Authentication Profile")
   
    a. **[説明]** テキストボックスに、構成の名前を入力します。
   
    b. **[Mimecast Personal Portal に SAML 認証を適用]** を選択します。
   
    c. **[プロバイダー]** で **[Azure Active Directory]** を選択します。
   
    d. **[発行者の URL]** ボックスに、Azure portal からコピーした **Azure AD 識別子**の値を貼り付けます。
   
    e. **[ログイン URL]** ボックスに、Azure portal からコピーした**ログイン URL** の値を貼り付けます。
   
    f. **[ログアウト URL]** ボックスに、Azure portal からコピーした**ログアウト URL** の値を貼り付けます。

    g. Azure Portal からダウンロードした **base-64** でエンコードされた証明書をメモ帳で開き、その内容をクリップボードにコピーして **[Identity Provider Certificate (Metadata)]\(ID プロバイダー証明書 \(メタデータ\)\)** ボックスに貼り付けます。

    h. **[シングル サインオンの許可]** を選択します。
   
    i. **[Save]** をクリックします。

### <a name="create-an-azure-ad-test-user"></a>Azure AD のテスト ユーザーの作成 

このセクションの目的は、Azure Portal で Britta Simon というテスト ユーザーを作成することです。

1. Azure portal の左側のウィンドウで、**[Azure Active Directory]**、**[ユーザー]**、**[すべてのユーザー]** の順に選択します。

    ![[ユーザーとグループ] と [すべてのユーザー] リンク](common/users.png)

2. 画面の上部にある **[新しいユーザー]** を選択します。

    ![[新しいユーザー] ボタン](common/new-user.png)

3. [ユーザーのプロパティ] で、次の手順を実行します。

    ![[ユーザー] ダイアログ ボックス](common/user-properties.png)

    a. **[名前]** フィールドに「**BrittaSimon**」と入力します。
  
    b. **[User name]\(ユーザー名\)** フィールドに「**brittasimon\@yourcompanydomain.extension**」と入力します。  
    たとえば、BrittaSimon@contoso.com のように指定します。

    c. **[パスワードを表示]** チェック ボックスをオンにし、[パスワード] ボックスに表示された値を書き留めます。

    d. **Create** をクリックしてください。

### <a name="assign-the-azure-ad-test-user"></a>Azure AD テスト ユーザーの割り当て

このセクションでは、Britta Simon に Mimecast Personal Portal へのアクセスを許可することで、このユーザーが Azure シングル サインオンを使用できるようにします。

1. Azure portal で **[エンタープライズ アプリケーション]** を選択し、**[すべてのアプリケーション]** を選択してから、**[Mimecast Personal Portal]** を選択します。

    ![[エンタープライズ アプリケーション] ブレード](common/enterprise-applications.png)

2. アプリケーションの一覧で、「**Mimecast Personal Portal**」と入力して選択します。

    ![アプリケーションの一覧の Mimecast Personal Portal のリンク](common/all-applications.png)

3. 左側のメニューで **[ユーザーとグループ]** を選びます。

    ![[ユーザーとグループ] リンク](common/users-groups-blade.png)

4. **[ユーザーの追加]** をクリックし、**[割り当ての追加]** ダイアログで **[ユーザーとグループ]** を選択します。

    ![[割り当ての追加] ウィンドウ](common/add-assign-user.png)

5. **[ユーザーとグループ]** ダイアログの [ユーザー] の一覧で **[Britta Simon]** を選択し、画面の下部にある **[選択]** ボタンをクリックします。

6. SAML アサーション内に任意のロール値が必要な場合、**[ロールの選択]** ダイアログでユーザーに適したロールを一覧から選択し、画面の下部にある **[選択]** をクリッします。

7. **[割り当ての追加]** ダイアログで、**[割り当て]** ボタンをクリックします。

### <a name="create-mimecast-personal-portal-test-user"></a>Mimecast Personal Portal テスト ユーザーの作成

Azure AD ユーザーが Mimecast Personal Portal にログインできるようにするには、そのユーザーを Mimecast Personal Portal にプロビジョニングする必要があります。 Mimecast Personal Portal の場合、プロビジョニングは手動で行います。

ユーザーを作成する前に、ドメインを登録する必要があります。

**ユーザー プロビジョニングを構成するには、次の手順に従います。**

1. 管理者として **[Mimecast Personal Portal]** にサインオンします。

2. **[ディレクトリ]\>[内部]** に移動します。
   
    ![Directories](./media/mimecast-personal-portal-tutorial/ic795003.png "Directories")

3. **[新規ドメインの登録]** をクリックします。
   
    ![Register New Domain](./media/mimecast-personal-portal-tutorial/ic795004.png "Register New Domain")

4. 新しいドメインを作成したら、 **[新規アドレス]** をクリックします。
   
    ![New Address](./media/mimecast-personal-portal-tutorial/ic795005.png "New Address")

5. 新規アドレス ダイアログで、プロビジョニングする有効な Azure AD アカウントを次の手順で設定します。
   
    ![保存](./media/mimecast-personal-portal-tutorial/ic795006.png "保存")
   
    a. **[Email Address]\(メール アドレス\)** ボックスに、ユーザーの**メール アドレス** (**BrittaSimon\@contoso.com** など) を入力します。
    
    b. **[Global Name]\(グローバル名\)** ボックスに、**ユーザー名**を「**BrittaSimon**」と入力します。

    c. **[Password]\(パスワード\)** および **[Confirm Password]\(確認パスワード\)** ボックスに、ユーザーの**パスワード**を入力します。
   
    b. **[Save]** をクリックします。

>[!NOTE]
>Mimecast Personal Portal から提供されている他の IdeaScale ユーザー アカウント作成ツールや API を使用して、Azure AD ユーザー アカウントをプロビジョニングできます。

### <a name="test-single-sign-on"></a>シングル サインオンのテスト 

このセクションでは、アクセス パネルを使用して Azure AD のシングル サインオン構成をテストします。

アクセス パネルで Mimecast Personal Portal のタイルをクリックすると、SSO を設定した Mimecast Personal Portal に自動的にサインインします。 アクセス パネルの詳細については、[アクセス パネルの概要](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)に関する記事を参照してください。

## <a name="additional-resources"></a>その他のリソース

- [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory の条件付きアクセスとは](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

