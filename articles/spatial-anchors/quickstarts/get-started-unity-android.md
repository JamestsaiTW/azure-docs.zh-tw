---
title: 快速入門 - 使用 Azure Spatial Anchors 建立 Android Unity 應用程式 | Microsoft Docs
description: 在本快速入門中，您將了解如何使用 Spatial Anchors 建置搭配 Unity 的 Android 應用程式。
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: e92c812ffc8b72fe79248c602e48ff01ef9fefcb
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2019
ms.locfileid: "56961002"
---
# <a name="quickstart-create-an-android-unity-app-with-azure-spatial-anchors"></a>快速入門：使用 Azure Spatial Anchors 建立 Android Unity 應用程式

本快速入門說明如何使用 [Azure Spatial Anchors](../overview.md) 建立 Android Unity 應用程式。 Azure Spatial Anchors 是一款跨平台開發人員服務，可讓您使用在一段時間之後仍跨裝置保持其位置的物件，建立混合實境體驗。 當您完成時，您將會有搭配 Unity 建置的 ARCore Android 應用程式，可用來儲存和回收空間錨點。

您將學習如何：

> [!div class="checklist"]
> * 建立 Spatial Anchors 帳戶
> * 準備 Unity 組建設定
> * 下載並匯入適用於 Unity 的 ARCore SDK
> * 設定 Spatial Anchors 帳戶識別碼和帳戶金鑰
> * 匯出 Android Studio 專案
> * 在 Android 裝置上部署和執行

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>必要條件

若要完成本快速入門，請確定您具備︰

- 已安裝 <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2018.3+</a> 和 <a href="https://developer.android.com/studio/" target="_blank">Android Studio 3.3+</a> 的 Windows 或 macOS 機器。
- <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">由開發人員啟用</a>且<a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">具備 ARCore 功能</a>的 Android 裝置。

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project-in-unity"></a>在 Unity 中開啟範例專案

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Android Unity Build Settings](../../../includes/spatial-anchors-unity-android-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>設定帳戶識別碼和金鑰

在 [專案] 窗格中瀏覽至 `Assets/AzureSpatialAnchorsPlugin/Examples`，然後開啟 `AzureSpatialAnchorsBasicDemo.unity` 場景檔案。

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

選取 [檔案] -> [儲存] 以儲存場景。

## <a name="export-the-android-studio-project"></a>匯出 Android Studio 專案

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

選取 [匯出] 以開啟對話方塊。 然後選取資料夾，以匯出 Android Studio 專案。

匯出完成時將會顯示一個資料夾，其中包含已匯出的 Android Studio 專案，和一個名為 **HelloAR U3D** 的子資料夾。

## <a name="deploy-the-android-application"></a>部署 Android 應用程式

開啟 Android Studio，並選取 [開啟現有的 Android Studio 專案]。 然後，從匯出的 Android Studio 專案中選取 **HelloAR U3D** 子資料夾，並按一下 [確定]。

在開啟時，會出現要求您使用 Gradle 包裝函式的提示。 請選取 [確定] 以使用 Gradle 包裝函式並開啟專案。

將 Android 裝置開機並登入，然後使用 USB 纜線將其連接到電腦。

在 Android Studio 工具列中選取 [執行]。

![Android Studio 的部署和執行](./media/get-started-unity-android/android-studio-deploy-run.png)

在 [選取部署目標] 對話方塊中選取 Android 裝置，然後選取 [確定] 以在 Android 裝置上執行應用程式。

依照應用程式中的指示放置及回收錨點。

> [!NOTE]
> 在執行應用程式時，如果您看到的背景不是相機 (例如，您看到空白、藍色或其他紋理的背景)，則可能需要在 Unity 中重新匯入資產。 請停止應用程式。 在 Unity 的頂端功能表中，選擇 [資產] -> [全部重新匯入]。 然後，再次執行應用程式。

在 Android Studio 工具列中選取 [停止]，以停止應用程式。

![Android Studio 的停止](./media/get-started-unity-android/android-studio-stop.png)

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [教學課程：跨裝置共用 Spatial Anchors](../tutorials/tutorial-share-anchors-across-devices.md)
