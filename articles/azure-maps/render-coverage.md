---
title: Azure 地圖中的轉譯涵蓋範圍 | Microsoft Docs
description: 了解 Azure 地圖中的轉譯涵蓋範圍
author: jingjing-z
ms.author: jinzh
ms.date: 03/22/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 536a74046f46c7f83907833846e9ec99e8d8a289
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2019
ms.locfileid: "58370273"
---
# <a name="azure-maps-render-coverage"></a>Azure 地圖轉譯涵蓋範圍

Azure 地圖會同時使用點陣圖格和向量圖格來建立地圖。 在其最低解析度下，整個世界會剛好放進單一圖格裡。 在其最高解析度下，單一圖格代表 38 平方公尺。 因此，當您放大地圖時，將可更詳細地查看大陸、區域、城市和個別街道。 如需詳細資訊，請參閱[縮放層級和圖格格線](zoom-levels-and-tile-grid.md)。

不過，地圖並未對所有區域提供相同層級的資訊和精確度。 下表提供各個區域預期應有之轉譯詳細程度的相關資訊。

## <a name="legend"></a>圖例

| 符號 | 意義 |
|--------|---------|
| ✓ | 以詳細資料呈現區域。   |
| Ø | 以簡化的資料呈現區域。 |


## <a name="africa"></a>非洲 


| 國家/區域 | 點陣圖格整合 | 向量圖格整合 |
| ------ | :------------------: | :------------------: |
| 阿爾及利亞                          | ✓ | ✓ |
| 安哥拉                           | ✓ | ✓ |
| 貝南                            | ✓ | ✓ |
| 波札那                         | ✓ | ✓ |
| 布吉納法索                     | ✓ | ✓ |
| 蒲隆地                          | ✓ | ✓ |
| 維德角                       | ✓ | ✓ |
| 喀麥隆                         | ✓ | ✓ |
| 中非共和國         | ✓ | Ø |
| 查德                             | ✓ | Ø |
| 葛摩                          | ✓ | Ø |
| 剛果                            | ✓ | ✓ |
| 剛果民主共和國 | ✓ | ✓ |
| 象牙海岸                    | ✓ | Ø |
| 吉布地                         | ✓ | Ø |
| 埃及                            | ✓ | ✓ |
| 赤道幾內亞                | ✓ | Ø |
| 厄利垂亞                          | ✓ | Ø |
| 衣索比亞                         | ✓ | Ø |
| 加彭                            | ✓ | ✓ |
| 甘比亞                           | ✓ | Ø |
| 迦納                            | ✓ | ✓ |
| 幾內亞                           | ✓ | Ø |
| 幾內亞比索                    | ✓ | Ø |
| 肯亞                            | ✓ | ✓ |
| 賴索托                          | ✓ | ✓ |
| 賴比瑞亞                          | ✓ | Ø |
| 利比亞                            | ✓ | Ø |
| 馬達加斯加                       | ✓ | Ø |
| 馬拉威                           | ✓ | ✓ |
| 馬利                             | ✓ | ✓ |
| 茅利塔尼亞                       | ✓ | ✓ |
| 模里西斯                        | ✓ | ✓ |
| 馬約特島                          | ✓ | ✓ |
| 摩洛哥                          | ✓ | ✓ |
| 莫三比克                       | ✓ | ✓ |
| 納米比亞                          | ✓ | ✓ |
| 尼日                            | ✓ | ✓ |
| 奈及利亞                          | ✓ | ✓ |
| 留尼旺                          | ✓ | ✓ |
| 盧安達                           | ✓ | ✓ |
| 聖赫勒拿、亞森欣、特里斯坦達庫尼亞群島 | ✓ | Ø |
| 聖多美普林西比            | ✓ | Ø |
| 塞內加爾                          | ✓ | ✓ |
| 獅子山                     | ✓ | ✓ |
| 索馬利亞                          | ✓ | ✓ |
| 南非                     | ✓ | ✓ |
| 南蘇丹                      | ✓ | ✓ |
| 蘇丹                            | ✓ | ✓ |
| 史瓦濟蘭                        | ✓ | ✓ |
| 坦尚尼亞聯合共和國      | ✓ | ✓ |
| 多哥                             | ✓ | ✓ |
| 突尼西亞                          | ✓ | ✓ |
| 烏干達                           | ✓ | ✓ |
| 尚比亞                           | ✓ | ✓ |
| 辛巴威                         | ✓ | ✓ |

## <a name="americas"></a>美洲

