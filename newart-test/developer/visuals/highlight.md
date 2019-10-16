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
# <a name="highlight-data-points-in-power-bi-visuals"></a>Выделение точек данных в визуальных элементах Power BI

По умолчанию при каждом выборе элемента массив `values` в объекте `dataView` будет отфильтрован до одних выбранных значений. Это приведет к тому, что все остальные визуальные элементы на странице будут отображать только выбранные данные.

![работа выделения "dataview" по умолчанию](./media/highlight-dataview.png)

Если задать для свойства `supportsHighlight` в `capabilities.json` значение `true`, вы получите полный нефильтрованный массив `values` вместе с массивом `highlights`. Массив `highlights` будет иметь ту же длину, что и массив values, а все невыбранные значения будут установлены в `null`. Если это свойство включено, визуальный элемент отвечает за выделение соответствующих данных путем сравнения массива `values` с массивом `highlights`.

![выделение "dataview" в поддержкой выделения](./media/highlight-dataview-supports.png)

В этом примере вы заметите 1 выбранную полосу. И это единственное значение в массиве highlights. Также важно отметить, что могут иметь место несколько выбранных значений и частичное выделение. В этом случае соответствующее числовое значение в массивах values и highlights будет присутствовать, но различаться.
