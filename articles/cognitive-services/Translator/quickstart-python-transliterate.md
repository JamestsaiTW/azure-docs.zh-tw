---
title: 快速入門：直譯文字 (Python) - 翻譯工具文字 API
titleSuffix: Azure Cognitive Services
description: 在本快速入門中，您會了解如何搭配使用 Python 和翻譯工具文字 REST API，將文字從一個字集直譯 (轉換) 成另一個字集。 在此範例中，日文會直譯為使用拉丁字母。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 02/21/2019
ms.author: erhopf
ms.openlocfilehash: 2022971d24f7ac8a24986f45031f568a86fc31d9
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/23/2019
ms.locfileid: "56726358"
---
# <a name="quickstart-use-the-translator-text-api-to-transliterate-text-using-python"></a>快速入門：搭配使用翻譯工具文字 API 與 Python 來直譯文字

在本快速入門中，您會了解如何搭配使用 Python 和翻譯工具文字 REST API，將文字從一個字集直譯 (轉換) 成另一個字集。 在所提供的範例中，日文會直譯為使用拉丁字母。

本快速入門需要 [Azure 認知服務帳戶](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)和翻譯工具文字資源。 如果您還沒有帳戶，可以使用[免費試用](https://azure.microsoft.com/try/cognitive-services/)來取得訂用帳戶金鑰。

## <a name="prerequisites"></a>必要條件

本快速入門需要：

* Python 2.7.x 或 3.x
* 翻譯工具文字的 Azure 訂用帳戶金鑰

## <a name="create-a-project-and-import-required-modules"></a>建立專案，並匯入所需的模組

在您慣用的 IDE 或編輯器建立新的 Python 專案。 然後，將下列程式碼片段複製到您的專案中名為 `transliterate-text.py` 的檔案。

```python
# -*- coding: utf-8 -*-
import os, requests, uuid, json
```

> [!NOTE]
> 如果您未曾使用這些模組，則必須先加以安裝，再執行您的程式。 若要安裝這些套件，請執行：`pip install requests uuid`。

第一個註解會指出您的 Python 解譯器應使用 UTF-8 編碼。 然後，所需的模組會匯入，並從環境變數中讀取您的訂用帳戶金鑰、建構 HTTP 要求、建立唯一的識別碼，並處理翻譯工具文字 API 所傳回的 JSON 回應。

## <a name="set-the-subscription-key-base-url-and-path"></a>設定訂用帳戶金鑰、基底 URL 和路徑

此範例會嘗試從環境變數 `TRANSLATOR_TEXT_KEY` 中讀取您的翻譯工具文字訂用帳戶金鑰。 如果您不熟悉環境變數，您可以將 `subscriptionKey` 設為字串，並註解化條件陳述式。

請將下列程式碼複製到您的專案中：

```python
# Checks to see if the Translator Text subscription key is available
# as an environment variable. If you are setting your subscription key as a
# string, then comment these lines out.
if 'TRANSLATOR_TEXT_KEY' in os.environ:
    subscriptionKey = os.environ['TRANSLATOR_TEXT_KEY']
else:
    print('Environment variable for TRANSLATOR_TEXT_KEY is not set.')
    exit()
# If you want to set your subscription key as a string, uncomment the line
# below and add your subscription key.
#subscriptionKey = 'put_your_key_here'
```

翻譯工具文字全域端點會設定為 `base_url`。 `path` 會設定 `transliterate` 路由，並指出我們要叫用第 3 版的 API。

`params` 用來設定輸入語言以及輸入和輸出字集。 在此範例中，我們會從日文翻譯為拉丁字母。

>[!NOTE]
> 如需關於端點、路由和要求參數的詳細資訊，請參閱[翻譯工具文字 API 3.0：音譯](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-transliterate)。

```python
base_url = 'https://api.cognitive.microsofttranslator.com'
path = '/transliterate?api-version=3.0'
params = '&language=ja&fromScript=jpan&toScript=latn'
constructed_url = base_url + path + params
```

## <a name="add-headers"></a>新增標頭

要驗證要求，最簡單的方式是將您的訂用帳戶金鑰以 `Ocp-Apim-Subscription-Key` 標頭的形式傳入，我們在此範例中即採用此方式。 或者，您可以將訂用帳戶金鑰替換為存取權杖，並將存取權杖連同 `Authorization` 標頭傳入，以驗證您的要求。 如需詳細資訊，請參閱[驗證](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication)。

請將下列程式碼片段複製到您的專案中：

```python
headers = {
    'Ocp-Apim-Subscription-Key': subscriptionKey,
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}
```

## <a name="create-a-request-to-transliterate-text"></a>建立直譯文字的要求

定義您要直譯的一或多個字串：

```python
# Transliterate "good afternoon" from source Japanese.
# Note: You can pass more than one object in body.
body = [{
    'text': 'こんにちは'
}]
```

接著，我們將使用 `requests` 模組建立 POST 要求。 此作業會採用三個引數：串連的 URL、要求標頭和要求本文：

```python
request = requests.post(constructed_url, headers=headers, json=body)
response = request.json()
```

## <a name="print-the-response"></a>列印回應

最後一個步驟是列印結果。 此程式碼片段會排序索引鍵、設定縮排，並宣告項目和索引鍵分隔符號，以美化結果。

```python
print(json.dumps(response, sort_keys=True, indent=4, ensure_ascii=False, separators=(',', ': ')))
```

## <a name="put-it-all-together"></a>組合在一起

如此，您就建立了一個簡單的程式，會呼叫翻譯工具文字 API 並傳回 JSON 回應。 現在，請執行您的程式：

```console
python transliterate-text.py
```

如果想要將您的程式碼與我們的做比較，請使用 [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python) 上提供的完整範例。

## <a name="sample-response"></a>範例回應

```json
[
    {
        "script": "latn",
        "text": "konnnichiha"
    }
]
```

## <a name="clean-up-resources"></a>清除資源

如果您已將訂用帳戶金鑰硬式編碼到您的程式中，請務必在完成本快速入門後移除訂用帳戶金鑰。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [瀏覽 GitHub 上的 Python 範例](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python) (英文)

## <a name="see-also"></a>另請參閱

了解如何使用翻譯工具文字 API 來執行下列動作：

* [翻譯文字](quickstart-python-translate.md)
* [從輸入識別語言](quickstart-python-detect.md)
* [取得替代的翻譯](quickstart-python-dictionary.md)
* [取得支援的語言清單](quickstart-python-languages.md)
* [從輸入判斷句子長度](quickstart-python-sentences.md)
