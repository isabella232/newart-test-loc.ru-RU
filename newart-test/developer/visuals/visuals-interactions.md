---
title: Взаимодействие визуальных элементов в Power BI
description: В этой статье описывается, как проверить, допускается ли взаимодействие визуальных элементов в Power BI.
author: KesemSharabi
ms.author: kesharab
manager: rkarlin
ms.reviewer: sranins
ms.service: powerbi
ms.subservice: powerbi-custom-visuals
ms.topic: conceptual
ms.date: 06/18/2019
ms.openlocfilehash: 0155f0fce1bc0fec5c96aef1c7e1dc9cf64b122f
ms.sourcegitcommit: a1c994bc8fa5f072fce13a6f35079e87d45f00f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72424642"
---
# <a name="visual-interactions-in-power-bi-visuals"></a>Взаимодействие визуальных элементов в Power BI

Визуальные элементы могут запрашивать значение флага `allowInteractions`, которое указывает, должен ли визуальный элемент допускать взаимодействие с визуальными элементами. Например, визуальные элементы допускают взаимодействие во время просмотра или редактирования отчета и не допускают его при просмотре на панели мониторинга. К таким видам взаимодействия относятся *щелчок*, *сдвиг*, *масштабирование*, *выделение* и другие. 

> [!NOTE]
> Независимо от настройки флага, необходимо включить всплывающие подсказки во всех сценариях.

Флаг `allowInteractions` передается в виде логического значения во время инициализации визуального элемента в качестве члена интерфейса IVisualHost.

В любом сценарии Power BI, требующем отсутствия взаимодействия визуальных элементов (например, для плиток панели мониторинга), для флага `allowInteractions` будет установлено значение `false`. В противном случае (например, для отчета), флагу `allowInteractions` присваивается значение `true`.

Дополнительные сведения см. в [репозитории визуальных элементов SampleBarChart](https://github.com/Microsoft/PowerBI-visuals-sampleBarChart/commit/59a47935d8f5272ce145fe804193599ddb7e2001).

```typescript
   ...
   let allowInteractions = options.host.allowInteractions;
   bars.on('click', function(d) {
       if (allowInteractions) {
           selectionManager.select(d.selectionId);
           ...
       }
   });
```
