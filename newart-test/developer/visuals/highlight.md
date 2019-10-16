---
title: Выделение
description: Ведение выбранных точек данных в визуальных элементах Power BI
author: KesemSharabi
ms.author: kesharab
manager: rkarlin
ms.reviewer: sranins
ms.service: powerbi
ms.subservice: powerbi-custom-visuals
ms.topic: conceptual
ms.date: 06/18/2019
ms.openlocfilehash: 4ff2187dc99d4e790b08c11f55a37e31e85693ad
ms.sourcegitcommit: a1c994bc8fa5f072fce13a6f35079e87d45f00f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72424282"
---
# <a name="highlight-data-points-in-power-bi-visuals"></a><span data-ttu-id="59e81-103">Выделение точек данных в визуальных элементах Power BI</span><span class="sxs-lookup"><span data-stu-id="59e81-103">Highlight data points in Power BI Visuals</span></span>

<span data-ttu-id="59e81-104">По умолчанию при каждом выборе элемента массив `values` в объекте `dataView` будет отфильтрован до одних выбранных значений.</span><span class="sxs-lookup"><span data-stu-id="59e81-104">By default whenever an element is selected the `values` array in the `dataView` object will be filtered to just the selected values.</span></span> <span data-ttu-id="59e81-105">Это приведет к тому, что все остальные визуальные элементы на странице будут отображать только выбранные данные.</span><span class="sxs-lookup"><span data-stu-id="59e81-105">It will cause all other visuals on the page to display just the selected data.</span></span>

![работа выделения "dataview" по умолчанию](./media/highlight-dataview.png)

<span data-ttu-id="59e81-107">Если задать для свойства `supportsHighlight` в `capabilities.json` значение `true`, вы получите полный нефильтрованный массив `values` вместе с массивом `highlights`.</span><span class="sxs-lookup"><span data-stu-id="59e81-107">If you set the `supportsHighlight` property in your `capabilities.json` to `true`, you'll receive the full unfiltered `values` array along with a `highlights` array.</span></span> <span data-ttu-id="59e81-108">Массив `highlights` будет иметь ту же длину, что и массив values, а все невыбранные значения будут установлены в `null`.</span><span class="sxs-lookup"><span data-stu-id="59e81-108">The `highlights` array will be the same length as the values array and any non-selected values will be set to `null`.</span></span> <span data-ttu-id="59e81-109">Если это свойство включено, визуальный элемент отвечает за выделение соответствующих данных путем сравнения массива `values` с массивом `highlights`.</span><span class="sxs-lookup"><span data-stu-id="59e81-109">With this property enabled it's the visual's responsibility to highlight the appropriate data by comparing the `values` array to the `highlights` array.</span></span>

![выделение "dataview" в поддержкой выделения](./media/highlight-dataview-supports.png)

<span data-ttu-id="59e81-111">В этом примере вы заметите 1 выбранную полосу.</span><span class="sxs-lookup"><span data-stu-id="59e81-111">In the example, you'll notice the 1 bar that is selected.</span></span> <span data-ttu-id="59e81-112">И это единственное значение в массиве highlights.</span><span class="sxs-lookup"><span data-stu-id="59e81-112">And it's the only value in the highlights array.</span></span> <span data-ttu-id="59e81-113">Также важно отметить, что могут иметь место несколько выбранных значений и частичное выделение.</span><span class="sxs-lookup"><span data-stu-id="59e81-113">It's also important to note there could be multiple selections and partial highlight.</span></span> <span data-ttu-id="59e81-114">В этом случае соответствующее числовое значение в массивах values и highlights будет присутствовать, но различаться.</span><span class="sxs-lookup"><span data-stu-id="59e81-114">There's the corresponding numeric value in the values and highlights arrays will be present but different.</span></span>
