---
title: 快速入門：從知識庫取得答案 - REST (Node.js) - QnA Maker
titlesuffix: Azure Cognitive Services
description: 這個以 Node.js REST 為基礎的快速入門會逐步引導您以程式設計方式從知識庫取得答案。
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 11/19/2018
ms.author: diberry
ms.openlocfilehash: fd6f6cd9b0078533541c9d846281c48a446f9c4a
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/07/2019
ms.locfileid: "55883095"
---
# <a name="get-answers-to-a-question-from-a-knowledge-base-with-nodejs"></a>使用 Node.js 從知識庫取得答案

本快速入門會逐步引導您以程式設計方式從已發佈的 QnA Maker 知識庫取得答案。 QnA Maker 會從[資料來源](../Concepts/data-sources-supported.md)中的半結構化內容 (如常見問題集) 自動擷取問題和答案。 問題會以 JSON 格式在 API 要求的本文中傳送。 

## <a name="prerequisites"></a>必要條件

* [Node.js](https://nodejs.org/en/download/)
* [Visual Studio Code](https://code.visualstudio.com/)
* 您必須有 [QnA Maker 服務](../How-To/set-up-qnamaker-service-azure.md)。 若要擷取您的金鑰，請在 QnA Maker 資源的 Azure 儀表板中選取 [資源管理] 下方的 [金鑰]。 
* [發佈] 頁面設定。 如果您沒有已發佈的知識庫，請建立空白的知識庫，在 [設定] 頁面上匯入知識庫，然後發佈。 您可以下載並使用[這個基本知識庫](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv)。 

    發佈頁面設定包括 POST 路由值、主機值以及 EndpointKey 值。 

    ![發佈設定](../media/qnamaker-quickstart-get-answer/publish-settings.png)

本快速入門的程式碼位於 [https://github.com/Azure-Samples/cognitive-services-qnamaker-nodejs](https://github.com/Azure-Samples/cognitive-services-qnamaker-nodejs/tree/master/documentation-samples/quickstarts/get-answer) 存放庫中。 

## <a name="create-a-nodejs-file"></a>建立 Node.js 檔案

開啟 VSCode 並建立名為 `get-answer.js` 的新檔案。 

## <a name="add-the-required-dependencies"></a>新增必要的相依性

在 `get-answer.js` 檔案的頂端，將必要的相依性新增至專案：

[!code-nodejs[Add the required dependencies](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/get-answer/get-answer.js?range=1-4 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>新增必要的常數

接下來，新增必要常數以存取 QnA Maker。 在您發佈知識庫之後，這些值都位於 [發佈] 頁面上。 

[!code-nodejs[Add the required constants](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/get-answer/get-answer.js?range=6-22 "Add the required constants")]

## <a name="add-a-post-request-to-send-question-and-get-an-answer"></a>新增 POST 要求以傳送問題和取得答案

下列程式碼會對 QnA Maker API 提出 HTTPS 要求，以將問題傳送到知識庫並接收回應：

[!code-nodejs[Add a POST request to send question to knowledge base](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/get-answer/get-answer.js?range=24-49 "Add a POST request to send question to knowledge base")]

`Authorization` 標頭的值包含字串 `EndpointKey `。 

## <a name="install-the-dependencies"></a>安裝相依性

從命令列安裝相依性：

```bash
npm install request request-promise
```

## <a name="run-the-program"></a>執行程式

從命令列執行程式。 程式會自動將要求傳送至 QnA Maker API，然後在主控台視窗中輸出。

執行檔案：

```bash
node get-answer.js
```

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)]

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [QnA Maker (V4) REST API 參考](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
