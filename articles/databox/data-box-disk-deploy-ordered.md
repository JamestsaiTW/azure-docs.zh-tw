---
title: 訂購 Microsoft Azure 資料箱磁碟的教學課程 | Microsoft Docs
description: 使用本教學課程，了解如何註冊和訂購 Azure 資料箱磁碟，以將資料匯入 Azure 中。
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.date: 01/09/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: db10361707d83fcda20f0e4bf2adc2abc4176808
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/09/2019
ms.locfileid: "54156166"
---
# <a name="tutorial-order-an-azure-data-box-disk"></a>教學課程：訂購 Azure 資料箱磁碟

Azure 資料箱磁碟是一項混合式雲端解決方案，可讓您以快速、簡便而可靠的方式，將內部部署資料匯入 Azure 中。 您可以將資料傳輸至由 Microsoft 提供的固態硬碟 (SSD)，並將磁碟寄回。 這項資料隨後會上傳至 Azure。

本教學課程說明如何訂購 Azure 資料箱磁碟。 在本教學課程中，您會了解：

> [!div class="checklist"]
> * 訂購資料箱磁碟
> * 追蹤訂單狀態
> * 取消訂單

## <a name="prerequisites"></a>必要條件

在您部署之前，請先完成資料箱服務與資料相磁碟的下列組態必要條件。

### <a name="for-service"></a>針對服務

