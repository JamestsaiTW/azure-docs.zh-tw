---
title: 教學課程：Azure Active Directory 與 Marketo 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Marketo 之間的單一登入。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b88c45f5-d288-4717-835c-ca965add8735
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/19/2019
ms.author: jeedes
ms.openlocfilehash: 6eb7c294b884b94586458f1e6e3dc283549ee31e
ms.sourcegitcommit: 1afd2e835dd507259cf7bb798b1b130adbb21840
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/28/2019
ms.locfileid: "56984283"
---
# <a name="tutorial-azure-active-directory-integration-with-marketo"></a>教學課程：Azure Active Directory 與 Marketo 整合

在本教學課程中，您會了解如何整合 Marketo 與 Azure Active Directory (Azure AD)。
Marketo 與 Azure AD 整合提供下列優點：

* 您可以在 Azure AD 中控制可存取 Marketo 的人員。
* 您可以讓使用者使用其 Azure AD 帳戶自動登入 Marketo (單一登入)。
* 您可以在 Azure 入口網站中集中管理您的帳戶。

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱 [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。
如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>必要條件

若要設定 Azure AD 與 Marketo 整合，您需要下列項目：

* Azure AD 訂用帳戶。 如果您沒有 Azure AD 環境，您可以在[這裡](https://azure.microsoft.com/pricing/free-trial/)取得一個月的試用帳戶
* 已啟用 Marketo 單一登入的訂用帳戶

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD 單一登入。

* Marketo 支援由 **IDP** 起始的 SSO

## <a name="adding-marketo-from-the-gallery"></a>從資源庫新增 Marketo

若要設定 Marketo 與 Azure AD 整合，您需要從資源庫將 Marketo 新增到受控 SaaS 應用程式清單。

**若要從資源庫加入 Marketo，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，按一下 [Azure Active Directory] 圖示。

    ![Azure Active Directory 按鈕](common/select-azuread.png)

2. 瀏覽至 [企業應用程式]，然後選取 [所有應用程式] 選項。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

3. 若要新增新的應用程式，請按一下對話方塊頂端的 [新增應用程式] 按鈕。

    ![新增應用程式按鈕](common/add-new-app.png)

4. 在搜尋方塊中，輸入 **Marketo**，從結果面板中選取 [Marketo]，然後按一下 [新增] 按鈕以新增應用程式。

     ![結果清單中的 Marketo](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

在本節中，您將以名為 **Britta Simon** 的測試使用者身分，設定及測試與 Marketo 搭配運作的 Azure AD 單一登入。
若要讓單一登入能夠運作，必須建立 Azure AD 使用者與 Marketo 中相關使用者之間的連結關聯性。

若要設定及測試與 Marketo 搭配運作的 Azure AD 單一登入，您需要完成下列構成要素：

1. **[設定 Azure AD 單一登入](#configure-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[設定 Marketo 單一登入](#configure-marketo-single-sign-on)** - 在應用程式端設定單一登入設定。
3. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[建立 Marketo 測試使用者](#create-marketo-test-user)** - 使 Marketo 中對應的 Britta Simon 連結到該使用者在 Azure AD 中的代表項目。
6. **[測試單一登入](#test-single-sign-on)**，驗證組態是否能運作。

### <a name="configure-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入。

若要使用 Marketo 設定 Azure AD 單一登入，請執行下列步驟：

1. 在 [Azure 入口網站](https://portal.azure.com/) 的 [Marketo] 應用程式整合頁面上，選取 [單一登入]。

    ![設定單一登入連結](common/select-sso.png)

2. 在 [選取單一登入方法] 對話方塊中，選取 [SAML/WS-Fed] 模式以啟用單一登入。

    ![單一登入選取模式](common/select-saml-option.png)

3. 在 [以 SAML 設定單一登入] 頁面上，按一下 [編輯] 圖示以開啟 [基本 SAML 設定] 對話方塊。

    ![編輯基本 SAML 組態](common/edit-urls.png)

4. 在 [以 SAML 設定單一登入] 頁面上，執行下列步驟：

    ![Marketo 網域與 URL 單一登入資訊](common/idp-intiated.png)

    a. 在 [識別碼] 文字方塊中，使用下列模式來輸入 URL：`https://saml.marketo.com/sp`

    b. 在 [回覆 URL] 文字方塊中，使用下列模式來輸入 URL：`https://login.marketo.com/saml/assertion/\<munchkinid\>`

    > [!NOTE]
    > 這些都不是真正的值。 請使用實際的識別碼和回覆 URL 更新這些值。 請連絡[Marketo 用戶端支援小組](http://investors.marketo.com/contactus.cfm)以取得這些值。 您也可以參考 Azure 入口網站中**基本 SAML 組態**區段所示的模式。

5. 在 [以 SAML 設定單一登入] 頁面的 [SAML 簽署憑證] 區段中，按一下 [下載]，以依據您的需求從指定選項下載 [憑證 (Base64)]，並儲存在您的電腦上。

    ![憑證下載連結](common/certificatebase64.png)

6. 在 [設定 Marketo] 區段上，依據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

    a. 登入 URL

    b. Azure AD 識別碼

    c. 登出 URL

### <a name="configure-marketo-single-sign-on"></a>設定 Marketo 單一登入

1. 若要取得您的應用程式的 Munchkin 識別碼，請使用系統管理員認證登入 Marketo，執行以下動作︰
   
    a. 使用管理員認證登入 Marketo 應用程式。
   
    b. 按一下頂端瀏覽窗格中的 [管理] 按鈕。
   
    ![設定單一登入](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. 瀏覽至 [整合] 功能表，然後按一下 [Munchkin] 連結。
   
    ![設定單一登入](./media/marketo-tutorial/tutorial_marketo_11.png)
   
    d. 複製螢幕上顯示的 Munchkin 識別碼，完成 Azure AD 設定精靈中的回覆 URL。
   
    ![設定單一登入](./media/marketo-tutorial/tutorial_marketo_12.png) 

2. 若要設定應用程式中的 SSO，請遵循下列步驟︰
   
    a. 使用管理員認證登入 Marketo 應用程式。
   
    b. 按一下頂端瀏覽窗格中的 [管理] 按鈕。
   
    ![設定單一登入](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. 瀏覽至 [整合] 功能表，然後按一下 [單一登入] 。
   
    ![設定單一登入](./media/marketo-tutorial/tutorial_marketo_07.png) 
   
    d. 若要啟用 SAML 設定，按一下 [編輯]按鈕。
   
    ![設定單一登入](./media/marketo-tutorial/tutorial_marketo_08.png) 
   
    e. **已啟用**單一登入設定。
   
    f. 在 [簽發者識別碼] 文字方塊中，貼上 [Azure AD 識別碼]。
   
    g. 在 [實體識別碼] 文字方塊中，輸入 URL `http://saml.marketo.com/sp`。
   
    h. 選取 [使用者識別碼位置] 做為 [名稱識別碼元素]。
   
    ![設定單一登入](./media/marketo-tutorial/tutorial_marketo_09.png)
   
    > [!NOTE]
    > 如果您的使用者識別碼不是 UPN 值，則至 [屬性] 索引標籤中變更其值。
   
    i. 上傳您從 Azure AD 設定精靈下載的憑證。 **儲存**設定。
   
    j. 編輯 [重新導向頁面] 設定。
   
    k. 在 [登入 URL] 文字方塊中，貼上 [登入 URL]。
   
    l. 在 [登出 URL] 文字方塊中，貼上登出 URL。
   
    m. 在 [錯誤 URL] 中，複製您的 **Marketo 執行個體 URL**，並按一下 [儲存] 按鈕以儲存設定。
   
    ![設定單一登入](./media/marketo-tutorial/tutorial_marketo_10.png)

3. 若要啟用使用者的 SSO，完成下列動作：
   
    a. 使用管理員認證登入 Marketo 應用程式。
   
    b. 按一下頂端瀏覽窗格中的 [管理] 按鈕。
   
    ![設定單一登入](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. 瀏覽至 [安全性] 功能表，然後按一下 [登入設定]。
   
    ![設定單一登入](./media/marketo-tutorial/tutorial_marketo_13.png)
   
    d. 勾選 [需要 SSO] 選項，並 [儲存]設定。
   
    ![設定單一登入](./media/marketo-tutorial/tutorial_marketo_14.png)

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

在本節中，您將把 Marketo 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，依序選取 [企業應用程式]、[所有應用程式] 及 [Marketo]。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，選取 [Marketo] 。

    ![應用程式清單中的 Marketo 連結](common/all-applications.png)

3. 在左側功能表中，選取 [使用者和群組]。

    ![[使用者和群組] 連結](common/users-groups-blade.png)

4. 按一下 [新增使用者] 按鈕，然後在 [新增指派] 對話方塊中，選取 [使用者和群組]。

    ![[新增指派] 窗格](common/add-assign-user.png)

5. 在 [使用者和群組] 對話方塊的 [使用者] 清單中，選取 [Britta Simon]，然後按一下畫面底部的 [選取] 按鈕。

6. 如果您預期使用 SAML 判斷提示中的任何角色值，請在 [選取角色] 對話方塊的清單中選取適當使用者角色，然後按一下畫面底部的 [選取] 按鈕。

7. 在 [新增指派] 對話方塊中，按一下 [指派] 按鈕。

### <a name="create-marketo-test-user"></a>建立 Marketo 測試使用者

在本節中，您要在 Marketo 中建立名為 Britta Simon 的使用者。 遵循以下步驟在 Marketo 平台中建立使用者。

1. 使用管理員認證登入 Marketo 應用程式。

2. 按一下頂端瀏覽窗格中的 [管理] 按鈕。
   
    ![設定單一登入](./media/marketo-tutorial/tutorial_marketo_06.png) 

3. 瀏覽至 [安全性] 功能表，然後按一下 [使用者與角色]
   
    ![設定單一登入](./media/marketo-tutorial/tutorial_marketo_19.png)  

4. 按一下 [使用者] 索引標籤上的 [邀請新使用者]  連結
   
    ![設定單一登入](./media/marketo-tutorial/tutorial_marketo_15.png) 

5. 在 [邀請新使用者] 精靈中填寫下列資訊。
   
    a. 在 [電子郵件] 文字方塊中輸入使用者的電子郵件地址。
   
    ![設定單一登入](./media/marketo-tutorial/tutorial_marketo_16.png)
   
    b. 在 [名字] 文字方塊中輸入名字。
   
    c. 在 [姓氏] 文字方塊中輸入姓氏。
   
    d. 按一下 [下一步] 

6. 在 [權限]  索引標籤中選取 [userRoles]，然後按一下 [下一步]
   
    ![設定單一登入](./media/marketo-tutorial/tutorial_marketo_17.png)
7. 按一下 [傳送] 按鈕，傳送使用者邀請
   
    ![設定單一登入](./media/marketo-tutorial/tutorial_marketo_18.png)

8. 使用者會收到電子郵件通知，他必須按一下連結，並變更密碼才能啟動帳戶。 

### <a name="test-single-sign-on"></a>測試單一登入 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 Marketo 圖格時，應該會自動登入您已設定 SSO 的 Marketo。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什麼是 Azure Active Directory 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

