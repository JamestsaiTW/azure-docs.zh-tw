---
title: 新功能 版本資訊 - Azure Active Directory | Microsoft Docs
description: 了解 Azure Active Directory 的新功能，例如最新的版本資訊、已知問題、錯誤 (bug) 修正、已被取代的功能，以及即將進行的變更。
services: active-directory
author: eross-msft
manager: daveba
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 02/28/2019
ms.author: lizross
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: eda145f43c9268e5f6b291a767ed51249804f87d
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2019
ms.locfileid: "58286443"
---
# <a name="whats-new-in-azure-active-directory"></a>Azure Active Directory 的新增功能？

>獲知時返回此頁面的更新複製並貼上此 URL:`https://docs.microsoft.com/api/search/rss?search=%22release+notes+for+azure+AD%22&locale=en-us`到您![RSS 摘要讀取器圖示](./media/whats-new/feed-icon-16x16.png)摘要讀取器。

Azure AD 會持續不斷進行改進。 為了讓您隨時掌握最新的開發訊息，本文提供下列相關資訊：

- 最新版本
- 已知問題
- 錯誤修正
- 已被取代的功能
- 方案變更

此頁面會每月更新，因此請定期瀏覽。 如果想要尋找超過 6 個月的項目，請至 [Azure Active Directory 的新增功能封存](whats-new-archive.md) (英文)。

---
## <a name="february-2019"></a>2019 年 2 月

### <a name="configurable-azure-ad-saml-token-encryption-public-preview"></a>可設定的 Azure AD SAML 權杖加密 (公開預覽) 

**類型：** 新功能  
**服務類別：** 企業應用程式  
**產品功能：** SSO
 
您現在可以設定任何支援的 SAML 應用程式來接收加密的 SAML 權杖。 當設定並與應用程式，Azure AD 會加密使用公開金鑰取自的憑證儲存在 Azure AD 發出的 SAML 判斷提示。

