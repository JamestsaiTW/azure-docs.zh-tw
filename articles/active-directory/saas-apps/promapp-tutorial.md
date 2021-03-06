---
title: 教學課程：Azure Active Directory 與 Promapp 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Promapp 之間的單一登入。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 418d0601-6e7a-4997-a683-73fa30a2cfb5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 758dc65b4177192047ff1b092899608b04211221
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/13/2019
ms.locfileid: "56192824"
---
# <a name="tutorial-azure-active-directory-integration-with-promapp"></a>教學課程：Azure Active Directory 與 Promapp 整合

在本教學課程中，您會了解如何整合 Promapp 與 Azure Active Directory (Azure AD)。

Promapp 與 Azure AD 整合提供下列優點：

- 您可以在 Azure AD 中控制可存取 Promapp 的人員
- 您可以讓使用者使用他們的 Azure AD 帳戶自動登入 Promapp (單一登入)
- 您可以在 Azure 入口網站中集中管理您的帳戶

如果您想要了解有關 SaaS 應用程式與 Azure AD 之整合的更多詳細資料，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>必要條件

若要設定與 Promapp 的 Azure AD 整合，您需要下列項目：

- Azure AD 訂用帳戶
- 啟用 Promapp 單一登入的訂用帳戶

> [!NOTE]
> 若要測試本教學課程中的步驟，我們不建議使用生產環境。

若要測試本教學課程中的步驟，您應該遵循這些建議：

