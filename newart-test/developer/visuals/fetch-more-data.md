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
# <a name="fetch-more-data-from-power-bi"></a>Получение дополнительных данных из Power BI

В этой статье описывается, как загрузить дополнительные данные, чтобы обойти ограничение в 30 КБ, установленное для точки данных. При таком подходе данные предоставляются в виде блоков. Чтобы повысить производительность, можно настроить размер блока в соответствии с конкретными требованиями.  

## <a name="enable-a-segmented-fetch-of-large-datasets"></a>Включение сегментированного получения больших наборов данных

Для сегментного режима `dataview` определите размер окна для dataReductionAlgorithm в файле *capabilities.json* визуального элемента для требуемого dataViewMapping. `count` определяет размер окна, ограничивающий количество новых строк данных, добавляемых к `dataview` при каждом обновлении.

Добавьте следующий код в файл *capabilities.json*:

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

Новые сегменты добавляются к существующему `dataview` и предоставляются визуальному элементу в виде вызова `update`.

## <a name="usage-in-the-power-bi-visual"></a>Использование в визуальном элементе Power BI

Чтобы определить, существуют ли данные, проверьте наличие `dataView.metadata.segment`:

```typescript
    public update(options: VisualUpdateOptions) {
        const dataView = options.dataViews[0];
        console.log(dataView.metadata.segment);
        // output: __proto__: Object
    }
```

С помощью `options.operationKind` вы также можете проверить, является ли это обновление первичным или очередным. В следующем коде `VisualDataChangeOperationKind.Create` ссылается на первый сегмент, а `VisualDataChangeOperationKind.Append` — на последующие сегменты.

Пример реализации см. в следующем фрагменте кода:

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

Можно также вызвать метод `fetchMoreData` из обработчика событий пользовательского интерфейса, как показано ниже:

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

В ответ на вызов метода `this.host.fetchMoreData` Power BI вызывает метод `update` визуального элемента с новым сегментом данных.

> [!NOTE]
> Power BI ограничит общий объем получаемых данных значением 100 МБ, чтобы предотвратить нехватку памяти на клиенте. Когда будет достигнуто ограничение, метод fetchMoreData() возвратит `false`.