如需有關如何設定您的 SAML 權杖加密的詳細資訊，請參閱 <<c0> [ 設定 Azure AD SAML 權杖加密](https://docs.microsoft.com/azure/active-directory/manage-apps/howto-saml-token-encryption)。

---

### <a name="create-an-access-review-for-groups-or-apps-using-azure-ad-access-reviews"></a>使用 Azure AD 存取權檢閱建立群組或應用程式的存取權檢閱

**類型：** 新功能  
**服務類別：** 存取權檢閱  
**產品功能：** 控管

您現在可以包含多個群組，或在單一 Azure AD 中的應用程式存取權的群組成員資格或應用程式指派的檢閱。 存取權檢閱與多個群組或應用程式設定使用相同的設定和所有包含的檢閱者會收到通知，在相同的時間。

如需有關如何建立使用 Azure AD 存取權檢閱存取權檢閱，請參閱[建立在 Azure AD 存取權檢閱的群組或應用程式的存取權檢閱](https://docs.microsoft.com/azure/active-directory/governance/create-access-review)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---february-2019"></a>Azure AD 應用程式庫推出新的同盟應用程式 - 2019 年 2 月

**類型：** 新功能  
**服務類別：** 企業應用程式  
**產品功能：** 協力廠商整合
 
在 1 月 2019年中，我們已新增這些 27 的新應用程式具有同盟支援新增至應用程式庫：

[Euromonitor Passport](https://docs.microsoft.com/azure/active-directory/saas-apps/euromonitor-passport-tutorial)， [MindTickle](https://docs.microsoft.com/azure/active-directory/saas-apps/mindtickle-tutorial)，[意外](https://seeforgetest-exxon.azurewebsites.net/Account/create?Length=7)， [AirStack](https://docs.microsoft.com/azure/active-directory/saas-apps/airstack-tutorial)， [Oracle Fusion ERP](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-fusion-erp-tutorial)， [IDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/idrive-tutorial)， [Skyward Qmlativ](https://docs.microsoft.com/azure/active-directory/saas-apps/skyward-qmlativ-tutorial)， [Brightidea](https://docs.microsoft.com/azure/active-directory/saas-apps/brightidea-tutorial)， [AlertOps](https://docs.microsoft.com/azure/active-directory/saas-apps/alertops-tutorial)， [Soloinsight CloudGate SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-tutorial)，權限按一下[Brandfolder](https://docs.microsoft.com/azure/active-directory/saas-apps/brandfolder-tutorial)， [StoregateSmartFile](https://docs.microsoft.com/azure/active-directory/saas-apps/smartfile-tutorial)， [Pexip](https://docs.microsoft.com/azure/active-directory/saas-apps/pexip-tutorial)， [Stormboard](https://docs.microsoft.com/azure/active-directory/saas-apps/stormboard-tutorial)，[地震](https://docs.microsoft.com/azure/active-directory/saas-apps/seismic-tutorial)，[共用夢想](https://www.shareadream.org/how-it-works)， [Bugsnag](https://docs.microsoft.com/azure/active-directory/saas-apps/bugsnag-tutorial)， [webMethods 整合雲端](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial)，[知識隨處 LMS](https://docs.microsoft.com/azure/active-directory/saas-apps/knowledge-anywhere-lms-tutorial)，[OU 校園](https://docs.microsoft.com/azure/active-directory/saas-apps/ou-campus-tutorial)， [Periscope 資料](https://docs.microsoft.com/azure/active-directory/saas-apps/periscope-data-tutorial)， [Netop 入口網站](https://docs.microsoft.com/azure/active-directory/saas-apps/netop-portal-tutorial)， [smartvid.io](https://docs.microsoft.com/azure/active-directory/saas-apps/smartvid.io-tutorial)， [PureCloud 由 Genesys](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-tutorial)， [ClickUp 產能平台](https://docs.microsoft.com/azure/active-directory/saas-apps/clickup-productivity-platform-tutorial)

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。 如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://aka.ms/azureadapprequest)。

---

### <a name="enhanced-combined-mfasspr-registration"></a>增強的合併 MFA/SSPR 註冊

**類型：** 已變更的功能  
**服務類別：** 自助式密碼重設  
**產品功能：** 使用者驗證
 
為了回應客戶的意見反應，我們已增強結合的 MFA/SSPR 註冊預覽版體驗，協助使用者更快速地 MFA 和 SSPR 註冊他們的安全性資訊。 

**若要立即開啟增強的體驗，為您的使用者，請遵循下列步驟：**

1. 身為全域管理員或使用者系統管理員，登入 Azure 入口網站並移至**Azure Active Directory > 使用者設定 > 管理的存取面板的預覽功能設定**。 

2. 在**使用者可以使用預覽功能的註冊和管理的安全性資訊 – 重新整理**選項，請開啟 功能選擇**選取的使用者的群組**或**所有使用者**.

在接下來的幾週，我們將會移除開啟舊結合 MFA/SSPR 註冊預覽版體驗還沒有開啟它的租用戶的能力。

**若要查看您的租用戶，將會移除控制項，請遵循下列步驟：**

1. 身為全域管理員或使用者系統管理員，登入 Azure 入口網站並移至**Azure Active Directory > 使用者設定 > 管理的存取面板的預覽功能設定**。  

2.  如果**可以使用預覽功能註冊和管理的安全性資訊的使用者**選項設定為**無**，選項會移除您的租用戶。

不論是否您先前已開啟舊的結合 MFA/SSPR 註冊預覽與否的使用者體驗，舊的體驗將會關閉在未來日期。 因此，我們強烈建議您儘速移至新的增強體驗。

如需有關增強的註冊體驗的詳細資訊，請參閱[至 Azure AD 的超炫增強功能結合 MFA 和密碼重設註冊體驗](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Cool-enhancements-to-the-Azure-AD-combined-MFA-and-password/ba-p/354271)。

---

### <a name="updated-policy-management-experience-for-user-flows"></a>更新的使用者流程原則管理體驗

**類型：** 已變更的功能  
**服務類別：** B2C - 消費者身分識別管理  
**產品功能：** B2B/B2C

我們已更新使用者流程 （先前稱為、 內建原則） 更輕鬆的原則建立及管理程序。 這個新體驗現在是您的 Azure AD 租用戶的所有預設值。

您可以提供其他意見反應和建議使用笑臉或苦臉中的圖示**傳送意見反應**入口網站畫面上方區域。

如需以新的原則管理體驗的詳細資訊，請參閱[Azure AD B2C 現在有 JavaScript 自訂以及許多新功能](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595)部落格。

---

### <a name="choose-specific-page-element-versions-provided-by-azure-ad-b2c"></a>選擇 Azure AD B2C 所提供的特定網頁元素版本

**類型：** 新功能  
**服務類別：** B2C - 消費者身分識別管理  
**產品功能：** B2B/B2C

您現在可以選擇特定版本的 Azure AD B2C 所提供的頁面元素。 藉由選取特定版本，您可以測試您的更新，再它們出現在頁面上，而且您可以取得可預期的行為。 此外，您可以立即選擇強制執行特定的頁面版本，以允許 JavaScript 的自訂項目。 若要開啟此功能，請前往**屬性**在使用者流程中的頁面。

如需有關選擇特定版本的頁面元素的詳細資訊，請參閱[Azure AD B2C 現在有 JavaScript 自訂以及許多新功能](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595)部落格。

---

### <a name="configurable-end-user-password-requirements-for-b2c-ga"></a>可設定的終端使用者密碼需求 B2C (GA)

**類型：** 新功能  
**服務類別：** B2C - 消費者身分識別管理  
**產品功能：** B2B/B2C

您現在可以特別設定貴組織的密碼複雜性為一般使用者，而不需使用您的原生 Azure AD 密碼原則。 從**屬性**刀鋒視窗中的使用者 （先前稱為內建的原則） 的流程，您可以選擇的密碼複雜性**簡單**或是**強式**，或者您也可以建立**自訂**組的需求。

如需密碼複雜性需求設定的詳細資訊，請參閱[設定 Azure Active Directory B2C 中的密碼複雜度需求](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity)。

---

### <a name="new-default-templates-for-custom-branded-authentication-experiences"></a>自訂品牌化驗證體驗的新預設範本

**類型：** 新功能  
**服務類別：** B2C - 消費者身分識別管理  
**產品功能：** B2B/B2C

您可以使用我們新的預設範本，位於**頁面上配置**刀鋒視窗中，您的使用者流程 （先前稱為內建原則），來建立自訂品牌的使用者的驗證體驗。

如需使用範本的詳細資訊，請參閱[Azure AD B2C 現在有 JavaScript 自訂以及許多新功能](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595)。

---

## <a name="january-2019"></a>2019 年 1 月

### <a name="active-directory-b2b-collaboration-using-one-time-passcode-authentication-public-preview"></a>使用單次密碼驗證 (公開預覽) 的 Active Directory B2B 共同作業

**類型：** 新功能  
**服務類別：** B2B  
**產品功能：** B2B/B2C

對於無法透過如 Azure AD、Microsoft 帳戶 (MSA) 或 Google 同盟等方式驗證的 B2B 來賓使用者，我們已加入單次密碼驗證 (OTP)。 這個新的驗證方法表示來賓使用者不需要建立新的 Microsoft 帳戶。 另一方便，兌換邀請或存取共用的資源時，來賓使用者可以要求臨時代碼來傳送電子郵件地址。 使用此組臨時代碼時，來賓使用者可以繼續登入。

如需詳細資訊，請參閱[電子郵件單次密碼驗證 (預覽版)](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode) 和部落格 [Azure AD 可讓任何帳戶的任何使用者順利共用和共同作業](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-makes-sharing-and-collaboration-seamless-for-any-user/ba-p/325949)。

### <a name="new-azure-ad-application-proxy-cookie-settings"></a>新增 Azure AD 應用程式 Proxy Cookie 設定

**類型：** 新功能  
**服務類別：** 應用程式 Proxy  
**產品功能：** 存取控制

我們引進了三項新的 cookie 設定，以供透過應用程式 Proxy 發佈的應用程式使用：

- **使用僅限 HTTP Cookie。** 在您的應用程式 Proxy 存取和工作階段 Cookie 上設定 **HTTPOnly** 旗標。 開啟此設定可提供額外的安全性優點，例如協助防止透過用戶端指令碼複製或修改 Cookie。 我們建議您開啟這個旗標 (選擇 [是])，以獲得附加好處。

- **使用安全的 Cookie。** 在您的應用程式 Proxy 存取和工作階段 Cookie 上設定 [安全] 旗標。 開啟此設定可提供額外的安全性優點，例如確定 Cookie 只會透過 TLS 安全通道 (例如 HTTPS) 傳輸。 我們建議您開啟這個旗標 (選擇 [是])，以獲得附加好處。

- **使用永續性 Cookie。** 防止存取 Cookie 在關閉 Web 瀏覽器關閉時過期。 這些 Cookie 會在存取權杖的存留期中持續存在。 不過，如果已達到期時間，或是使用者手動刪除 Cookie，便會重設 Cookie。 我們建議您保留預設設定 [否]，僅針對程序間不共用 Cookie 的舊版應用程式開啟此設定。

如需新 Cookie 的詳細資訊，請參閱 [Azure Active Directory 中用來存取內部部署應用程式的 Cookie 設定](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings)。

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---january-2019"></a>Azure AD 應用程式庫推出新的同盟應用程式 - 2019 年 1 月

**類型：** 新功能  
**服務類別：** 企業應用程式  
**產品功能：** 協力廠商整合
 
我們已在 2019 年 1 月將下列這 35 個提供同盟支援的全新應用程式新增至應用程式庫：

[Firstbird](https://docs.microsoft.com/azure/active-directory/saas-apps/firstbird-tutorial)， [Folloze](https://docs.microsoft.com/azure/active-directory/saas-apps/folloze-tutorial)，[人才調色盤](https://docs.microsoft.com/azure/active-directory/saas-apps/talent-palette-tutorial)， [Infor CloudSuite](https://docs.microsoft.com/azure/active-directory/saas-apps/infor-cloud-suite-tutorial)， [Cisco 傘](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-umbrella-tutorial)， [Zscaler網際網路存取 」 系統管理員](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-internet-access-administrator-tutorial)，[到期提醒](https://docs.microsoft.com/azure/active-directory/saas-apps/expiration-reminder-tutorial)， [InstaVR 檢視器](https://docs.microsoft.com/azure/active-directory/saas-apps/instavr-viewer-tutorial)， [CorpTax](https://docs.microsoft.com/azure/active-directory/saas-apps/corptax-tutorial)，[動詞](https://app.verb.net/login)， [OpenLattice](https://openlattice.com/agora)， [TheOrgWiki](https://www.theorgwiki.com/signup)，[關閉 Pavaso 數位](https://docs.microsoft.com/azure/active-directory/saas-apps/pavaso-digital-close-tutorial)， [GoodPractice Toolkit](https://docs.microsoft.com/azure/active-directory/saas-apps/goodpractice-toolkit-tutorial)， [雲端服務 PICCO](https://docs.microsoft.com/azure/active-directory/saas-apps/cloud-service-picco-tutorial)， [AuditBoard](https://docs.microsoft.com/azure/active-directory/saas-apps/auditboard-tutorial)， [iProva](https://docs.microsoft.com/azure/active-directory/saas-apps/iprova-tutorial)， [Workable](https://docs.microsoft.com/azure/active-directory/saas-apps/workable-tutorial)， [CallPlease](https://webapp.callplease.com/create-account/create-account.html)， [GTNexus SSO 系統](https://docs.microsoft.com/azure/active-directory/saas-apps/gtnexus-sso-module-tutorial)， [CBRE ServiceInsight](https://docs.microsoft.com/azure/active-directory/saas-apps/cbre-serviceinsight-tutorial)， [Deskradar](https://docs.microsoft.com/azure/active-directory/saas-apps/deskradar-tutorial)， [Coralogixv](https://docs.microsoft.com/azure/active-directory/saas-apps/coralogix-tutorial)， [Signagelive](https://docs.microsoft.com/azure/active-directory/saas-apps/signagelive-tutorial)，[阿瑞斯 for Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/ares-for-enterprise-tutorial)，[適用於 Office 365 的 K2](https://www.k2.com/O365)， [Xledger](https://www.xledger.net/)， [iDiD Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/idid-manager-tutorial)， [HighGear](https://docs.microsoft.com/azure/active-directory/saas-apps/highgear-tutorial)， [Visitly](https://docs.microsoft.com/azure/active-directory/saas-apps/visitly-tutorial)， [Korn 兩者之間互相傳遞 ALP](https://docs.microsoft.com/azure/active-directory/saas-apps/korn-ferry-alp-tutorial)， [Acadia](https://docs.microsoft.com/azure/active-directory/saas-apps/acadia-tutorial)， [Adoddle cSaas 平台](https://docs.microsoft.com/azure/active-directory/saas-apps/adoddle-csaas-platform-tutorial)<!-- , [CaféX Portal (Meetings)](https://docs.microsoft.com/azure/active-directory/saas-apps/cafexportal-meetings-tutorial), [MazeMap Link](https://docs.microsoft.com/azure/active-directory/saas-apps/mazemaplink-tutorial)-->  

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。 如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://aka.ms/azureadapprequest)。

---

### <a name="new-azure-ad-identity-protection-enhancements-public-preview"></a>新增 Azure AD Identity Protection 增強功能 (公開預覽)

**類型：** 已變更的功能  
**服務類別：** 身分識別保護  
**產品功能：** 身分識別安全性與保護

我們很興奮地宣布，我們在 Azure AD Identity Protection 公開預覽供應項目中新增了下列增強功能，包括：

- 已更新且整合性更高的使用者介面

- 其他 API

- 透過機器學習服務改良風險評估

- 有風險的使用者與有風險的登入間的全產品對照

如需增強功能的詳細資訊，請參閱[什麼是 Azure Active Directory Identity Protection (已重新整理)？](https://aka.ms/IdentityProtectionDocs) 以深入了解相關資訊並透過產品內的提示分享您的想法。

---

### <a name="new-app-lock-feature-for-the-microsoft-authenticator-app-on-ios-and-android-devices"></a>為 iOS 及 Android 裝置上的 Microsoft Authenticator 應用程式新增應用程式鎖定功能

**類型：** 新功能  
**服務類別：** Microsoft Authenticator 應用程式  
**產品功能：** 身分識別安全性與保護

若要讓單次密碼、應用程式資訊和應用程式設定更加安全，您可以在 Microsoft Authenticator 應用程式中開啟應用程式鎖定功能。 開啟 [應用程式鎖定] 表示系統會在您每次開啟 Microsoft Authenticator 應用程式時，要求您使用 PIN 碼或生物特徵辨識進行驗證。

如需詳細資訊，請參閱 [Microsoft Authenticator 應用程式常見問題集](https://docs.microsoft.com/azure/active-directory/user-help/microsoft-authenticator-app-faq)。

---

### <a name="enhanced-azure-ad-privileged-identity-management-pim-export-capabilities"></a>增強 Azure AD Privileged Identity Management (PIM) 的匯出功能

**類型：** 新功能  
**服務類別：** Privileged Identity Management  
**產品功能：** Privileged Identity Management

Privileged Identity Management (PIM) 系統管理員現在可以針對特定資源匯出所有作用中且合格的角色指派，其中包含所有子資源的角色指派。 之前系統管理員很難取得訂用帳戶的角色指派完整清單，而且他們必須針對每個特定資源匯出角色指派。

如需詳細資訊，請參閱[在 PIM 中檢視 Azure 資源角色的活動和稽核記錄](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)。

---

## <a name="novemberdecember-2018"></a>2018 年 11 月/12月

### <a name="users-removed-from-synchronization-scope-no-longer-switch-to-cloud-only-accounts"></a>從同步處理範圍移除的使用者不會再切換為僅雲端帳戶

**類型：** 已修正  
**服務類別：** 使用者管理  
**產品功能：** 目錄

>[!Important]
>我們已收到訊息，且了解您對此修正並不滿意。 因此，我們已將這項變更還原到可讓您較易於在組織中實作修正的時間點。

我們已修正在 Azure AD 中將 Active Directory Domain Services (AD DS) 物件從同步處理範圍排除，並接著在後續的同步週期移至資源回收桶時，使用者的 DirSyncEnabled 旗標會錯誤地切換至 **False**。 由於此修正的結果，如果從同步處理範圍排除使用者，並在之後從 Azure AD 資源回收桶還原，使用者帳戶會如預期地保留與內部部署 AD 的同步，且無法從雲端管理，因為其授權來源 (SoA) 仍然是內部部署 AD。

在此修正之前，於 DirSyncEnabled 旗標切換為 False 時會發生問題。 這會給人錯誤的印象，認為這些帳戶已被轉換為僅限雲端物件，且該帳戶可在雲端中進行管理。 不過，帳戶仍然保留其 SoA 為內部部署，且所有已同步的屬性 (陰影屬性) 皆來自內部部署 AD。 這種情況會在 Azure AD 中造成多個問題，且其他雲端工作負載 (例如 Exchange Online) 預期將這些帳戶視為從 AD 進行同步處理，但現在行為卻類似僅雲端帳戶。

這時，將 AD 帳戶同步處理真正轉換為僅雲端帳戶的唯一方式，是停用租用戶層級的 DirSync，這會觸發後端作業以傳輸 SoA。 這類 SoA 變更必須 (但不限於) 清除所有與內部部署相關的屬性 (例如 LastDirSyncTime 和陰影屬性)，並且傳送信號給其他雲端工作負載，以使其各自的物件也轉換為僅雲端帳戶。

因此，此修正防止直接更新從 AD 同步處理之使用者的 ImmutableID 屬性 (這在過去的某些案例中是必要的)。 根據設計，Azure AD 中物件的 ImmutableID 顧名思義就是不可變更的。 Azure Active Directory Connect Health 和 Azure AD Connect 同步用戶端實作的新功能，可用來解決這類案例：

- **以分段方式完成大規模更新許多使用者的 ImmutableID**
  
  例如，您需要執行冗長的 AD DS 內部樹系移轉。 解決方案：使用 Azure AD Connect **設定 Source Anchor**，然後隨著使用者移轉，從 Azure AD 將現有的 ImmutableID 值複製到新樹系本機 AD DS 使用者的 DS-Consistency-Guid 屬性。 如需詳細資訊，請參閱[使用 ms-DS-ConsistencyGuid 作為 sourceAnchor](/azure/active-directory/hybrid/plan-connect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor)。

- **一次完成大規模更新許多使用者的 ImmutableID**

  例如，您在實作 Azure AD Connect 時發生錯誤，而現在您需要變更 SourceAnchor 屬性。 解決方案：停用租用戶層級的 DirSync，並清除所有無效的 ImmutableID 值。 如需詳細資訊，請參閱[關閉 Office 365 的目錄同步處理](/office365/enterprise/turn-off-directory-synchronization)。

- **重新比對內部部署使用者與 Azure AD 中的現有使用者** 例如，在 AD DS 中重新建立了某個使用者，在 Azure AD 帳戶中產生重複，而不是重新比對現有的 Azure AD 帳戶 (孤立的物件)。 解決方案：在 Azure 入口網站中 使用 Azure AD Connect Health 以重新對應來源錨點/ImmutableID。 如需詳細資訊，請參閱[孤立物件案例](/azure/active-directory/hybrid/how-to-connect-health-diagnose-sync-errors#orphaned-object-scenario)。

### <a name="breaking-change-updates-to-the-audit-and-sign-in-logs-schema-through-azure-monitor"></a>中斷性變更：透過 Azure 監視器更新稽核和登入記錄檔結構描述

**類型：** 已變更的功能  
**服務類別：** 報告  
**產品功能：** 監視與報告

我們目前透過 Azure 監視器發佈稽核和登入記錄資料流，因此您可以將記錄檔與 SIEM 工具或 Log Analytics完美整合。 根據您的意見反應，並準備此功能正式發行的宣佈，我們對結構描述進行下列變更。 這些結構描述變更及其相關文件更新將在 1 月的第一週進行。

#### <a name="new-fields-in-the-audit-schema"></a>稽核結構描述中的新欄位
我們正在新增新的 [操作類型] 欄位，以提供對資源執行的操作類型。 例如，**新增**、**更新**或**刪除**。

#### <a name="changed-fields-in-the-audit-schema"></a>稽核結構描述中變更的欄位
稽核結構描述中的下列欄位會變更：

|欄位名稱|變更內容|舊值|新值|
|----------|------------|----------|----------|
|類別|這以前是 [服務名稱] 欄位。 現在是 [稽核類別] 欄位。 [服務名稱] 已重新命名為 [loggedByService] 欄位。|<ul><li>帳戶佈建</li><li>核心目錄</li><li>自助式密碼重設</li></ul>|<ul><li>使用者管理</li><li>群組管理</li><li>應用程式管理</li></ul>|
|targetResources|包含最高層級的 **TargetResourceType**。|&nbsp;|<ul><li>原則</li><li>應用程式</li><li>使用者</li><li>群組</li></ul>|
|loggedByService|提供產生稽核記錄之服務的名稱。|Null|<ul><li>帳戶佈建</li><li>核心目錄</li><li>自助式密碼重設</li></ul>|
|結果|提供稽核記錄的結果。 在過去這是列舉的，但我們現在會顯示實際值。|<ul><li>0</li><li>1</li></ul>|<ul><li>成功</li><li>失敗</li></ul>|

#### <a name="changed-fields-in-the-sign-in-schema"></a>登入結構描述中變更的欄位
登入結構描述中的下列欄位會變更：

|欄位名稱|變更內容|舊值|新值|
|----------|------------|----------|----------|
|appliedConditionalAccessPolicies|這以前是 [conditionalaccessPolicies] 欄位。 現在是 [appliedConditionalAccessPolicies] 欄位。|無變更|無變更|
|conditionalAccessStatus|在登入時提供條件式存取原則狀態的結果。 在過去這是列舉的，但我們現在會顯示實際值。|<ul><li>0</li><li>1</li><li>2</li><li>3</li></ul>|<ul><li>成功</li><li>失敗</li><li>未套用</li><li>已停用</li></ul>|
|appliedConditionalAccessPolicies: result|在登入時提供個別條件式存取原則狀態的結果。 在過去這是列舉的，但我們現在會顯示實際值。|<ul><li>0</li><li>1</li><li>2</li><li>3</li></ul>|<ul><li>成功</li><li>失敗</li><li>未套用</li><li>已停用</li></ul>|

如需結構描述的相關詳細資訊，請參閱[解譯 Azure 監視器中的 Azure AD 稽核記錄結構描述 (預覽)](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema)

---

### <a name="identity-protection-improvements-to-the-supervised-machine-learning-model-and-the-risk-score-engine"></a>受監督的機器學習模型和風險分數引擎的身分識別保護增強功能

**類型：** 已變更的功能  
**服務類別：** 身分識別保護  
**產品功能：** 風險分數

對身分識別保護相關使用者和登入風險評估引擎的增強功能，有助於改善使用者風險精確度和涵蓋範圍。 系統管理員可能會注意到，使用者風險層級不再直接與特定偵測的風險層級相關聯，並且有風險的登入事件的數量和等級也會增加。

風險偵測現在由受監督的機器學習服務模型評估，該模型透過使用使用者登入的其他功能和偵測模式來計算使用者風險。 根據此模型，系統管理員可能會發現具有高風險分數的使用者，即使與該使用者相關聯的偵測屬於低或中層級的風險。 

---

### <a name="administrators-can-reset-their-own-password-using-the-microsoft-authenticator-app-public-preview"></a>系統管理員可以使用 Microsoft Authenticator 應用程式重設其密碼 (公開預覽)

**類型：** 已變更的功能  
**服務類別：** 自助式密碼重設  
**產品功能：** 使用者驗證

Azure AD 系統管理員現在可以使用 Microsoft Authenticator 應用程式通知，或來自任何行動驗證器應用程式或硬體權杖的程式碼重設其密碼。 若要重設其密碼，系統管理員現在可以使用下列兩個方法：

- Microsoft Authenticator 應用程式通知

- 其他行動驗證器應用程式 / 硬體權杖

- 電子郵件

- 撥打電話

- 簡訊

如需使用 Microsoft Authenticator 應用程式重設密碼的相關詳細資訊，請參閱 [Azure AD 自助式密碼重設 - 行動裝置應用程式和 SSPR (預覽)](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks#mobile-app-and-sspr-preview)

---

### <a name="new-azure-ad-cloud-device-administrator-role-public-preview"></a>新的 Azure AD 雲端裝置系統管理員角色 (公開預覽)

**類型：** 新功能  
**服務類別：** 裝置註冊與管理  
**產品功能：** 存取控制

系統管理員可以將使用者指派至新的雲端裝置系統管理員角色，以執行雲端裝置系統管理員工作。 使用者指派的雲端裝置系統管理員角色者可以啟用、停用和刪除 Azure AD 中的裝置，且能在 Azure 入口網站中讀取 Windows 10 BitLocker 金鑰 (如果有的話)。

如需有關角色和權限的詳細資訊，請參閱[在 Azure Active Directory 中指派系統管理員角色](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)

---

### <a name="manage-your-devices-using-the-new-activity-timestamp-in-azure-ad-public-preview"></a>使用 Azure AD 中的新活動時間戳記管理裝置 (公開預覽)

**類型：** 新功能  
**服務類別：** 裝置註冊與管理  
**產品功能：** 裝置生命週期管理

我們了解，您必須隨著時間在 Azure AD 中重新整理和淘汰組織的裝置，以避免裝置在您的環境中閒置。 為了協助進行此程序，Azure AD 現在會使用新的活動時間戳記更新您的裝置，協助您管理您的裝置生命週期。

如需如何取得及使用此時間戳記的詳細資訊，請參閱[作法：管理 Azure AD 中的過時裝置](https://docs.microsoft.com/azure/active-directory/devices/manage-stale-devices)

---

### <a name="administrators-can-require-users-to-accept-a-terms-of-use-on-each-device"></a>系統管理員可以要求使用者接受每個裝置上的使用規定

**類型：** 新功能  
**服務類別：** 使用條款  
**產品功能：** 控管
 
系統管理員現在可以開啟 [需要使用者在每部裝置上同意] 選項，要求使用者接受他們在您的租用戶上所使用之每個裝置的使用規定。

如需詳細資訊，請參閱 [Azure Active Directory 使用規定特性的＜每部裝置的使用規定＞一節](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou#per-device-terms-of-use)。

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-a-recurring-schedule"></a>系統管理員可以根據週期性排程將使用規定設定為到期

**類型：** 新功能  
**服務類別：** 使用條款  
**產品功能：** 控管
 

系統管理員現在可以開啟 [到期同意]選項，根據您指定的週期性排程，為所有使用者設定使用規定到期。 排程可以是每年、每兩年、每季，或每個月。 使用規定到期之後，使用者必須重新接受。

如需詳細資訊，請參閱 [Azure Active Directory 使用規定特性的＜新增使用規定＞一節](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou#add-terms-of-use)。

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-each-users-schedule"></a>系統管理員可以根據每位使用者的排程，將使用規定設定為到期

**類型：** 新功能  
**服務類別：** 使用條款  
**產品功能：** 控管

系統管理員現在可以指定使用者必須重新接受使用規定的持續時間。 例如，系統管理員可以指定使用者必須每隔 90 天重新接受使用規定。

如需詳細資訊，請參閱 [Azure Active Directory 使用規定特性的＜新增使用規定＞一節](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou#add-terms-of-use)。
 
---

### <a name="new-azure-ad-privileged-identity-management-pim-emails-for-azure-active-directory-roles"></a>Azure Active Directory 角色的新 Azure AD Privileged Identity Management (PIM) 電子郵件

**類型：** 新功能  
**服務類別：** Privileged Identity Management  
**產品功能：** Privileged Identity Management
 
使用 Azure AD Privileged Identity Management (PIM) 的客戶現在可以收到每週摘要電子郵件，其中包括過去七天的下列資訊：

- 最高合格和永久角色指派概觀

- 啟用角色的使用者數目

- 指派給 PIM 中角色的使用者數目

- 指派給 PIM 外部角色的使用者數目

- PIM 中「永久的」使用者數目

如需 PIM 和可用電子郵件通知的相關詳細資訊，請參閱 [PIM 中的電子郵件通知](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-email-notifications)。

---

### <a name="group-based-licensing-is-now-generally-available"></a>群組型授權現已正式推出

**類型：** 已變更的功能  
**服務類別：** 其他  
**產品功能：** 目錄

群組型授權不在公開預覽範圍內，而且現在已正式推出。 作為此正式發行的一部分，我們使這項功能更具擴充性，並新增了為單一使用者重新處理群組型授權指派的功能，以及搭配 Office 365 E3/A3 授權使用群組型授權的功能。

如需群組型授權的相關詳細資訊，請參閱[什麼是Azure Active Directory 中的群組型授權？](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-licensing-whatis-azure-portal)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---november-2018"></a>Azure AD 應用程式資源庫中有新的同盟應用程式可用 - 2018 年 11 月

**類型：** 新功能  
**服務類別：** 企業應用程式  
**產品功能：** 協力廠商整合
 
我們已在 2018 年 11 月將下列這 26 個提供同盟支援的全新應用程式新增至應用程式資源庫：

[CoreStack](https://cloud.corestack.io/site/login)、[HubSpot](https://docs.microsoft.com/azure/active-directory/saas-apps/HubSpot-tutorial)、[GetThere](https://docs.microsoft.com/azure/active-directory/saas-apps/getthere-tutorial)、[Gra-Pe](https://docs.microsoft.com/azure/active-directory/saas-apps/grape-tutorial)、[eHour](https://getehour.com/try-now)、[Consent2Go](https://docs.microsoft.com/azure/active-directory/saas-apps/Consent2Go-tutorial)、[Appinux](https://docs.microsoft.com/azure/active-directory/saas-apps/appinux-tutorial)、[DriveDollar](https://www.drivedollar.com/Account/Login)、[Useall](https://docs.microsoft.com/azure/active-directory/saas-apps/useall-tutorial)、[Infinite Campus](https://docs.microsoft.com/azure/active-directory/saas-apps/infinitecampus-tutorial)、[Alaya](https://alayagood.com/en/demo/)、[HeyBuddy](https://docs.microsoft.com/azure/active-directory/saas-apps/heybuddy-tutorial)、[Wrike SAML](https://docs.microsoft.com/azure/active-directory/saas-apps/wrike-tutorial)、[Drift](https://docs.microsoft.com/azure/active-directory/saas-apps/drift-tutorial)、[Zenegy for Business Central 365](https://accounting.zenegy.com/)、[Everbridge 成員入口網站](https://docs.microsoft.com/azure/active-directory/saas-apps/everbridge-tutorial)、[IDEO](https://profile.ideo.com/users/sign_up)、[Ivanti Service Manager (ISM)](https://docs.microsoft.com/azure/active-directory/saas-apps/ivanti-service-manager-tutorial)、[Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-tutorial)、[Allbound SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/allbound-sso-tutorial)、[Plex 應用程式 - 傳統測試](https://test.plexonline.com/signon)、[Plex 應用程式 – 傳統 ](https://www.plexonline.com/signon)、[Plex 應用程式 - UX 測試](https://test.cloud.plex.com/sso)、[Plex 應用程式 – UX](https://cloud.plex.com/sso)、[Plex 應用程式 – IAM](https://accounts.plex.com/)、[CRAFTS - 育兒記錄、出席與財務追蹤系統](https://getcrafts.ca/craftsregistration) 

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。 如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://aka.ms/azureadapprequest)。

---

## <a name="october-2018"></a>2018 年 10 月

### <a name="azure-ad-logs-now-work-with-azure-log-analytics-public-preview"></a>Azure AD 記錄現在適用於 Azure Log Analytics (公開預覽)

**類型：** 新功能  
**服務類別：** 報告  
**產品功能：** 監視與報告

我們很高興宣佈您現在可以將您的 Azure AD 記錄轉送到 Azure Log Analytics！ 此呼聲最高的功能可讓您更容易存取業務、作業和安全性的分析，以及提供協助監視基礎結構的方法。 如需詳細資訊，請參閱 [Azure Active Directory Activity logs in Azure Log Analytics now available](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-Activity-logs-in-Azure-Log-Analytics-now/ba-p/274843) (現在可以使用 Azure Log Analytics 中的 Azure Active Directory 記錄) 部落格。

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---october-2018"></a>Azure AD 應用程式資源庫中有新的同盟應用程式可用 - 2018 年 10 月

**類型：** 新功能  
**服務類別：** 企業應用程式  
**產品功能：** 協力廠商整合
 
我們已在 2018 年 10 月將下列這 14 個提供同盟支援的全新應用程式新增至應用程式資源庫：

[My Award Points](https://docs.microsoft.com/azure/active-directory/saas-apps/myawardpoints-tutorial)、[Vibe HCM](https://docs.microsoft.com/azure/active-directory/saas-apps/vibehcm-tutorial)、Ambyint、[MyWorkDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/myworkdrive-tutorial)、[BorrowBox](https://docs.microsoft.com/azure/active-directory/saas-apps/borrowbox-tutorial)、Dialpad、[ON24 Virtual Environment](https://docs.microsoft.com/azure/active-directory/saas-apps/on24-tutorial)、[RingCentral](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-tutorial)、[Zscaler Three](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-three-tutorial)、[Phraseanet](https://docs.microsoft.com/azure/active-directory/saas-apps/phraseanet-tutorial)、[Appraisd](https://docs.microsoft.com/azure/active-directory/saas-apps/appraisd-tutorial)、[Workspot Control](https://docs.microsoft.com/azure/active-directory/saas-apps/workspotcontrol-tutorial)、[Shuccho Navi](https://docs.microsoft.com/azure/active-directory/saas-apps/shucchonavi-tutorial)、[Glassfrog](https://docs.microsoft.com/azure/active-directory/saas-apps/glassfrog-tutorial)

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。 如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://aka.ms/azureadapprequest)。

---

### <a name="azure-ad-domain-services-email-notifications"></a>Azure AD Domain Services 電子郵件通知

**類型：** 新功能  
**服務類別：** Azure AD 網域服務  
**產品功能：** Azure AD 網域服務

Azure Active Directory Domain Services 會在 Azure 入口網站上提供有關受控網域設定錯誤或問題的相關警示。 這些警示包含逐步指南，因此您可以嘗試修正問題，而不需連絡支援人員。

從十月開始，您就能夠自訂受控網域的通知設定，以便在出現新的警示時，將電子郵件傳送給指定的一群人，而不需要經常檢查入口網站中是否有更新。

如需詳細資訊，請參閱 [Azure AD Domain Services 中的通知設定](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-notifications)。

---

### <a name="azure-ad-portal-supports-using-the-forcedelete-domain-api-to-delete-custom-domains"></a>Azure AD 入口網站支援使用 ForceDelete 網域 API 來刪除自訂網域 

**類型：** 已變更的功能  
**服務類別：** 目錄管理  
**產品功能：** 目錄

我們很高興宣佈您現在可以使用 ForceDelete 網域 API，以非同步方式將參考資料 (例如使用者、群組和應用程式) 從自訂網域名稱 (contoso.com) 重新命名回初始預設網域名稱 (contoso.onmicrosoft.com)，藉此刪除自訂網域名稱。

如果貴組織不再使用您的自訂網域名稱，或如果您需要在另一個 Azure AD 中使用該網域名稱，此變更可協助您更快速地刪除該網域名稱。

如需詳細資訊，請參閱[刪除自訂網域名稱](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-manage#delete-a-custom-domain-name)。

---

## <a name="september-2018"></a>2018 年 9 月
 
### <a name="updated-administrator-role-permissions-for-dynamic-groups"></a>已更新動態群組的系統管理員角色權限

**類型：** 已修正  
**服務類別：** 群組管理  
**產品功能：** 共同作業

我們已修正某個問題，讓特定系統管理員角色現在可以建立和更新動態成員資格規則，而不必成為群組的擁有者。

角色如下：

- 全域管理員

- Intune 系統管理員

- 使用者管理員

如需詳細資訊，請參閱[建立動態群組並檢查狀態](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule)

---

### <a name="simplified-single-sign-on-sso-configuration-settings-for-some-third-party-apps"></a>簡化了某些第三方應用程式的單一登入 (SSO) 組態設定

**類型：** 新功能  
**服務類別：** 企業應用程式  
**產品功能：** SSO

我們了解，因為每個應用程式的組態都有獨特的本質，所以為軟體即服務 (SaaS) 應用程式設定單一登入 (SSO) 並不容易。 我們已建置經過簡化的組態體驗，來為下列第三方 SaaS 應用程式自動填入 SSO 組態設定：

- Zendesk

- ArcGis Online

- Jamf Pro

若要開始使用這個一鍵式體驗，請移至應用程式的 **Azure 入口網站** > [SSO 組態] 頁面。 如需詳細資訊，請參閱 [SaaS 應用程式與 Azure Active Directory 的整合](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)

---

### <a name="azure-active-directory---where-is-your-data-located-page"></a>[Azure Active Directory - 資料位於何處？] 頁面

**類型：** 新功能  
**服務類別：** 其他  
**產品功能：** GoLocal

從 [Azure Active Directory - 資料位於何處] 頁面選取公司的區域，以檢視是哪一個 Azure 資料中心裝載了所有 Azure AD 服務的 Azure AD 待用資料。 您可以根據貴公司區域的特定 Azure AD 服務來篩選資訊。

若要存取這項功能以及需要詳細資訊，請參閱 [Azure Active Directory - 資料位於何處](https://aka.ms/AADDataMap)。

---

### <a name="new-deployment-plan-available-for-the-my-apps-access-panel"></a>適用於 [我的應用程式] 存取面板的新部署方案

**類型：** 新功能  
**服務類別：** 我的應用程式  
**產品功能：** SSO

查看適用於 [我的應用程式] 存取面板的新部署方案 (https://aka.ms/deploymentplans)。
[我的應用程式] 存取面板可讓使用者在單一位置尋找及存取其應用程式。 此入口網站也提供使用者自助服務的機會，例如要求存取應用程式和群組，或代表其他人管理這些資源的存取權。

如需詳細資訊，請參閱[什麼是「我的應用程式」入口網站？](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-saas-access-panel-introduction)

---

### <a name="new-troubleshooting-and-support-tab-on-the-sign-ins-logs-page-of-the-azure-portal"></a>Azure 入口網站的 [登入記錄] 頁面上有新的 [疑難排解和支援] 索引標籤

**類型：** 新功能  
**服務類別：** 報告  
**產品功能：** 監視與報告

Azure 入口網站的 [登入] 頁面上有新的 [疑難排解和支援] 索引標籤，其目的是要協助系統管理員和支援工程師針對與 Azure AD 登入相關的問題進行疑難排解。這個新的索引標籤會提供錯誤碼、錯誤訊息和補救建議 (如果有的話) 來協助您解決問題。 如果您無法解決問題，我們也提供了新的辦法供您使用**複製到剪貼簿**體驗來新建支援票證，其會在支援票證的記錄檔中填入 [要求識別碼] 和 [日期 (UTC)] 欄位。  

![顯示新的索引標籤的登入記錄](media/whats-new/troubleshooting-and-support.png)

---

### <a name="enhanced-support-for-custom-extension-properties-used-to-create-dynamic-membership-rules"></a>針對用來建立動態成員資格規則的自訂擴充屬性強化其支援

**類型：** 已變更的功能  
**服務類別：** 群組管理  
**產品功能：** 共同作業

經由此更新，您現在可以從動態使用者群組規則建立器按一下 [取得自訂擴充屬性] 連結，輸入唯一的應用程式識別碼，然後接收在為使用者建立動態成員資格規則時所要使用的自訂擴充屬性完整清單。 您也可以重新整理這份清單，來針對該應用程式取得任何新的自訂擴充屬性。

如需針對動態成員資格規則使用自訂擴充屬性的詳細資訊，請參閱[擴充屬性和自訂擴充屬性](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership#extension-properties-and-custom-extension-properties)

---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>針對 Azure AD 應用程式型條件式存取之新的已核准用戶端應用程式

**類型：** 針對變更做規劃  
**服務類別：** 條件式存取  
**產品功能：** 身分識別安全性與保護

下列應用程式已列入[已核准的用戶端應用程式](https://docs.microsoft.com/azure/active-directory/conditional-access/technical-reference#approved-client-app-requirement)清單：

- Microsoft To-Do

- Microsoft Stream

如需詳細資訊，請參閱

- [Azure AD 應用程式型條件式存取](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="new-support-for-self-service-password-reset-from-the-windows-7881-lock-screen"></a>針對 Windows 7/8/8.1 鎖定畫面中的自助式密碼重設提供了新的支援

**類型：** 新功能  
**服務類別：** SSPR  
**產品功能：** 使用者驗證

在設定這項新功能之後，使用者就會在執行 Windows 7、Windows 8 或 Windows 8.1 的裝置中，從其 [鎖定] 畫面看到用以重設其密碼的連結。 藉由按一下該連結，系統就會引導使用者完成密碼重設流程，過程就和透過網頁瀏覽器所進行的一樣。

如需詳細資訊，請參閱[如何從 Windows 7、8 和 8.1 啟用密碼重設](https://aka.ms/ssprforwindows78)

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>變更通知：授權碼將不再供重複使用 

**類型：** 針對變更做規劃  
**服務類別：** 驗證 (登入)  
**產品功能：** 使用者驗證

從 2018 年 11 月 15 日起，Azure AD 將不再接受將先前使用的驗證碼用於應用程式。 這項安全性變更有助於讓 Azure AD 與 OAuth 規格一致，且將會對 v1 和 v2 端點強制執行。

如果您的應用程式重複使用授權碼取得多個資源的權杖，建議您先使用授權碼取得重新整理權杖，再使用該重新整理權杖取得其他資源的額外權杖。 授權碼只能使用一次，但重新整理權杖可跨多個資源多次使用。 在 OAuth 程式碼流程期間嘗試重複使用驗證碼的應用程式，會收到 invalid_grant 錯誤。

針對這項變更以及其他與通訊協定相關的變更，請參閱[新驗證功能的完整清單](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes)。

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---september-2018"></a>Azure AD 應用程式資源庫中有新的同盟應用程式可用 - 2018 年 9 月

**類型：** 新功能  
**服務類別：** 企業應用程式  
**產品功能：** 協力廠商整合
 
我們已在 2018 年 9 月將下列這 16 個提供同盟支援的全新應用程式新增至應用程式資源庫：

[Uberflip](https://docs.microsoft.com/azure/active-directory/saas-apps/uberflip-tutorial)、[Comeet Recruiting Software](https://docs.microsoft.com/azure/active-directory/saas-apps/comeetrecruitingsoftware-tutorial)、[Workteam](https://docs.microsoft.com/azure/active-directory/saas-apps/workteam-tutorial)、[ArcGIS Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/arcgisenterprise-tutorial)、[Nuclino](https://docs.microsoft.com/azure/active-directory/saas-apps/nuclino-tutorial)、[JDA Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/jdacloud-tutorial)、[Snowflake](https://docs.microsoft.com/azure/active-directory/saas-apps/snowflake-tutorial)、NavigoCloud、[Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-tutorial)、join.me、[ZephyrSSO](https://docs.microsoft.com/azure/active-directory/saas-apps/zephyrsso-tutorial)、[Silverback](https://docs.microsoft.com/azure/active-directory/saas-apps/silverback-tutorial)、Riverbed Xirrus EasyPass、[Rackspace SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/rackspacesso-tutorial)、Enlyft SSO for Azure、SurveyMonkey、[Convene](https://docs.microsoft.com/azure/active-directory/saas-apps/convene-tutorial)、[dmarcian](https://docs.microsoft.com/azure/active-directory/saas-apps/dmarcian-tutorial)

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。 如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://aka.ms/azureadapprequest)。

---

### <a name="support-for-additional-claims-transformations-methods"></a>支援其他宣告轉換方法

**類型：** 新功能  
**服務類別：** 企業應用程式  
**產品功能：** SSO

我們已導入新的宣告轉換方法 ToLower() 和 ToUpper()，您可從以 SAML 為基礎的 [單一登入設定] 頁面套用至 SAML 權杖。

如需詳細資訊，請參閱[如何針對 Azure AD 中的企業應用程式自訂 SAML 權杖中發出的宣告](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)

---

### <a name="updated-saml-based-app-configuration-ui-preview"></a>經過更新的 SAML 型應用程式設定 UI (預覽)

**類型：** 已變更的功能  
**服務類別：** 企業應用程式  
**產品功能：** SSO

在經過更新的 SAML 型應用程式設定 UI 中，您會獲得：

- 經過更新的 SAML 型應用程式設定逐步解說體驗。

- 更加了解設定中少了什麼或有何錯誤。

- 能夠新增多個用於到期憑證通知的電子郵件地址。

- 新的宣告轉換方法 ToLower() 和 ToUpper() 等等。

- 有辦法為企業應用程式上傳您自己的權杖簽署憑證。

- 有辦法設定 SAML 應用程式的 NameID 格式以及將 NameID 值設定為目錄擴充。

若要開啟此更新後的檢視，請從 [單一登入] 頁面頂端按一下 [試用新的體驗] 連結。 如需詳細資訊，請參閱[教學課程：為應用程式設定透過 Azure Active Directory 進行的 SAML 型單一登入](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-single-sign-on-portal)。

---

## <a name="august-2018"></a>2018 年 8 月

### <a name="changes-to-azure-active-directory-ip-address-ranges"></a>Azure Active Directory IP 位址範圍的變更

**類型：** 針對變更做規劃  
**服務類別：** 其他  
**產品功能：** 平台

我們正在將更大的 IP 範圍引入 Azure AD 中，這表示如果您已為您的防火牆、路由器或網路安全性群組設定 Azure AD IP 位址範圍，則必須進行更新。 我們會完成此更新，因此您不需要在 Azure AD 新增端點時，再次變更防火牆、路由器或網路安全群組的 IP 範圍組態。 

接下來的兩個月，網路流量會移至這些新的範圍。 若要繼續使用不中斷的服務，您必須在 2018 年 9 月 10 日之前，將這些更新值新增至您的 IP 位址：

- 20.190.128.0/18 

- 40.126.0.0/18 

我們強烈建議您，在所有網路流量都移至新的範圍之前，不要移除舊的 IP 位址範圍。 若要了解有關移動的更新及可移除舊範圍的時機，請參閱 [Office 365 URL 與 IP 位址範圍](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)。

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>變更通知：授權碼將不再供重複使用 

**類型：** 針對變更做規劃  
**服務類別：** 驗證 (登入)  
**產品功能：** 使用者驗證

從 2018 年 11 月 15 日起，Azure AD 將不再接受將先前使用的驗證碼用於應用程式。 這項安全性變更有助於讓 Azure AD 與 OAuth 規格一致，且將會對 v1 和 v2 端點強制執行。

如果您的應用程式重複使用授權碼取得多個資源的權杖，建議您先使用授權碼取得重新整理權杖，再使用該重新整理權杖取得其他資源的額外權杖。 授權碼只能使用一次，但重新整理權杖可跨多個資源多次使用。 在 OAuth 程式碼流程期間嘗試重複使用驗證碼的應用程式，會收到 invalid_grant 錯誤。

針對這項變更以及其他與通訊協定相關的變更，請參閱[新驗證功能的完整清單](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes)。
 
---

### <a name="converged-security-info-management-for-self-service-password-sspr-and-multi-factor-authentication-mfa"></a>適用於自助式密碼 (SSPR) 和 Multi-Factor Authentication ( MFA) 的融合式安全性資訊管理

**類型：** 新功能  
**服務類別：** SSPR  
**產品功能：** 使用者驗證
 
這項新功能可協助人員以單一位置和相同方式管理其 SSPR 和 MFA 的安全性資訊 (例如，電話號碼、行動應用程式等等)；不同於先前必須在兩個不同的位置管理。

此融合式體驗也適用於使用 SSPR 或 MFA 的人員。 此外，如果您的組織未強制執行 MFA 或 SSPR 註冊，人員仍可從「我的應用程式」入口網站註冊您的組織所允許的任何 MFA 或 SSPR 安全性資訊方法。

這是可選擇加入的公開預覽。 系統管理員可以為租用戶中選定的群組或所有使用者開啟新體驗 (如有需要)。 如需融合式體驗的詳細資訊，請參閱[融合式體驗部落格](https://cloudblogs.microsoft.com/enterprisemobility/2018/08/06/mfa-and-sspr-updates-now-in-public-preview/)

---

### <a name="new-http-only-cookies-setting-in-azure-ad-application-proxy-apps"></a>Azure AD 應用程式 Proxy 應用程式中新的僅限 HTTP Cookie 設定

**類型：** 新功能  
**服務類別：** 應用程式 Proxy  
**產品功能：** 存取控制

您的應用程式 Proxy 應用程式中有新的設定，名為「僅限 HTTP Cookies」。 此設定有助於提供更高的安全性，因為它可在應用程式 Proxy 存取和工作階段 Cookie 的 HTTP 回應標頭中加入 HTTPOnly 旗標，以禁止透過用戶端指令碼存取 Cookie，並進一步防止複製或修改 Cookie 之類的動作。 雖然過去並未使用此旗標，但您的 Cookie 一向都會加密，並使用 SSL 連線進行傳輸，以利防止不當的修改。

此設定與使用 ActiveX 控制項的應用程式不相容，例如遠端桌面。 面臨此情況時，建議您關閉這項設定。

如需「僅限 HTTP Cookie」設定的詳細資訊，請參閱[使用 Azure AD 應用程式 Proxy 發佈應用程式](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-publish-azure-portal)。

---

### <a name="privileged-identity-management-pim-for-azure-resources-supports-management-group-resource-types"></a>適用於 Azure 資源的 Privileged Identity Management (PIM) 支援「管理群組」資源類型

**類型：** 新功能  
**服務類別：** Privileged Identity Management  
**產品功能：** Privileged Identity Management
 
Just-In-Time 的啟用和指派設定現已可套用至「管理群組」資源類型，一如您對訂用帳戶、資源群組和資源 (例如 VM、App Service 等) 所做的一般。 此外，任何人只要具備為管理群組提供系統管理員存取權的角色，都可以在 PIM 中探索及管理該項資源。

如需 PIM 和 Azure 資源的詳細資訊，請參閱[使用 Privileged Identity Management 探索與管理 Azure 資源](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-resource-roles-discover-resources)
 
---

### <a name="application-access-preview-provides-faster-access-to-the-azure-ad-portal"></a>「應用程式存取」(預覽) 可加快存取 Azure AD 入口網站的速度

**類型：** 新功能  
**服務類別：** Privileged Identity Management  
**產品功能：** Privileged Identity Management
 
目前，在啟用使用 PIM 的角色時，權限可能要 10 分鐘以上才會生效。 如果您選擇使用目前處於公開預覽狀態的「應用程式存取」，系統管理員將可在啟用要求完成後隨即存取 Azure AD 入口網站。

目前，「應用程式存取」僅支援 Azure AD 入口網站體驗和 Azure 資源。 如需 PIM 和「應用程式存取」的詳細資訊，請參閱[什麼是 Azure AD Privileged Identity Management？](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure)
 
---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2018"></a>Azure AD 應用程式資源庫中有新的同盟應用程式可用 - 2018 年 8 月

**類型：** 新功能  
**服務類別：** 企業應用程式  
**產品功能：** 協力廠商整合
 
我們已在 2018 年 8 月將下列這 16 個提供同盟支援的全新應用程式新增至應用程式資源庫：

[Hornbill](https://docs.microsoft.com/azure/active-directory/saas-apps/hornbill-tutorial)、[Bridgeline Unbound](https://docs.microsoft.com/azure/active-directory/saas-apps/bridgelineunbound-tutorial)、[Sauce Labs - 行動和 Web 測試](https://docs.microsoft.com/azure/active-directory/saas-apps/saucelabs-mobileandwebtesting-tutorial)、[Meta Networks Connector](https://docs.microsoft.com/azure/active-directory/saas-apps/metanetworksconnector-tutorial)、[Way We Do](https://docs.microsoft.com/azure/active-directory/saas-apps/waywedo-tutorial)、[Spotinst](https://docs.microsoft.com/azure/active-directory/saas-apps/spotinst-tutorial)、[ProMaster (by Inlogik)](https://docs.microsoft.com/azure/active-directory/saas-apps/promaster-tutorial)、SchoolBooking、[4me](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-tutorial)、[Dossier](https://docs.microsoft.com/azure/active-directory/saas-apps/DOSSIER-tutorial)、[N2F - Expense reports](https://docs.microsoft.com/azure/active-directory/saas-apps/n2f-expensereports-tutorial)、[Comm100 Live Chat](https://docs.microsoft.com/azure/active-directory/saas-apps/comm100livechat-tutorial)、[SafeConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/safeconnect-tutorial)、[ZenQMS](https://docs.microsoft.com/azure/active-directory/saas-apps/zenqms-tutorial)、[eLuminate](https://docs.microsoft.com/azure/active-directory/saas-apps/eluminate-tutorial)、[Dovetale](https://docs.microsoft.com/azure/active-directory/saas-apps/dovetale-tutorial)。

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。 如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://aka.ms/azureadapprequest)。

---

### <a name="native-tableau-support-is-now-available-in-azure-ad-application-proxy"></a>Azure AD 應用程式 Proxy 現已提供原生 Tableau 支援

**類型：** 已變更的功能  
**服務類別：** 應用程式 Proxy  
**產品功能：** 存取控制

在我們將預先驗證通訊協定從 OpenID Connect 更新為 OAuth 2.0 Code Grant 通訊協定後，您無須再進行任何額外的設定，即可搭配使用 Tableau 與應用程式 Proxy。 此一通訊協定變更也有助於應用程式 Proxy 更妥善地支援更現代化的應用程式，因為此後只需使用 HTTP 重新導向即可，而這在 JavaScript 和 HTML 標記中通常都可受到支援。

如需與我們的 Tableau 原生支援有關的詳細資訊，請參閱 [Azure AD 應用程式 Proxy 現已具備原生 Tableau 支援](https://blogs.technet.microsoft.com/applicationproxyblog/2018/08/14/azure-ad-application-proxy-now-with-native-tableau-support)。

---

### <a name="new-support-to-add-google-as-an-identity-provider-for-b2b-guest-users-in-azure-active-directory-preview"></a>在 Azure Active Directory (預覽) 中新增 Google 供 B2B 來賓使用者作為識別提供者的新支援

**類型：** 新功能  
**服務類別：** B2B  
**產品功能：** B2B/B2C

藉由在組織中設定與 Google 的同盟，您可以讓受邀的 Gmail 使用者使用其現有的 Google 帳戶登入您的共用應用程式和資源，而不需要建立個人 Microsoft 帳戶 (MSA) 或 Azure AD 帳戶。

這是可選擇加入的公開預覽。 如需 Google 同盟的詳細資訊，請參閱[將 Google 新增為 B2B 來賓使用者的識別提供者](https://docs.microsoft.com/azure/active-directory/b2b/google-federation)。

---
