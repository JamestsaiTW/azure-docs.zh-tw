---
author: vladvino
ms.service: api-management
ms.topic: include
ms.date: 11/09/2018
ms.author: vlvinogr
ms.openlocfilehash: 2bfa356deeede1c16bd5a464ea7081132a67faf6
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2018
ms.locfileid: "51571867"
---
## <a name="append-other-apis"></a>附加其他 API

API 可由不同服務所公開的 API 組成，包括 OpenAPI 規格、SOAP API、Azure App Service 的 API 應用程式功能、Azure 函式應用程式、Azure Logic Apps 和 Azure Service Fabric。

![匯入 API](./media/api-management-append-apis/import.png)

若要將不同的 API 附加至您現有的 API，請完成下列步驟。 在您匯入另一個 API 後，作業就會附加至目前的 API。

1. 在 Azure 入口網站中移至您的 Azure API 管理執行個體。
2. 從左側功能表中選取 [API]。
3. 在您要附加另一個 API 的 API 旁，按一下 **...**。
4. 從下拉式功能表中選取 [匯入]。
5. 選取要從中匯入 API 的服務。