- 除非必要，否則請勿使用生產環境。
- 如果您沒有 Azure AD 試用環境，您可以在 [這裡](https://azure.microsoft.com/pricing/free-trial/)取得一個月試用。

## <a name="scenario-description"></a>案例描述
在本教學課程中，您會在測試環境中測試 Azure AD 單一登入。 本教學課程中說明的案例由二項主要的基本工作組成：

1. 從資源庫加入 Promapp
1. 設定並測試 Azure AD 單一登入

## <a name="adding-promapp-from-the-gallery"></a>從資源庫加入 Promapp
若要設定 Promapp 與 Azure AD 整合，您需要從資源庫將 Promapp 加入到受控 SaaS 應用程式清單。

**若要從資源庫加入 Promapp，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，按一下 [Azure Active Directory] 圖示。 

    ![Active Directory][1]

1. 瀏覽至 [企業應用程式]。 然後移至 [所有應用程式]。

    ![[應用程式]][2]
    
1. 若要新增新的應用程式，請按一下對話方塊頂端的 [新增應用程式] 按鈕。

    ![[應用程式]][3]

1. 在搜尋方塊中，輸入 **Promapp**。

    ![建立 Azure AD 測試使用者](./media/promapp-tutorial/tutorial_promapp_search.png)

1. 在結果面板中，選取 [Promapp]，然後按一下 [新增] 按鈕以新增應用程式。

    ![建立 Azure AD 測試使用者](./media/promapp-tutorial/tutorial_promapp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>設定並測試 Azure AD 單一登入
在本節中，您會以名為 "Britta Simon" 的測試使用者身分，設定及測試與 Promapp 搭配運作的 Azure AD 單一登入。

若要讓單一登入運作，Azure AD 必須知道 Promapp 與 Azure AD 中互相對應的使用者。 換句話說，必須在 Azure AD 使用者和 Promapp 中的相關使用者之間，建立連結關聯性。

在 Promapp 中，將 Azure AD 中**使用者名稱**的值指派為 **Username** 的值，以建立連結關聯性。

若要設定及測試對 Promapp 的 Azure AD 單一登入，您需要完成下列建置組塊：

1. **[設定 Azure AD 單一登入](#configuring-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
1. **[建立 Azure AD 測試使用者](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
1. **[建立 Promapp 測試使用者](#creating-a-promapp-test-user)** - 使 Promapp 中對應的 Britta Simon 連結到該使用者在 Azure AD 中的代表項目。
1. **[指派 Azure AD 測試使用者](#assigning-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
1. **[Testing Single Sign-On](#testing-single-sign-on)** - 驗證組態是否能運作。

### <a name="configuring-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入，並在您的 Promapp 應用程式中設定單一登入。

**若要使用 Promapp 設定 Azure AD 單一登入，請執行下列步驟：**

1. 在 Azure 入口網站的 [Promapp] 應用程式整合頁面上，按一下 [單一登入]。

    ![設定單一登入][4]

1. 在 [單一登入] 對話方塊上，於 [模式] 選取 [SAML 登入]，以啟用單一登入。
 
    ![設定單一登入](./media/promapp-tutorial/tutorial_promapp_samlbase.png)

1. 如果您想要以 **IDP** 起始模式設定應用程式，請在 [Promapp 網域和 URL] 區段上執行下列步驟：

    ![設定單一登入](./media/promapp-tutorial/tutorial_promapp_url.png)

    a. 在 [識別碼] 文字方塊中，使用下列模式輸入 URL：
    
    | |
    |--|
    | `https://go.promapp.com/TENANTNAME/`|
    | `https://au.promapp.com/TENANTNAME/`|
    | `https://us.promapp.com/TENANTNAME/`|
    | `https://eu.promapp.com/TENANTNAME/`|
    | `https://ca.promapp.com/TENANTNAME/`|
    
    > [!NOTE] 
    > 目前僅針對服務起始的驗證 (例如移至 Promapp URL 會起始驗證程序)，設定 Azure AD 與 Promapp 整合。 然而 [回覆 URL] 是必要欄位。
    
    b. 在 **[回覆 URL]** 文字方塊中，以下列模式輸入 URL：`https://DOMAINNAME.promapp.com/azuread/saml/authenticate.aspx`

1. 如果您想要以 **SP** 起始模式設定應用程式，請勾選 [顯示進階 URL 設定]，然後執行下列步驟：

    ![設定單一登入](./media/promapp-tutorial/tutorial_promapp_url1.png)

    在 [登入 URL] 文字方塊中，使用下列模式輸入 URL︰ `https://DOMAINNAME.promapp.com/TENANTNAME/saml/authenticate`

    > [!NOTE] 
    > 這些都不是真正的值。 使用實際的「單一登入 URL」、「識別碼」及「回覆 URL」來更新這些值。 請連絡 [Promapp 客戶支援小組](https://www.promapp.com/about-us/contact-us/)以取得這些值。

1. 在 [SAML 簽署憑證] 區段上，按一下 [憑證 (Base64)]，然後將憑證檔案儲存在您的電腦上。

    ![設定單一登入](./media/promapp-tutorial/tutorial_promapp_certificate.png) 

1. 按一下 [儲存]  按鈕。

    ![設定單一登入](./media/promapp-tutorial/tutorial_general_400.png)

1. 在 [Promapp 組態] 區段上，按一下 [設定 Promapp] 以開啟 [設定登入] 視窗。 從 [快速參考] 區段中複製 [SAML 單一登入服務 URL]。

    ![設定單一登入](./media/promapp-tutorial/tutorial_promapp_configure.png) 

1. 以系統管理員身分登入您的 Promapp 公司網站。 

1. 在頂端的功能表中，按一下 [系統管理員] 。 
   
    ![Azure AD 單一登入][12]

1. 按一下 [設定] 。 
   
    ![Azure AD 單一登入][13]

1. 在 [安全性]  對話方塊上，執行下列步驟：
   
    ![Azure AD 單一登入][14]
    
    a. 將您從 Azure 入口網站複製的 [SAML 單一登入服務 URL]，貼到 [SSO 登入 URL] 文字方塊中。
    
    b. 針對 [SSO - 單一登入模式]，選取 [選擇性]，然後按一下 [儲存]。

    > [!NOTE]
    > [選擇性] 模式僅供測試而已。 一旦滿意設定，請選取 [必要] 模式，強制所有使用者使用 Azure AD 進行驗證。

    c. 在記事本中開啟下載的憑證，複製憑證的內容但不包含第一行 (-----**BEGIN CERTIFICATE**-----) 和最後一行 (-----**END CERTIFICATE**-----)，將它貼到 [SSO-x.509 憑證] 文字方塊中，然後按一下 [儲存]。
        
> [!TIP]
> 現在，當您設定此應用程式時，在 [Azure 入口網站](https://portal.azure.com)內即可閱讀這些指示的簡要版本！  從 [Active Directory] > [企業應用程式] 區段新增此應用程式之後，只要按一下 [單一登入] 索引標籤，即可透過底部的 [組態] 區段存取內嵌的文件。 您可以從以下連結閱讀更多有關內嵌文件功能的資訊：[Azure AD 內嵌文件]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者
本節的目標是要在 Azure 入口網站中建立一個名為 Britta Simon 的測試使用者。

![建立 Azure AD 使用者][100]

**若要在 Azure AD 中建立測試使用者，請執行下列步驟：**

1. 在 **Azure 入口網站**的左方瀏覽窗格中，按一下 [Azure Active Directory] 圖示。

    ![建立 Azure AD 測試使用者](./media/promapp-tutorial/create_aaduser_01.png) 

1. 若要顯示使用者清單，請移至 [使用者和群組]，然後按一下 [所有使用者]。
    
    ![建立 Azure AD 測試使用者](./media/promapp-tutorial/create_aaduser_02.png) 

1. 若要開啟 [使用者] 對話方塊，按一下對話方塊頂端的 [新增]。
 
    ![建立 Azure AD 測試使用者](./media/promapp-tutorial/create_aaduser_03.png) 

1. 在 [使用者]  對話頁面上，執行下列步驟：
 
    ![建立 Azure AD 測試使用者](./media/promapp-tutorial/create_aaduser_04.png) 

    a. 在 [名稱] 文字方塊中，輸入 **BrittaSimon**。

    b. 在 [使用者名稱] 文字方塊中，輸入 BrittaSimon 的**電子郵件地址**。

    c. 選取 [顯示密碼] 並記下 [密碼] 的值。

    d. 按一下頁面底部的 [新增] 。
 
### <a name="creating-a-promapp-test-user"></a>建立 Promapp 測試使用者

Promapp 應用程式支援 Just-in-Time 佈建。 這表示，在使用存取面板嘗試存取應用程式期間，必要時會自動建立使用者帳戶。

### <a name="assigning-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會將 Promapp 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

![指派使用者][200] 

**若要將 Britta Simon 指派到 Promapp，請執行以下步驟：**

1. 在 Azure 入口網站中，開啟應用程式檢視，接著瀏覽至目錄檢視並移至 [企業應用程式]，然後按一下 [所有應用程式]。

    ![指派使用者][201] 

1. 在應用程式清單中，選取 **Promapp**。

    ![設定單一登入](./media/promapp-tutorial/tutorial_promapp_app.png) 

1. 在左側功能表中，按一下 [使用者和群組]。

    ![指派使用者][202] 

1. 按一下 [新增] 按鈕。 然後選取 [新增指派] 對話方塊上的 [使用者和群組]。

    ![指派使用者][203]

1. 在 [使用者和群組] 對話方塊上，選取 [使用者] 清單中的 [Britta Simon]。

1. 按一下 [使用者和群組] 對話方塊上的 [選取] 按鈕。

1. 按一下 [新增指派] 對話方塊上的 [指派] 按鈕。
    
### <a name="testing-single-sign-on"></a>測試單一登入

若要以 **SP** 起始模式測試您的應用程式，您將需要起始來自 Promapp 網站的驗證。 在已啟用 [選擇性] 模式的 [登入] 頁面上，按一下 [使用單一登入進行登入] 按鈕，即可完成此作業。

## <a name="additional-resources"></a>其他資源

* [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](tutorial-list.md)
* [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/promapp-tutorial/tutorial_general_01.png
[2]: ./media/promapp-tutorial/tutorial_general_02.png
[3]: ./media/promapp-tutorial/tutorial_general_03.png
[4]: ./media/promapp-tutorial/tutorial_general_04.png
[12]: ./media/promapp-tutorial/tutorial_promapp_05.png
[13]: ./media/promapp-tutorial/tutorial_promapp_06.png
[14]: ./media/promapp-tutorial/tutorial_promapp_07.png

[100]: ./media/promapp-tutorial/tutorial_general_100.png

[200]: ./media/promapp-tutorial/tutorial_general_200.png
[201]: ./media/promapp-tutorial/tutorial_general_201.png
[202]: ./media/promapp-tutorial/tutorial_general_202.png
[203]: ./media/promapp-tutorial/tutorial_general_203.png