| 國家/區域 | 點陣圖格整合 | 向量圖格整合 |
| ------ | :------------------: | :------------------: |
| 安奎拉                  | ✓ | ✓ |
| 安地卡及巴布達       | ✓ | ✓ |
| 阿根廷                 | ✓ | ✓ |
| 荷屬阿魯巴                     | ✓ | ✓ |
| 巴哈馬                   | ✓ | ✓ |
| 巴貝多                  | ✓ | ✓ |
| 貝里斯                    | ✓ | ✓ |
| 百慕達                   | ✓ | ✓ |
| 玻利維亞多民族國 | ✓ | ✓ |
| 波奈、聖佑達修斯和沙巴 | ✓ | ✓ |
| 巴西                    | ✓ | ✓ |
| 加拿大                    | ✓ | ✓ |
| 開曼群島            | ✓ | ✓ |
| 智利                     | ✓ | ✓ |
| 克利珀頓島         | ✓ | ✓ |
| 哥倫比亞                  | ✓ | ✓ |
| 哥斯大黎加                | ✓ | ✓ |
| 古巴                      | ✓ | ✓ |
| 古拉果                   | ✓ | ✓ |
| 多米尼克                  | ✓ | ✓ |
| 多明尼加共和國        | ✓ | ✓ |
| 厄瓜多                   | ✓ | ✓ |
| 福克蘭群島 (瑪爾維納斯) | ✓ | ✓ |
| 法屬圭亞那             | ✓ | ✓ |
| 格陵蘭                 | ✓ | Ø |
| 格瑞那達                   | ✓ | ✓ |
| 哥德洛普                | ✓ | ✓ |
| 瓜地馬拉                 | ✓ | ✓ |
| 蓋亞那                    | ✓ | ✓ |
| 海地                     | ✓ | ✓ |
| 宏都拉斯                  | ✓ | ✓ |
| 牙買加                   | ✓ | ✓ |
| 馬丁尼克                | ✓ | ✓ |
| 墨西哥                    | ✓ | ✓ |
| 蒙哲臘                | ✓ | ✓ |
| 尼加拉瓜                 | ✓ | ✓ |
| 北馬利安納群島  | ✓ | ✓ |
| 巴拿馬                    | ✓ | ✓ | 
| 巴拉圭                  | ✓ | ✓ |
| 祕魯                      | ✓ | ✓ |
| 波多黎各               | ✓ | ✓ |
| 魁北克 (加拿大)           | ✓ | ✓ |
| 聖巴瑟米          | ✓ | ✓ |
| 聖克里斯多福及尼維斯     | ✓ | ✓ |
| 聖露西亞               | ✓ | ✓ |
| 法屬聖馬丁     | ✓ | ✓ |
| 聖皮埃與密克隆群島 | ✓ | ✓ |
| 聖文森及格瑞那丁 | ✓ | ✓ |
| 荷屬聖馬丁      | ✓ | ✓ |
| 南喬治亞與南三明治群島 | ✓ | ✓ |
| 蘇利南                  | ✓ | ✓ |
| 千里達及托巴哥       | ✓ | ✓ |
| 土克斯及開科斯群島  | ✓ | ✓ |
| 美國             | ✓ | ✓ |
| 烏拉圭                   | ✓ | ✓ |
| 委內瑞拉                 | ✓ | ✓ |
| 英屬維京群島   | ✓ | ✓ |
| 美屬維京群島      | ✓ | ✓ |

## <a name="asia"></a>亞洲 

| 國家/區域 | 點陣圖格整合 | 向量圖格整合 |
| ------ | :------------------: | :------------------: |
| 阿富汗               |   | Ø |
| 巴林                   | ✓ | ✓ |
| 孟加拉                |   | Ø |
| 不丹                    |   | Ø |
| 英屬印度洋領地 |   | Ø |
| 汶萊                    | ✓ | ✓ |
| 柬埔寨                  |   | Ø |
| 中國                     |   | Ø |
| 可可斯 (基林) 群島   |   | Ø |
| 朝鮮民主主義人民共和國 |   | Ø |
| 獨島和竹島       |   | Ø |
| 香港                 | ✓ | ✓ |
| 印度                     | Ø | ✓ | 
| 印尼                 | ✓ | ✓ |
| 伊朗                      |   | Ø |
| 伊拉克                      | ✓ | ✓ |
| 以色列                    |   | ✓ |
| 日本                     |   | Ø |
| 約旦                    | ✓ | ✓ |
| 哈薩克                |   | ✓ |
| 科威特                    | ✓ | ✓ |
| 吉爾吉斯                |   | Ø |
| 寮人民民主共和國 |   | Ø |
| 黎巴嫩                   | ✓ | ✓ |
| 澳門                     | ✓ | ✓ |
| 馬來西亞                  | ✓ | ✓ |
| 馬爾地夫                  |   | Ø |
| 蒙古                  |   | Ø |
| 緬甸                   |   | Ø |
| 尼泊爾                     |   | Ø |
| 阿曼                      | ✓ | ✓ |
| 巴基斯坦                  |   | Ø |
| 菲律賓               | ✓ | ✓ |
| 卡達                     | ✓ | ✓ |
| 大韓民國         | ✓ | Ø |
| 沙烏地阿拉伯              | ✓ | ✓ |
| 釣魚台群島           |   | ✓ |
| 新加坡                 | ✓ | ✓|
| 斯里蘭卡                 |   | Ø |
| 敘利亞阿拉伯共和國      |   | Ø |
| 台灣                    | ✓ | ✓ |
| 塔吉克                |   | Ø |
| 泰國                  | ✓ | ✓ |
| 東帝汶               |   | Ø |
| 土庫曼              |   | Ø |
| 阿拉伯聯合大公國      | ✓ | ✓ |
| 美國本土外小島嶼 |   | Ø |
| 烏茲別克                |   | Ø |
| 越南                   | ✓ | ✓ |
| 葉門                     | ✓ | ✓ |

