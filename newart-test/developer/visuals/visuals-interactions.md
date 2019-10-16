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
# <a name="visual-interactions-in-power-bi-visuals"></a><span data-ttu-id="40866-103">Взаимодействие визуальных элементов в Power BI</span><span class="sxs-lookup"><span data-stu-id="40866-103">Visual interactions in Power BI visuals</span></span>

<span data-ttu-id="40866-104">Визуальные элементы могут запрашивать значение флага `allowInteractions`, которое указывает, должен ли визуальный элемент допускать взаимодействие с визуальными элементами.</span><span class="sxs-lookup"><span data-stu-id="40866-104">Visuals can query the value of the `allowInteractions` flag, which indicates whether the visual should allow visual interactions.</span></span> <span data-ttu-id="40866-105">Например, визуальные элементы допускают взаимодействие во время просмотра или редактирования отчета и не допускают его при просмотре на панели мониторинга.</span><span class="sxs-lookup"><span data-stu-id="40866-105">For example, visuals are interactive during report viewing or editing, but not interactive when they're viewed in a dashboard.</span></span> <span data-ttu-id="40866-106">К таким видам взаимодействия относятся *щелчок*, *сдвиг*, *масштабирование*, *выделение* и другие.</span><span class="sxs-lookup"><span data-stu-id="40866-106">These interactions are *click*, *pan*, *zoom*, *selection*, and others.</span></span> 

> [!NOTE]
> <span data-ttu-id="40866-107">Независимо от настройки флага, необходимо включить всплывающие подсказки во всех сценариях.</span><span class="sxs-lookup"><span data-stu-id="40866-107">You should enable tooltips in all scenarios, regardless of which flag is indicated.</span></span>

<span data-ttu-id="40866-108">Флаг `allowInteractions` передается в виде логического значения во время инициализации визуального элемента в качестве члена интерфейса IVisualHost.</span><span class="sxs-lookup"><span data-stu-id="40866-108">The `allowInteractions` flag is passed as a Boolean during the initialization of the visual, as a member of the IVisualHost interface.</span></span>

<span data-ttu-id="40866-109">В любом сценарии Power BI, требующем отсутствия взаимодействия визуальных элементов (например, для плиток панели мониторинга), для флага `allowInteractions` будет установлено значение `false`.</span><span class="sxs-lookup"><span data-stu-id="40866-109">In any Power BI scenario that requires that the visuals not be interactive (for example, dashboard tiles), the `allowInteractions` flag is set to `false`.</span></span> <span data-ttu-id="40866-110">В противном случае (например, для отчета), флагу `allowInteractions` присваивается значение `true`.</span><span class="sxs-lookup"><span data-stu-id="40866-110">Otherwise (for example, Report), `allowInteractions` is set to `true`.</span></span>

<span data-ttu-id="40866-111">Дополнительные сведения см. в [репозитории визуальных элементов SampleBarChart](https://github.com/Microsoft/PowerBI-visuals-sampleBarChart/commit/59a47935d8f5272ce145fe804193599ddb7e2001).</span><span class="sxs-lookup"><span data-stu-id="40866-111">For more information, see the [SampleBarChart visual repository](https://github.com/Microsoft/PowerBI-visuals-sampleBarChart/commit/59a47935d8f5272ce145fe804193599ddb7e2001).</span></span>

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
