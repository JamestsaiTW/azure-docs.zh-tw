---
title: 教學課程：Azure Active Directory 與 Qlik Sense Enterprise 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Qlik Sense Enterprise 之間的單一登入。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 8c27e340-2b25-47b6-bf1f-438be4c14f93
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/17/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d5824708f6a99d0222ef5e236758b78a7eedafb
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/26/2019
ms.locfileid: "56882173"
---
# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>教學課程：Azure Active Directory 與 Qlik Sense Enterprise 整合

在本教學課程中，您會了解如何整合 Qlik Sense Enterprise 與 Azure Active Directory (Azure AD)。
Qlik Sense Enterprise 與 Azure AD 整合提供下列優點：

* 您可以在 Azure AD 中控制可存取 Qlik Sense Enterprise 的人員。
* 您可以讓使用者使用其 Azure AD 帳戶自動登入 Qlik Sense Enterprise (單一登入)。
* 您可以在 Azure 入口網站中集中管理您的帳戶。

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱 [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。
如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>必要條件

若要設定 Azure AD 與 Qlik Sense Enterprise 整合，您需要下列項目：

* Azure AD 訂用帳戶。 如果您沒有 Azure AD 環境，您可以在[這裡](https://azure.microsoft.com/pricing/free-trial/)取得一個月的試用帳戶
* 已啟用 Qlik Sense Enterprise 單一登入的訂用帳戶

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD 單一登入。

* Qlik Sense Enterprise 支援 **SP** 起始的 SSO

## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>從資源庫新增 Qlik Sense Enterprise

若要設定將 Qlik Sense Enterprise 整合到 Azure AD 中，您需要從資源庫將 Qlik Sense Enterprise 新增到受控 SaaS 應用程式清單。

**若要從資源庫新增 Qlik Sense Enterprise，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，按一下 [Azure Active Directory] 圖示。

    ![Azure Active Directory 按鈕](common/select-azuread.png)

2. 瀏覽至 [企業應用程式]，然後選取 [所有應用程式] 選項。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

3. 若要新增新的應用程式，請按一下對話方塊頂端的 [新增應用程式] 按鈕。

    ![新增應用程式按鈕](common/add-new-app.png)

4. 在搜尋方塊中，輸入 **Qlik Sense Enterprise**，從結果面板中選取 [Qlik Sense Enterprise]，然後按一下 [新增] 按鈕以新增應用程式。

     ![結果清單中的 Qlik Sense Enterprise](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

在本節中，您會以名為 **Britta Simon** 的測試使用者身分，使用 Qlik Sense Enterprise 設定及測試 Azure AD 單一登入。
若要讓單一登入能夠運作，必須建立 Azure AD 使用者與 Qlik Sense Enterprise 中相關使用者之間的連結關聯性。

若要設定及測試與 Qlik Sense Enterprise 搭配運作的 Azure AD 單一登入，您需要完成下列構成要素：

1. **[設定 Azure AD 單一登入](#configure-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[設定 Qlik Sense Enterprise 單一登入](#configure-qlik-sense-enterprise-single-sign-on)** - 在應用程式端設定單一登入設定。
3. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[建立 Qlik Sense Enterprise 測試使用者](#create-qlik-sense-enterprise-test-user)** - 使 Qlik Sense Enterprise 中對應的 Britta Simon 連結到該使用者在 Azure AD 中的代表項目。
6. **[測試單一登入](#test-single-sign-on)**，驗證組態是否能運作。

### <a name="configure-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入。

若要使用 Qlik Sense Enterprise 設定 Azure AD 單一登入，請執行下列步驟：

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [Qlik Sense Enterprise] 應用程式整合頁面上，選取 [單一登入]。

    ![設定單一登入連結](common/select-sso.png)

2. 在 [選取單一登入方法] 對話方塊中，選取 [SAML/WS-Fed] 模式以啟用單一登入。

    ![單一登入選取模式](common/select-saml-option.png)

3. 在 [以 SAML 設定單一登入] 頁面上，按一下 [編輯] 圖示以開啟 [基本 SAML 設定] 對話方塊。

    ![編輯基本 SAML 組態](common/edit-urls.png)

4. 在 [基本 SAML 組態] 區段上，執行下列步驟：

    ![Qlik Sense Enterprise 網域與 URL 單一登入資訊](common/sp-identifier-reply.png)

    a. 在 [登入 URL] 文字方塊中，使用下列模式輸入 URL︰ `https://<Qlik Sense Fully Qualifed Hostname>:4443/azure/hub`

    b. 在 [識別碼] 文字方塊中，使用下列模式輸入 URL：
    | |
    |--|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qlikpoc.com`|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qliksense.com`|
    | |

    c. 在 [回覆 URL] 文字方塊中，以下列模式輸入 URL：

    `https://<Qlik Sense Fully Qualifed Hostname>:4443/samlauthn/`

    > [!NOTE]
    > 這些都不是真正的值。 請使用實際的「登入 URL」、「識別碼」和「回覆 URL」來更新這些值 (本教學課程稍後會說明)，或連絡 [Qlik Sense Enterprise 用戶端支援小組](https://www.qlik.com/us/services/support)以取得這些值。

5. 在 [以 SAML 設定單一登入] 頁面的 [SAML 簽署憑證] 區段中，按一下 [下載] 以依據您的需求從指定選項下載**同盟中繼資料 XML**，並儲存在您的電腦上。

    ![憑證下載連結](common/metadataxml.png)

### <a name="configure-qlik-sense-enterprise-single-sign-on"></a>設定 Qlik Sense Enterprise 單一登入

1. 準備同盟中繼資料 XML 檔案，以便將該檔案上傳至 Qlik Sense 伺服器。

    > [!NOTE]
    > 將 IdP 中繼資料上傳至 Qlik Sense 伺服器之前，必須先編輯此檔案才能移除資訊，以確保 Azure AD 與 Qlik Sense 伺服器之間的運作正常。

    ![QlikSense][qs24]

    a. 在文字編輯器中開啟從 Azure 入口網站下載的 FederationMetaData.xml 檔案。

    b. 搜尋 **RoleDescriptor** 值。  共會有四個項目 (兩對開頭和結尾元素標籤)。

    c. 從檔案中刪除 RoleDescriptor 標籤與其間的所有資訊。

    d. 將此檔案存放在附近，以便稍後用於本文件。

2. 以可建立虛擬 Proxy 組態的使用者身分，瀏覽至 Qlik Sense Qlik 管理主控台 (QMC)。

3. 在 QMC 中，按一下 [虛擬Proxy] 功能表項目。

    ![QlikSense][qs6]

4. 按一下畫面底部的 [新建] 按鈕。

    ![QlikSense][qs7]

5. 虛擬 Proxy 編輯畫面隨即出現。  畫面右邊的功能表會顯示組態選項。

    ![QlikSense][qs9]

6. 勾選 [識別] 功能表選項後，輸入 Azure 虛擬 Proxy 組態的識別資訊。

    ![QlikSense][qs8]  

    a. [說明] 欄位是虛擬 Proxy 組態的易記名稱。  輸入說明的值。

    b. [前置詞] 欄位可識別虛擬 Proxy 端點，以便連接到採用 Azure AD 單一登入的 Qlik Sense。  輸入此虛擬 Proxy 的獨特前置詞名稱。

    c. [工作階段閒置逾時 (分鐘)] 是指透過此虛擬 Proxy 連接的逾時值。

    d. [工作階段 cookie 標頭名稱] 是 cookie 名稱，用於儲存使用者在成功驗證後收到的 Qlik Sense 工作階段識別碼。  此名稱必須是唯一的。

7. 按一下 [驗證] 功能表選項，讓它立即可見。  [驗證] 畫面隨即出現。

    ![QlikSense][qs10]

    a. [匿名存取模式] 下拉式清單可讓您決定匿名使用者是否可透過虛擬 Proxy 存取 Qlik Sense。  預設選項是 [沒有匿名使用者]。

    b. [驗證方法] 下拉式清單可讓您決定虛擬 Proxy 將使用的驗證配置。  從下拉式清單選取 [SAML]。  此時會顯示更多選項。

    c. 在 [SAML 主機 URI] 欄位中，輸入使用者必須輸入才可透過此 SAML 虛擬 Proxy 存取 Qlik Sense 的主機名稱。  主機名稱是 Qlik Sense 伺服器的 URI。

    d. 在 [SAML 實體識別碼] 中，輸入與 [SAML 主機 URI] 欄位相同的值。

    e. [SAML IdP 中繼資料] 是稍早在 [從 Azure AD 組態編輯同盟中繼資料] 區段中編輯的檔案。  **上傳 IdP 中繼資料之前，必須先編輯此檔案**才能移除資訊，以確保 Azure AD 與 Qlik Sense 伺服器之間的運作正常。  **如果檔案尚未進行編輯，請參閱上述指示。**  如果檔案已經過編輯，請按一下 [瀏覽] 按鈕，然後選取已編輯的中繼資料檔案，將它上傳至虛擬 Proxy 組態。

    f. 輸入 SAML 屬性的屬性名稱或結構描述參考，代表 Azure AD 將傳送至 Qlik Sense 伺服器的 **UserID**。  在設定後的 Azure 應用程式畫面中可取得結構描述參考資訊。  若要使用名稱屬性，請輸入 `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`。

    g. 輸入**使用者目錄**的值，此值會在使用者透過 Azure AD 向 Qlik Sense 伺服器進行驗證時附加至使用者。  硬式編碼值必須以**方括號 []** 括住。  若要使用 Azure AD SAML 判斷提示中傳送的屬性，請在此文字方塊中輸入屬性名稱 (**不需**方括號)。

    h. [SAML 簽章演算法] 可設定虛擬 Proxy 組態的服務提供者 (在此例中是 Qlik Sense 伺服器) 憑證簽署。  如果 Qlik Sense 伺服器使用透過 Microsoft Enhanced RSA 和 AES 密碼編譯提供者產生的受信任憑證，請將 SAML 簽署演算法變更為 **SHA-256**。

    i. [SAML 屬性對應] 區段可讓其他屬性 (如群組) 傳送到 Qlik Sense，以便用於安全性規則。

8. 按一下 [負載平衡] 功能表選項，讓它立即可見。  [負載平衡] 畫面隨即出現。

    ![QlikSense][qs11]

9. 按一下 [新增伺服器節點] 按鈕，選取 Qlik Sense 為了達到負載平衡而會將工作階段傳送至的引擎節點，然後按一下 [新增] 按鈕。

    ![QlikSense][qs12]

10. 按一下 [進階] 功能表選項，讓它立即可見。 [進階] 畫面隨即出現。

    ![QlikSense][qs13]

    [主機允許清單] 會識別在連接到 Qlik Sense 伺服器時所接受的主機名稱。  **輸入連接到 Qlik Sense 伺服器時使用者會指定的主機名稱。** 主機名稱是與 SAML 主機 uri 相同的值 (不含 https://)。

11. 按一下 [套用] 按鈕。

    ![QlikSense][qs14]

12. 按一下 [確定] 以接受警告訊息，該訊息指出將重新啟動連結至虛擬 Proxy 的 Proxy。

    ![QlikSense][qs15]

13. 畫面右側會出現 [相關聯的項目] 功能表。  按一下 [Proxy] 功能表選項。

    ![QlikSense][qs16]

14. Proxy 畫面隨即出現。  按一下底部的 [連結] 按鈕，將 Proxy 連結到虛擬 Proxy。

    ![QlikSense][qs17]

15. 選取將會支援此虛擬 Proxy 連線的 Proxy 節點，然後按一下 [連結] 按鈕。  連結之後，Proxy 會列在相關聯的 Proxy 之下。

    ![QlikSense][qs18]
  
    ![QlikSense][qs19]

16. 大約五到十秒後，[重新整理 QMC] 訊息會出現。  按一下 [重新整理 QMC] 按鈕。

    ![QlikSense][qs20]

17. 當 QMC 重新整理時，按一下 [虛擬 Proxy]  功能表項目。 新的 SAML 虛擬 Proxy 項目會列在螢幕上的表格中。  按一下此虛擬 Proxy 項目。

    ![QlikSense][qs51]

18. 畫面底部的 [下載 SP 中繼資料] 按鈕將會啟用。  按一下 [下載 SP 中繼資料] 按鈕，將中繼資料儲存至檔案。

    ![QlikSense][qs52]

19. 開啟 SP 中繼資料檔案。  觀察 **entityID** 項目和 **AssertionConsumerService** 項目。  這些值相當於 Azure AD 應用程式組態中的 [識別碼]、[登入 URL] 和 [回覆 URL]。 如果這些值不相符，請將其貼在 Azure AD 應用程式組態中的 [Qlik Sense Enterprise 網域與 URL] 區段，然後您應該在 Azure AD App 組態精靈中取代這些值。

    ![QlikSense][qs53]

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

在本節中，您會將 Qlik Sense Enterprise 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，依序選取 [企業應用程式]、[所有應用程式] 和 [Qlik Sense Enterprise]。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，輸入並選取 [Qlik Sense Enterprise] 。

    ![應用程式清單中的 Qlik Sense Enterprise 連結](common/all-applications.png)

3. 在左側功能表中，選取 [使用者和群組]。

    ![[使用者和群組] 連結](common/users-groups-blade.png)

4. 按一下 [新增使用者] 按鈕，然後在 [新增指派] 對話方塊中，選取 [使用者和群組]。

    ![[新增指派] 窗格](common/add-assign-user.png)

5. 在 [使用者和群組] 對話方塊的 [使用者] 清單中，選取 [Britta Simon]，然後按一下畫面底部的 [選取] 按鈕。

6. 如果您預期使用 SAML 判斷提示中的任何角色值，請在 [選取角色] 對話方塊的清單中選取適當使用者角色，然後按一下畫面底部的 [選取] 按鈕。

7. 在 [新增指派] 對話方塊中，按一下 [指派] 按鈕。

### <a name="create-qlik-sense-enterprise-test-user"></a>建立 Qlik Sense Enterprise 測試使用者

在本節中，您要在 Qlik Sense Enterprise 中建立名為 Britta Simon 的使用者。 請與  [Qlik Sense Enterprise 支援小組](https://www.qlik.com/us/services/support)合作，在 JQlik Sense Enterprise 平台中新增使用者。 您必須先建立和啟動使用者，然後才能使用單一登入。

### <a name="test-single-sign-on"></a>測試單一登入 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [Qlik Sense Enterprise] 圖格時，應該會自動登入您設定 SSO 的 Qlik Sense Enterprise。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什麼是 Azure Active Directory 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

[qs6]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs24]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs51]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png