## <a name="oceania"></a>大洋洲

| 國家/區域 | 點陣圖格整合 | 向量圖格整合 |
| ------ | :------------------: | :------------------: |
| 美屬薩摩亞            |   | ✓ |
| 澳大利亞                 | ✓ | ✓ |
| 庫克群島              |   | Ø |
| 斐濟                      |   | Ø |
| 法屬玻里尼西亞          |   | Ø |
| 關島                      | ✓ | ✓ |
| 吉里巴斯                  |   | Ø |
| 馬紹爾群島          |   | Ø |
| 密克羅尼西亞                |   | Ø |
| 諾魯                     |   | Ø |
| 新喀里多尼亞群島             |   | Ø |
| 紐西蘭               | ✓ | ✓ |
| 紐埃島                      |   | Ø |
| 諾福克島            |   | Ø |
| 帛琉                     |   | Ø |
| 巴布亞紐幾內亞          |   | Ø |
| 皮特康                  |   | Ø |
| 薩摩亞                     |   | Ø |
| 索羅門群島           |   | Ø|
| 托克勞                   |   | Ø |
| 東加                     |   | Ø |
| 吐瓦魯                    |   | Ø |
| 萬那杜                   |   | Ø |
| 瓦利斯及福杜納群島         |   | Ø |


## <a name="europe"></a>歐洲

| 國家/區域 | 點陣圖格整合 | 向量圖格整合 |
| ------ | :------------------: | :------------------: |
| 阿爾巴尼亞                   | ✓ | ✓ |
| 安道爾                   | ✓ | ✓ |
| 亞美尼亞                   | ✓ | Ø |
| 奧地利                   | ✓ | ✓ |
| 亞塞拜然                | ✓ | Ø |
| 白俄羅斯                   | Ø | ✓ |
| 比利時                   | ✓ | ✓ |
| 波士尼亞與赫塞哥維納        | ✓ | ✓ |
| 保加利亞                  | ✓ | ✓ |
| 克羅埃西亞                   | ✓ | ✓ |
| 賽浦路斯                    | ✓ | ✓ |
| 捷克共和國            | ✓ | ✓ |
| 丹麥                   | ✓ | ✓ |
| 愛沙尼亞                   | ✓ | ✓ |
| 法羅群島             | ✓ | Ø |
| 芬蘭                   | ✓ | ✓ |
| 法國                    | ✓ | ✓ |
| 喬治亞                   | ✓ | Ø |
| 德國                   | ✓ | ✓ |
| 直布羅陀                 | ✓ | ✓ |
| 希臘                    | ✓ | ✓ |
| 根息                  | ✓ | ✓ |
| 匈牙利                   | ✓ | ✓ |
| 冰島                   | ✓ | ✓ |
| 愛爾蘭                   | ✓ | ✓ |
| 曼島               | ✓ | ✓ |
| 義大利                     | ✓ | ✓ |
| 央棉                 | ✓ | ✓ |
| 澤西島                    | ✓ | ✓ |
| 拉脫維亞                    | ✓ | ✓ |
| 列支敦斯登             | ✓ | ✓ |
| 立陶宛                 | ✓ | ✓ |
| 盧森堡                | ✓ | ✓ |
| 馬其頓                 | ✓ | ✓ |
| 馬爾他                     | ✓ | ✓ |
| 摩爾多瓦                   | ✓ | ✓ |
| 摩納哥                    | ✓ | ✓ |
| 蒙特內哥羅                | ✓ | ✓ |
| 荷蘭               | ✓ | ✓ |
| 挪威                    | ✓ | ✓ |
| 波蘭                    | ✓ | ✓ |
| 葡萄牙                  | ✓ | ✓ |
| 羅馬尼亞                   | ✓ | ✓ |
| 俄羅斯聯邦        | ✓ | ✓ |
| 聖馬利諾                | ✓ | ✓ |
| 塞爾維亞                    | ✓ | ✓ |
| 斯洛伐克                  | ✓ | ✓ |
| 斯洛維尼亞                  | ✓ | ✓ |
| 南千島群島           | ✓ | ✓ |
| 西班牙                     | ✓ | ✓ |
| 冷岸                  | ✓ | ✓ |
| 瑞典                    | ✓ | ✓ |
| 瑞士               | ✓ | ✓ |
| 土耳其                    | ✓ | ✓ |
| 烏克蘭                   | ✓ | ✓ |
| 英國            | ✓ | ✓ |
| 梵蒂岡              | ✓ | ✓ |

## <a name="next-steps"></a>後續步驟

如需關於 Azure 地圖轉譯的詳細資訊，請參閱[縮放層級和圖格格線](zoom-levels-and-tile-grid.md)。

了解[地圖路由服務的涵蓋區域](routing-coverage.md)。 