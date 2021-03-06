---
title: 教學課程：Azure Active Directory 與 Origami 整合 | Microsoft Docs
description: 了解如何在 Azure Active Directory 與 Origami 之間設定單一登入。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a28bb0ba-b564-46ba-accc-e587699295d4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: 1a3e01b7275b7d8329a9fc3bfc90e20398fdf38b
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "57845086"
---
# <a name="tutorial-azure-active-directory-integration-with-origami"></a>教學課程：Azure Active Directory 與 Origami 整合

在本教學課程中，您會了解如何整合 Origami 與 Azure Active Directory (Azure AD)。
Origami 與 Azure AD 整合提供下列優點：

* 您可以控制可存取 Origami 的人員的 Azure AD 中。
* 您可以讓您自動登入 origami （單一登入） 使用其 Azure AD 帳戶的使用者。
* 您可以在 Azure 入口網站中集中管理您的帳戶。

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱 [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。
如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>必要條件

若要設定 Azure AD 與 Origami 整合，您需要下列項目：

* Azure AD 訂用帳戶。 如果您沒有 Azure AD 環境，您可以在[這裡](https://azure.microsoft.com/pricing/free-trial/)取得一個月的試用帳戶
* 啟用 origami 單一登入的訂用帳戶

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD 單一登入。

* 支援 origami **SP**初始化的 SSO

## <a name="adding-origami-from-the-gallery"></a>從資源庫加入 Origami

若要設定將 Origami 整合到 Azure AD 中，您需要從資源庫將 Origami 加入到受控 SaaS 應用程式清單。

**若要從資源庫加入 Origami，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，按一下 [Azure Active Directory] 圖示。

    ![Azure Active Directory 按鈕](common/select-azuread.png)

2. 瀏覽至 [企業應用程式]，然後選取 [所有應用程式] 選項。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

3. 若要新增新的應用程式，請按一下對話方塊頂端的 [新增應用程式] 按鈕。

    ![新增應用程式按鈕](common/add-new-app.png)

4. 在 搜尋 方塊中，輸入**Origami**，選取**Origami**從結果面板中按一下 **新增**按鈕以新增應用程式。

     ![結果清單中的 origami](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

在本節中，設定及測試 Azure AD 單一登入以測試使用者身分，使用 Origami**名為 Britta Simon**。
讓單一登入運作，必須建立在 Azure AD 使用者與 Origami 中的相關的使用者之間的連結關聯性。

若要使用 Origami 來設定並測試 Azure AD 單一登入，您需要完成下列建置組塊：

1. **[設定 Azure AD 單一登入](#configure-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[設定 Origami 單一登入](#configure-origami-single-sign-on)** -若要在應用程式端設定單一登入設定。
3. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[建立 Origami 測試使用者](#create-origami-test-user)** -在 Origami 與 Azure AD 中代表使用者的連結的 Britta Simon 的對應。
6. **[測試單一登入](#test-single-sign-on)**，驗證組態是否能運作。

### <a name="configure-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入。

若要設定 Azure AD 單一登入與 Origami 搭配運作，請執行下列步驟：

1. 在  [Azure 入口網站](https://portal.azure.com/)上**Origami**應用程式整合頁面上，選取**單一登入**。

    ![設定單一登入連結](common/select-sso.png)

2. 在 [選取單一登入方法] 對話方塊中，選取 [SAML/WS-Fed] 模式以啟用單一登入。

    ![單一登入選取模式](common/select-saml-option.png)

3. 在 [以 SAML 設定單一登入] 頁面上，按一下 [編輯] 圖示以開啟 [基本 SAML 設定] 對話方塊。

    ![編輯基本 SAML 組態](common/edit-urls.png)

4. 在 [基本 SAML 組態] 區段上，執行下列步驟：

    ![Origami 網域與 Url 單一登入資訊](common/sp-signonurl.png)

    在 [登入 URL] 文字方塊中，以下列模式輸入 URL︰`https://live.origamirisk.com/origami/account/login?account=<companyname>`

    > [!NOTE]
    > 這不是真正的值。 請使用實際的「登入 URL」來更新此值。 請連絡 [Origami 客戶支援小組](https://wordpress.org/support/theme/origami)以取得此值。 您也可以參考 Azure 入口網站中**基本 SAML 組態**區段所示的模式。

5. 在 [以 SAML 設定單一登入] 頁面的 [SAML 簽署憑證] 區段中，按一下 [下載]，以依據您的需求從指定選項下載 [憑證 (Base64)]，並儲存在您的電腦上。

    ![憑證下載連結](common/certificatebase64.png)

6. 在 **設定 Origami**區段上，複製 根據您的需求適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

    a. 登入 URL

    b. Azure AD 識別碼

    c. 登出 URL

### <a name="configure-origami-single-sign-on"></a>Origami 單一登入設定

1. 以系統管理員權限登入 Origami 帳戶。

2. 在頂端的功能表中，按一下 [系統管理員] 。
   
    ![設定單一登入](./media/origami-tutorial/tutorial_origami_51.png)

3. 在 [單一登入設定] 對話方塊頁面執行下列步驟：
   
    ![設定單一登入](./media/origami-tutorial/tutorial_origami_531.png)

    a. 選取 [啟用單一登入] 。

    b. 在 **身分識別提供者的登入頁面 URL**文字方塊中，貼上值**登入 URL**，您從 Azure 入口網站複製。

    c. 在 **身分識別提供者的登出頁面 URL**文字方塊中，貼上值**登出 URL**，您從 Azure 入口網站複製。

    d. 按一下 [瀏覽] 以上傳您從 Azure 入口網站下載的憑證。

    e. 按一下 [儲存變更] 。

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者 

本節的目標是要在 Azure 入口網站中建立一個名為 Britta Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，依序選取 [Azure Active Directory]、[使用者] 和 [所有使用者]。

    ![[使用者和群組] 與 [所有使用者] 連結](common/users.png)

2. 在畫面頂端選取 [新增使用者]。

    ![[新增使用者] 按鈕](common/new-user.png)

3. 在 [使用者] 屬性中，執行下列步驟。

    ![[使用者] 對話方塊](common/user-properties.png)

    a. 在 [名稱] 欄位中，輸入 **BrittaSimon**。
  
    b. 在 [使用者名稱] 欄位中，輸入 **brittasimon@yourcompanydomain.extension**  
    例如， BrittaSimon@contoso.com

    c. 選取 [顯示密碼] 核取方塊，然後記下 [密碼] 方塊中顯示的值。

    d. 按一下頁面底部的 [新增] 。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會把 Origami 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取**企業應用程式**，選取**所有應用程式**，然後選取**Origami**。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，選取 [Origami] 。

    ![應用程式清單中的 [Origami] 連結](common/all-applications.png)

3. 在左側功能表中，選取 [使用者和群組]。

    ![[使用者和群組] 連結](common/users-groups-blade.png)

4. 按一下 [新增使用者] 按鈕，然後在 [新增指派] 對話方塊中，選取 [使用者和群組]。

    ![[新增指派] 窗格](common/add-assign-user.png)

5. 在 [使用者和群組] 對話方塊的 [使用者] 清單中，選取 [Britta Simon]，然後按一下畫面底部的 [選取] 按鈕。

6. 如果您預期使用 SAML 判斷提示中的任何角色值，請在 [選取角色] 對話方塊的清單中選取適當使用者角色，然後按一下畫面底部的 [選取] 按鈕。

7. 在 [新增指派] 對話方塊中，按一下 [指派] 按鈕。

### <a name="create-origami-test-user"></a>建立 Origami 測試使用者

在本節中，您要在 Origami 中建立名為 Britta Simon 的使用者。 

1. 以系統管理員權限登入 Origami 帳戶。

2. 在頂端的功能表中，按一下 [系統管理員] 。
   
    ![設定單一登入](./media/origami-tutorial/tutorial_origami_51.png)

3. 在 [使用者與安全性] 對話方塊中，按一下 [使用者]。
   
    ![設定單一登入](./media/origami-tutorial/tutorial_origami_54.png)

4. 按一下 [新增使用者] 。
   
    ![設定單一登入](./media/origami-tutorial/tutorial_origami_55.png)

5. 在 [新增使用者] 對話方塊上，執行下列步驟：
   
    ![設定單一登入](./media/origami-tutorial/tutorial_origami_56.png)

    a. 在 **使用者名**文字方塊中，輸入使用者電子郵件，例如**brittasimon\@contoso.com**。

    b. 在 [密碼] 文字方塊中輸入密碼。

    c. 在 [確認密碼] 文字方塊中再次輸入密碼。

    d. 在 [名字] 文字方塊中，輸入使用者的名字，例如 **Britta**。

    e. 在 [姓氏] 文字方塊中，輸入使用者的姓氏，例如 **Simon**。

    f. 按一下 [檔案] 。
   
    ![設定單一登入](./media/origami-tutorial/tutorial_origami_57.png)

6. 將**使用者角色**和**用戶端存取**指派給使用者。 
   
    ![設定單一登入](./media/origami-tutorial/tutorial_origami_58.png)

### <a name="test-single-sign-on"></a>測試單一登入 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您按一下存取面板 」 中的 [Origami] 圖格時，您應該會自動登入 Origami，讓您設定 SSO。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什麼是 Azure Active Directory 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

