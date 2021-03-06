---
title: Windows 10 登入畫面中的 Azure AD SSPR
description: 在本教學課程中，您會在 Windows 10 登入畫面中啟用密碼重設，以避免使用技術服務。
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: a0463a2ad3fa74f33a52e15a246dfd4ffd63107a
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/13/2019
ms.locfileid: "56200865"
---
# <a name="tutorial-azure-ad-password-reset-from-the-login-screen"></a>教學課程：從登入畫面重設 Azure AD 密碼

在本教學課程中，您會讓使用者從 Windows 10 登入畫面重設其密碼。 透過新的 Windows 10 2018 年 4 月更新，使用者若具有**已加入 Azure AD** 或**已加入混合式 Azure AD** 的裝置，即可在其登入畫面上使用 [重設密碼] 連結。 使用者按一下此連結時，就可以享受熟悉的相同自助式密碼重設 (SSPR) 體驗。

> [!div class="checklist"]
> * 使用 Intune 來設定重設密碼連結
> * 選擇性地使用 Windows 登錄進行設定
> * 了解使用者會看到的內容

## <a name="prerequisites"></a>必要條件

* 您必須執行至少 Windows 10 (2018 年 4 月更新版)，且裝置必須是：
   * [已加入 Azure AD](../device-management-azure-portal.md) 或
   * [已加入混合式 Azure AD](../device-management-hybrid-azuread-joined-devices-setup.md) (可透過網路連線至網域控制站)。
* 您必須啟用 Azure AD 自助式密碼重設。
* 如果您的 Windows 10 裝置位在 Proxy 伺服器或防火牆後面，您必須將 URL (`passwordreset.microsoftonline.com` 和 `ajax.aspnetcdn.com`) 新增至您 HTTPS 流量 (連接埠 443) 允許的 URL 清單。
* 在您的環境中試用這項功能之前，請先檢閱以下限制。

## <a name="configure-reset-password-link-using-intune"></a>使用 Intune 來設定重設密碼連結

使用 Intune 部署組態變更，以啟用從登入畫面重設密碼的功能，是最具彈性的方法。 Intune 可讓您將組態變更部署至您所定義的特定機器群組。 必須完成裝置的 Intune 註冊，才能使用此方法。

### <a name="create-a-device-configuration-policy-in-intune"></a>在 Intune 中建立裝置設定原則

