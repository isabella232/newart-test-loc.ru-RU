---
title: Получение дополнительных данных из Power BI
description: В этой статье описывается включение сегментированного получения больших наборов данных для визуальных элементов Power BI.
author: KesemSharabi
ms.author: kesharab
manager: rkarlin
ms.reviewer: sranins
ms.service: powerbi
ms.subservice: powerbi-custom-visuals
ms.topic: conceptual
ms.date: 06/18/2019
ms.openlocfilehash: b67977abd93b3aa605430dd2d7fb516ca33bd51c
ms.sourcegitcommit: a1c994bc8fa5f072fce13a6f35079e87d45f00f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72424177"
---
# <a name="fetch-more-data-from-power-bi"></a><span data-ttu-id="4a322-103">Получение дополнительных данных из Power BI</span><span class="sxs-lookup"><span data-stu-id="4a322-103">Fetch more data from Power BI</span></span>

<span data-ttu-id="4a322-104">В этой статье описывается, как загрузить дополнительные данные, чтобы обойти ограничение в 30 КБ, установленное для точки данных.</span><span class="sxs-lookup"><span data-stu-id="4a322-104">This article discusses how to load more data to bypass the hard limit of a 30-KB data point.</span></span> <span data-ttu-id="4a322-105">При таком подходе данные предоставляются в виде блоков.</span><span class="sxs-lookup"><span data-stu-id="4a322-105">This approach provides data in chunks.</span></span> <span data-ttu-id="4a322-106">Чтобы повысить производительность, можно настроить размер блока в соответствии с конкретными требованиями.</span><span class="sxs-lookup"><span data-stu-id="4a322-106">To improve performance, you can configure the chunk size to accommodate your use case.</span></span>  

## <a name="enable-a-segmented-fetch-of-large-datasets"></a><span data-ttu-id="4a322-107">Включение сегментированного получения больших наборов данных</span><span class="sxs-lookup"><span data-stu-id="4a322-107">Enable a segmented fetch of large datasets</span></span>

<span data-ttu-id="4a322-108">Для сегментного режима `dataview` определите размер окна для dataReductionAlgorithm в файле *capabilities.json* визуального элемента для требуемого dataViewMapping.</span><span class="sxs-lookup"><span data-stu-id="4a322-108">For the `dataview` segment mode, you define a window size for dataReductionAlgorithm in the visual's *capabilities.json* file for the required dataViewMapping.</span></span> <span data-ttu-id="4a322-109">`count` определяет размер окна, ограничивающий количество новых строк данных, добавляемых к `dataview` при каждом обновлении.</span><span class="sxs-lookup"><span data-stu-id="4a322-109">The `count` determines the window size, which limits the number of new data rows that can be appended to the `dataview` in each update.</span></span>

<span data-ttu-id="4a322-110">Добавьте следующий код в файл *capabilities.json*:</span><span class="sxs-lookup"><span data-stu-id="4a322-110">Add the following code in the *capabilities.json* file:</span></span>

```typescript
"dataViewMappings": [
    {
        "table": {
            "rows": {
                "for": {
                    "in": "values"
                },
                "dataReductionAlgorithm": {
                    "window": {
                        "count": 100
                    }
                }
            }
    }
]
```

<span data-ttu-id="4a322-111">Новые сегменты добавляются к существующему `dataview` и предоставляются визуальному элементу в виде вызова `update`.</span><span class="sxs-lookup"><span data-stu-id="4a322-111">New segments are appended to the existing `dataview` and provided to the visual as an `update` call.</span></span>

## <a name="usage-in-the-power-bi-visual"></a><span data-ttu-id="4a322-112">Использование в визуальном элементе Power BI</span><span class="sxs-lookup"><span data-stu-id="4a322-112">Usage in the Power BI visual</span></span>

<span data-ttu-id="4a322-113">Чтобы определить, существуют ли данные, проверьте наличие `dataView.metadata.segment`:</span><span class="sxs-lookup"><span data-stu-id="4a322-113">You can determine whether data exists by checking the existence of `dataView.metadata.segment`:</span></span>

```typescript
    public update(options: VisualUpdateOptions) {
        const dataView = options.dataViews[0];
        console.log(dataView.metadata.segment);
        // output: __proto__: Object
    }
```

<span data-ttu-id="4a322-114">С помощью `options.operationKind` вы также можете проверить, является ли это обновление первичным или очередным.</span><span class="sxs-lookup"><span data-stu-id="4a322-114">You can also check to see whether it's the first update or a subsequent update by checking `options.operationKind`.</span></span> <span data-ttu-id="4a322-115">В следующем коде `VisualDataChangeOperationKind.Create` ссылается на первый сегмент, а `VisualDataChangeOperationKind.Append` — на последующие сегменты.</span><span class="sxs-lookup"><span data-stu-id="4a322-115">In the following code, `VisualDataChangeOperationKind.Create` refers to the first segment, and `VisualDataChangeOperationKind.Append` refers to subsequent segments.</span></span>

<span data-ttu-id="4a322-116">Пример реализации см. в следующем фрагменте кода:</span><span class="sxs-lookup"><span data-stu-id="4a322-116">For a sample implementation, see the following code snippet:</span></span>

```typescript
// CV update implementation
public update(options: VisualUpdateOptions) {
    // indicates this is the first segment of new data.
    if (options.operationKind == VisualDataChangeOperationKind.Create) {

    }

    // on second or subsequent segments:
    if (options.operationKind == VisualDataChangeOperationKind.Append) {

    }

    // complete update implementation
}
```

<span data-ttu-id="4a322-117">Можно также вызвать метод `fetchMoreData` из обработчика событий пользовательского интерфейса, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="4a322-117">You can also invoke the `fetchMoreData` method from a UI event handler, as shown here:</span></span>

```typescript
btn_click(){
{
    // check if more data is expected for the current data view
    if (dataView.metadata.segment) {
        //request for more data if available; as a response, Power BI will call update method
        let request_accepted: bool = this.host.fetchMoreData();
        // handle rejection
        if (!request_accepted) {
            // for example, when the 100 MB limit has been reached
        }
    }
}
```

<span data-ttu-id="4a322-118">В ответ на вызов метода `this.host.fetchMoreData` Power BI вызывает метод `update` визуального элемента с новым сегментом данных.</span><span class="sxs-lookup"><span data-stu-id="4a322-118">As a response to calling the `this.host.fetchMoreData` method, Power BI calls the `update` method of the visual with a new segment of data.</span></span>

> [!NOTE]
> <span data-ttu-id="4a322-119">Power BI ограничит общий объем получаемых данных значением 100 МБ, чтобы предотвратить нехватку памяти на клиенте.</span><span class="sxs-lookup"><span data-stu-id="4a322-119">To avoid client memory constraints, Power BI currently limits the fetched data total to 100 MB.</span></span> <span data-ttu-id="4a322-120">Когда будет достигнуто ограничение, метод fetchMoreData() возвратит `false`.</span><span class="sxs-lookup"><span data-stu-id="4a322-120">You can see that the limit has been reached when fetchMoreData() returns `false`.</span></span>