在您開始前，請確定：
- 您擁有的 Microsoft Azure 儲存體帳戶具有存取認證。
- 請確定您用於資料箱服務的訂用帳戶是下列其中一種類型：
    - Microsoft Enterprise 合約 (EA)。 深入了解 [EA 訂用帳戶](https://azure.microsoft.com/pricing/enterprise-agreement/)。
    - 雲端解決方案提供者 (CSP)。 深入了解 [Azure CSP 方案](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview)。
- 請確定您有訂用帳戶的擁有者或參與者存取權，才能建立資料箱訂單。

### <a name="for-device"></a>針對裝置

在您開始前，請確定：
- 您有一部用戶端電腦可用來複製資料。 您的用戶端電腦必須：
    - 執行[支援的作業系統](data-box-disk-system-requirements.md#supported-operating-systems-for-clients)。
    - 安裝其他[必要軟體](data-box-disk-system-requirements.md#other-required-software-for-windows-clients) (如果是 Windows 用戶端)。  

## <a name="order-data-box-disk"></a>訂購資料箱磁碟

請在 [Azure 入口網站](https://aka.ms/azuredataboxfromdiskdocs)中執行下列步驟，以訂購資料箱磁碟。

1. 在入口網站的左上角按一下 [+ 建立資源]，然後搜尋 [Azure 資料箱]。 按一下 [Azure 資料箱]。
    
   ![搜尋 Azure 資料箱 1](media/data-box-disk-deploy-ordered/search-data-box11.png)

2. 按一下頁面底部的 [新增] 。

3. 確認您的區域是否適用資料箱服務。 輸入或選取下列資訊，然後按一下 [套用]。

    ![選取 [資料箱磁碟] 選項](media/data-box-disk-deploy-ordered/select-data-box-sku-1.png)

    |設定|值|
    |---|---|
    |訂用帳戶|選取要啟用資料箱服務的訂用帳戶。<br> 訂用帳戶會連結到您的帳單帳戶。 |
    |傳輸類型| 匯入至 Azure|
    |來源國家/地區 | 選取您的資料目前所在的國家/地區。|
    |目的地 Azure 區域|選取要傳輸資料的 Azure 區域。|

  
5.  選取 [資料箱磁碟]。 在 5 個磁碟的單一訂單中，解決方案的容量上限為 35 TB。 您可以建立多份訂單以訂購更大的資料大小。

     ![選取 [資料箱磁碟] 選項](media/data-box-disk-deploy-ordered/select-data-box-sku-zoom.png)

6.  在 [訂單] 中，指定 [訂單詳細資料]。 輸入或選取下列資訊。

    |設定|值|
    |---|---|
    |Name|提供用來追蹤訂單的易記名稱。<br> 名稱長度可介於 3 到 24 個字元之間，且可以是字母、數字和連字號。 <br> 名稱必須以字母或數字為開頭或結尾。 |
    |資源群組| 使用現有的群組或建立新群組。 <br> 資源群組是適用於資源而可一併管理或部署的邏輯容器： |
    |目的地 Azure 區域| 選取儲存體帳戶的區域。<br> 目前支援全美國、西歐和北歐、加拿大及澳大利亞地區的儲存體帳戶。 |
    |儲存體帳戶|根據指定的 Azure 區域，從現有儲存體帳戶的篩選清單中進行選取。 <br>您也可以建立新的一般用途 v1 或一般用途 v2 帳戶。 |
    |預估資料大小 (TB)| 輸入以 TB 為單位的估計值。 <br>Microsoft 會根據資料大小為您傳送適當數目的 8 TB SSD (可用容量為 7 TB)。 <br>5 個磁碟的可用容量上限為 35 TB。 |
    |磁碟通行金鑰| 如果您核取了 [使用自訂金鑰而非 Azure 產生的通行金鑰]，請提供磁碟通行金鑰。 <br> 請提供 12 到 32 個字元的英數字元金鑰，且至少須包含一個數字和一個特殊字元。 允許的特殊字元為 `@?_+`。 <br> 您可以選擇略過此選項並使用 Azure 產生的通行金鑰，將您的磁碟解除鎖定。|

13. 按 [下一步] 。 

    ![提供訂單詳細資料](media/data-box-disk-deploy-ordered/data-box-order-details.png)

14. 在 [交貨地址] 索引標籤中，提供您的姓名、公司的名稱和郵寄地址，以及有效的電話號碼。 按一下 [驗證地址]。 服務會驗證交貨地址以確認服務可用性。 如果服務可提供至指定的交貨地址，您將會收到該項通知。 

    ![提供交貨地址](media/data-box-disk-deploy-ordered/data-box-shipping-address.png)
15. 在 [通知詳細資料] 中，指定電子郵件地址。 服務會將關於任何訂單狀態更新的電子郵件通知傳送至指定的電子郵件地址。 

    建議您使用群組電子郵件，以便在群組中的管理員離開時繼續接收通知。

16. 檢閱與訂單、連絡人、通知和隱私權條款有關的資訊**摘要**。 請勾選隱私權條款合約的對應方塊。

17. 按一下 [訂單]。 建立訂單需要幾分鐘的時間。

 
## <a name="track-the-order"></a>追蹤訂單狀態

在您下訂單之後，可以從 Azure 入口網站來追蹤訂單狀態。 請移至您的訂單，然後移至 [概觀] 以檢視狀態。 入口網站會顯示處於 [已訂購] 狀態的作業。 

![已訂購的資料箱磁碟狀態](media/data-box-disk-deploy-ordered/data-box-portal-ordered.png) 

如果磁碟無法使用，您會收到通知。 如果可使用磁碟，Microsoft 會識別要寄送的磁碟，並準備磁碟包裹。 在磁碟準備期間，會執行下列動作：

- 使用 AES-128 BitLocker 加密將磁碟加密。  
- 鎖定磁碟，以防止未經授權者存取磁碟。
- 在此程序期間，會產生解除鎖定磁碟的通行金鑰。

磁碟準備完成後，入口網站會顯示訂單處於 [已處理] 狀態。

接著，Microsoft 會準備磁碟，並透過區域貨運公司派送給您。 在磁碟送達後，您會收到追蹤號碼。 入口網站會顯示訂單處於 [已分派] 狀態。

## <a name="cancel-the-order"></a>取消訂單

若要取消此訂單，請在 Azure 入口網站中移至 [概觀]，然後從命令列按一下 [取消]。 

在訂購磁碟後，您只能在訂單仍在進行出貨處理時取消訂單。 一旦訂單已處理完畢，您即無法取消訂單。 

![取消訂單](media/data-box-disk-deploy-ordered/cancel-order1.png)

若要刪除已取消的訂單，請移至 [概觀]，然後從命令列按一下 [刪除]。


## <a name="next-steps"></a>後續步驟

在本教學課程中，您已了解 Azure 資料箱的相關主題，像是：

> [!div class="checklist"]
> * 訂購資料箱磁碟
> * 追蹤訂單狀態
> * 取消訂單

請繼續進行下一個教學課程，以了解如何設定資料箱磁碟。

> [!div class="nextstepaction"]
> [設定您的 Azure 資料箱磁碟](./data-box-disk-deploy-set-up.md)