1. 登入 [Azure 入口網站](https://portal.azure.com)，然後按一下 [Intune]。
2. 移至 [裝置組態] > [設定檔] > [建立設定檔]，以建立新的裝置組態設定檔
   * 為設定檔提供有意義的名稱
   * 選擇性地提供有意義的設定檔描述
   * 平台：**Windows 10 和更新版本**
   * 設定檔類型：**自訂**

3. 配置 [設定]
   * **新增**下列 OMA-URI 設定來啟用 [重設密碼] 連結
      * 提供有意義的名稱，以說明正在進行的設定
      * 選擇性地提供有意義的設定描述
      * **OMA URI** 設為 `./Vendor/MSFT/Policy/Config/Authentication/AllowAadPasswordReset`
      * **資料類型** 設為 **整數**
      * **值** 設為 **1**
      * 按一下 [檔案] &gt; [新增] &gt; [專案] 
   * 按一下 [檔案] &gt; [新增] &gt; [專案] 
4. 按一下 [建立] 

### <a name="assign-a-device-configuration-policy-in-intune"></a>在 Intune 中指派裝置設定原則

#### <a name="create-a-group-to-apply-device-configuration-policy-to"></a>建立要套用裝置設定原則的群組

1. 登入 [Azure 入口網站](https://portal.azure.com)，然後按一下 [Azure Active Directory]。
2. 瀏覽至 [使用者和群組] > [所有群組] > [新增群組]
3. 提供群組的名稱，然後在 [成員資格類型] 之下選擇 [已指派]
   * 在 [成員] 之下，選擇您要套用原則之已加入 Azure AD 的 Windows 10 裝置。
   * 按一下 [選取]
4. 按一下 [建立] 

如需建立群組的詳細資訊，請參閱[使用 Azure Active Directory 群組管理資源的存取權](../fundamentals/active-directory-manage-groups.md)。

#### <a name="assign-device-configuration-policy-to-device-group"></a>將裝置設定原則指派給裝置群組

1. 登入 [Azure 入口網站](https://portal.azure.com)，然後按一下 [Intune]。
2. 移至 [裝置組態] > [設定檔] 來尋找先前建立的裝置組態設定檔 > 按一下稍早建立的設定檔
3. 將設定檔指派給裝置群組 
   * 按一下 [指派] > 在 [Include] 之下 > [選取要包含的群組]
   * 選取先前建立的群組，然後按一下 [選取]
   * 按一下 [儲存] 

   ![指派][Assignment]

您現在已建立並指派裝置設定原則，以使用 Intune 在登入畫面上啟用 [重設密碼] 連結。

## <a name="configure-reset-password-link-using-the-registry"></a>使用登錄來設定重設密碼連結

1. 使用系統管理認證登入 Windows PC
2. 以系統管理員身分執行 **regedit**
3. 設定下列登錄機碼
   * `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\AzureADAccount`
      * `"AllowPasswordReset"=dword:00000001`

## <a name="what-do-users-see"></a>使用者看到的內容

現已設定和指派原則，使用者有哪些變更？ 他們如何得知可在登入畫面重設其密碼？

![LoginScreen][LoginScreen]

現在，使用者在嘗試登入時即會看到 [重設密碼] 連結，該連結可開啟登入畫面上的自助式密碼重設體驗。 這項功能可讓使用者重設其密碼，而不需使用另一個裝置來存取網頁瀏覽器。

您的使用者將在[重設您的工作或學校密碼](../user-help/active-directory-passwords-update-your-own-password.md#reset-password-at-sign-in)中發現使用這項功能的指引

Azure AD 稽核記錄會包含 IP 位址相關資訊以及發生密碼重設的 ClientType。

![Azure AD 稽核記錄中的範例登入畫面密碼重設](media/tutorial-sspr-windows/windows-sspr-azure-ad-audit-log.png)

當使用者從 Windows 10 裝置的登入畫面重設其密碼時，會建立稱為 “defaultuser1” 的低權限暫時帳戶。 此帳戶用來保護密碼重設程序。 此帳戶本身有隨機產生的密碼，該密碼不會針對裝置登入顯示，而在使用者重設其密碼後會自動移除。 可能存在多個 “defaultuser” 設定檔，但您可以放心忽略。

## <a name="limitations"></a>限制

使用 Hyper-V 測試這項功能時，不會出現 [重設密碼] 連結。

* 移至您用來測試的 VM，按一下 [檢視]，然後取消核取 [增強的工作階段]。

使用遠端桌面或增強的 VM 工作階段測試這項功能時，不會出現 [重設密碼] 連結。

* 目前不支援從遠端桌面進行密碼重設。

如果 1809 前的 Windows 10 版本中的原則需要 Ctrl+Alt+Del，[重設密碼] 將無法運作。

如果鎖定螢幕通知已關閉，[重設密碼] 將無法運作。

已知下列原則設定會影響重設密碼的能力

   * HideFastUserSwitching 設定為已啟用或 1
   * DontDisplayLastUserName 設定為已啟用或 1
   * NoLockScreen 設定為已啟用或 1
   * EnableLostMode 已設定於裝置上
   * Explorer.exe 會取代為自訂殼層

此功能不適用於已部署 802.1x 網路驗證及使用 [在使用者登入後立即執行] 選項的網路。 針對已部署 802.1x 網路驗證的網路，建議您使用機器驗證來啟用此功能。

在「已加入混合式網域」的案例中，SSPR 工作流程不需 Active Directory 網域控制站即可順利完成。 如果使用者在無法與 Active Directory 網域控制站通訊時 (像是在遠端工作時) 完成密碼重設程序，則直到裝置可與網域控制站通訊並更新快取的認證，使用者才能登入裝置。 **第一次使用新密碼時，必須要能夠與網域控制站連線**。

## <a name="clean-up-resources"></a>清除資源

如果您決定不再使用您在本教學課程中設定的功能，請刪除您所建立的 Intune 裝置組態設定檔或登錄機碼。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您讓使用者能夠從 Windows 10 登入畫面重設其密碼。 請繼續進行下一個教學課程，以了解如何將 Azure Identity Protection 整合到自助式密碼重設和 Multi-Factor Authentication 體驗中。

> [!div class="nextstepaction"]
> [在登入時評估風險](tutorial-risk-based-sspr-mfa.md)

[Assignment]: ./media/tutorial-sspr-windows/profile-assignment.png "將 Intune 裝置設定原則指派給 Windows 10 裝置群組"
[LoginScreen]: ./media/tutorial-sspr-windows/logon-reset-password.png "Windows 10 登入畫面上的重設密碼連結"
