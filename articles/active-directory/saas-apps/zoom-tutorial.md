---
title: 教學課程：Azure Active Directory 與 Zoom 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Zoom 之間的單一登入。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 0ebdab6c-83a8-4737-a86a-974f37269c31
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/05/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4175b626040f5fcb7ec157120f19b89508e67239
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/26/2019
ms.locfileid: "56872602"
---
# <a name="tutorial-azure-active-directory-integration-with-zoom"></a>教學課程：Azure Active Directory 與 Zoom 整合

在本教學課程中，您將了解如何整合 Zoom 與 Azure Active Directory (Azure AD)。
Zoom 與 Azure AD 整合提供下列優點：

* 您可以在 Azure AD 中控制可存取 Zoom 的人員。
* 您可以讓使用者使用他們的 Azure AD 帳戶自動登入 Zoom (單一登入)。
* 您可以在 Azure 入口網站中集中管理您的帳戶。

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱 [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。
如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>必要條件

若要設定 Azure AD 與 Zoom 整合，您需要下列項目：

* Azure AD 訂用帳戶。 如果您沒有 Azure AD 環境，您可以在[這裡](https://azure.microsoft.com/pricing/free-trial/)取得一個月的試用帳戶
* 已啟用 Zoom 單一登入的訂用帳戶

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD 單一登入。

* Zoom 支援 **SP** 起始的 SSO

## <a name="adding-zoom-from-the-gallery"></a>從資源庫新增 Zoom

若要設定將 Zoom 整合到 Azure AD 中，您需要從資源庫將 Zoom 新增到受控 SaaS 應用程式清單。

**若要從資源庫新增 Zoom，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，按一下 [Azure Active Directory] 圖示。

    ![Azure Active Directory 按鈕](common/select-azuread.png)

2. 瀏覽至 [企業應用程式]，然後選取 [所有應用程式] 選項。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

3. 若要新增新的應用程式，請按一下對話方塊頂端的 [新增應用程式] 按鈕。

    ![新增應用程式按鈕](common/add-new-app.png)

4. 在搜尋方塊中，輸入 **Zoom**，從結果面板中選取 [Zoom]，然後按一下 [新增] 按鈕以新增應用程式。

     ![結果清單中的 Zoom](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

在本節中，您會以名為 **Britta Simon** 的測試使用者身分，使用 Zoom 設定和測試 Azure AD 單一登入。
若要讓單一登入能夠運作，必須建立 Azure AD 使用者與 Zoom 中相關使用者之間的連結關聯性。

若要設定及測試與 Zoom 搭配運作的 Azure AD 單一登入，您需要完成下列構成要素：

1. **[設定 Azure AD 單一登入](#configure-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[設定 Zoom 單一登入](#configure-zoom-single-sign-on)** - 在應用程式端設定單一登入設定。
3. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[建立 Zoom 測試使用者](#create-zoom-test-user)** - 使 Zoom 中對應的 Britta Simon 連結到該使用者在 Azure AD 中的代表項目。
6. **[測試單一登入](#test-single-sign-on)**，驗證組態是否能運作。

### <a name="configure-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入。

若要使用 Zoom 設定 Azure AD 單一登入，請執行下列步驟：

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [Zoom] 應用程式整合頁面上，選取 [單一登入]。

    ![設定單一登入連結](common/select-sso.png)

2. 在 [選取單一登入方法] 對話方塊中，選取 [SAML/WS-Fed] 模式以啟用單一登入。

    ![單一登入選取模式](common/select-saml-option.png)

3. 在 [以 SAML 設定單一登入] 頁面上，按一下 [編輯] 圖示以開啟 [基本 SAML 設定] 對話方塊。

    ![編輯基本 SAML 組態](common/edit-urls.png)

4. 在 [基本 SAML 組態] 區段上，執行下列步驟：

    ![Zoom 網域與 URL 單一登入資訊](common/sp-identifier.png)

    a. 在 [登入 URL] 文字方塊中，使用下列模式輸入 URL：`https://<companyname>.zoom.us`

    b. 在 [識別碼 (實體識別碼)] 文字方塊中，使用下列模式輸入 URL：`<companyname>.zoom.us`

    > [!NOTE]
    > 這些都不是真正的值。 請使用實際的「登入 URL」及「識別碼」來更新這些值。 請連絡 [Zoom 客戶支援小組](https://support.zoom.us/hc/en-us)以取得這些值。 您也可以參考 Azure 入口網站中**基本 SAML 組態**區段所示的模式。

5. Zoom 應用程式需要特定格式的 SAML 判斷提示。 設定此應用程式的下列宣告。 您可以在應用程式整合頁面的 [使用者屬性] 區段中，管理這些屬性的值。 在 [以 SAML 設定單一登入] 頁面上，按一下 [編輯] 按鈕以開啟 [使用者屬性] 對話方塊。

    ![映像](common/edit-attribute.png)

6. 在 [使用者屬性] 對話方塊的 [使用者宣告] 區段中，如上圖所示設定 SAML 權杖屬性，然後執行下列步驟：
    
    | Name | 命名空間  |  來源屬性|
    | ---------------| --------------- | --------- |
    | 電子郵件地址  | user.mail  | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/mail |
    | 名字  | user.givenname  | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname |
    | 姓氏  | user.surname  | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname |
    | 電話號碼  | user.telephonenumber  | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/phone |
    | department  | user.department  | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/department |
    | 角色 |    user.assignedrole |http://schemas.xmlsoap.org/ws/2005/05/identity/claims/role |

    > [!NOTE]
    > 請按一下[這裡](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)，以了解如何在 Azure AD 中設定角色

    a. 按一下 [新增宣告] 以開啟 [管理使用者宣告] 對話方塊。

    ![映像](common/new-save-attribute.png)

    ![映像](common/new-attribute-details.png)

    b. 在 [名稱] 文字方塊中，輸入該資料列所顯示的屬性名稱。

    c. 選取 [來源] 作為 [屬性]。

    d. 在 [來源屬性] 清單中，輸入該資料列所顯示的屬性值。

    e. 按一下 [確定]。

    f. 按一下 [檔案] 。

    > [!NOTE]
    > Zoom 可能需要 SAML 承載中的群組宣告，因此如果您建立了任何群組，請連絡 [Zoom 客戶支援小組](https://support.zoom.us/hc/en-us)並提供群組資訊，讓他們也能在那一端設定此群組資訊。 您也必須將物件識別碼提供給 [Zoom 客戶支援小組](https://support.zoom.us/hc/en-us)，讓他們可在那一端設定。 請依照[文件](https://support.zoom.us/hc/en-us/articles/115005887566)指示取得物件識別碼。

7. 在 [以 SAML 設定單一登入] 頁面的 [SAML 簽署憑證] 區段中，按一下 [下載]，以依據您的需求從指定選項下載 [憑證 (Base64)]，並儲存在您的電腦上。

    ![憑證下載連結](common/certificatebase64.png)

8. 在 [設定 Zoom] 區段上，依據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

    a. 登入 URL

    b. Azure AD 識別碼

    c. 登出 URL

### <a name="configure-zoom-single-sign-on"></a>設定 Zoom 單一登入

1. 在不同的 Web 瀏覽器視窗中，以系統管理員身分登入您的 Zoom 公司網站。

2. 按一下 [單一登入]  索引標籤。

    ![[單一登入] 索引標籤](./media/zoom-tutorial/ic784700.png "單一登入")

3. 按一下 [安全性控制] 索引標籤，然後移至 [單一登入] 設定。

4. 在 [單一登入] 區段中，執行下列步驟：

    ![[單一登入] 區段](./media/zoom-tutorial/ic784701.png "單一登入")

    a. 在 [登入頁面 URL] 文字方塊中，貼上您從 Azure 入口網站複製的**登入 URL** 值。

    b. 在 [登出頁面 URL] 文字方塊中，貼上您從 Azure 入口網站複製的**登出 URL** 值。

    c. 在記事本中開啟您的 base-64 編碼的憑證，將其內容複製到您的剪貼簿，然後貼到 [識別提供者憑證]  文字方塊中。

    d. 在 [簽發者] 文字方塊中，貼上您從 Azure 入口網站複製的 **Azure AD 識別碼**值。 

    e. 按一下 [檔案] 。

    > [!NOTE]
    > 如需詳細資訊，請瀏覽 Zoom 文件[https://zoomus.zendesk.com/hc/articles/115005887566](https://zoomus.zendesk.com/hc/articles/115005887566)

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

在本節中，您會將 Zoom 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，依序選取 [企業應用程式]、[所有應用程式] 及 [Zoom]。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，輸入 **Zoom** 並加以選取。

    ![應用程式清單中的 Zoom 連結](common/all-applications.png)

3. 在左側功能表中，選取 [使用者和群組]。

    ![[使用者和群組] 連結](common/users-groups-blade.png)

4. 按一下 [新增使用者] 按鈕，然後在 [新增指派] 對話方塊中，選取 [使用者和群組]。

    ![[新增指派] 窗格](common/add-assign-user.png)

5. 在 [使用者和群組] 對話方塊的 [使用者] 清單中，選取 [Britta Simon]，然後按一下畫面底部的 [選取] 按鈕。

6. 如果您預期使用 SAML 判斷提示中的任何角色值，請在 [選取角色] 對話方塊的清單中選取適當使用者角色，然後按一下畫面底部的 [選取] 按鈕。

7. 在 [新增指派] 對話方塊中，按一下 [指派] 按鈕。

### <a name="create-zoom-test-user"></a>建立 Zoom 測試使用者

若要讓 Azure AD 使用者可以登入 Zoom，必須將他們佈建到 Zoom。 Zoom 需以手動的方式佈建。

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>若要佈建使用者帳戶，請執行下列步驟：

1. 以系統管理員身分登入您的 **Zoom** 公司網站。

2. 按一下 [帳戶管理] 索引標籤，再按一下 [使用者管理]。

3. 在 [使用者管理] 區段中按一下 [新增使用者] 。

    ![使用者管理](./media/zoom-tutorial/ic784703.png "使用者管理")

4. 在 [新增使用者]  頁面上，執行下列步驟：

    ![新增使用者](./media/zoom-tutorial/ic784704.png "新增使用者")

    a. 選取 [基本] 做為 [使用者類型]。

    b. 在 [電子郵件]  文字方塊中，輸入您要佈建之有效 Azure AD 帳戶的電子郵件地址。

    c. 按一下 [新增] 。

> [!NOTE]
> 您可以使用任何其他的 Zoom 使用者帳戶建立工具，或 Zoom 提供的 API，以佈建 Azure Active Directory 使用者帳戶。

### <a name="test-single-sign-on"></a>測試單一登入

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [Zoom] 圖格時，應該會自動登入您設定 SSO 的 Zoom。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什麼是 Azure Active Directory 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
