---
title: Возможности и свойства визуальных элементов Power BI
description: В этой статье описываются возможности и свойства визуальных элементов Power BI.
author: KesemSharabi
ms.author: kesharab
manager: rkarlin
ms.reviewer: sranins
ms.service: powerbi
ms.subservice: powerbi-custom-visuals
ms.topic: conceptual
ms.date: 06/18/2019
ms.openlocfilehash: 8b14568eb3a9ebc75b8d94935aedc2ae07122ef4
ms.sourcegitcommit: a1c994bc8fa5f072fce13a6f35079e87d45f00f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72424207"
---
# <a name="capabilities-and-properties-of-power-bi-visuals"></a>Возможности и свойства визуальных элементов Power BI 

Возможности предоставляют узлу сведения о визуальном элементе. Все свойства в модели возможностей являются `optional`.

Корневыми объектами возможностей визуального элемента являются `dataRoles`, `dataViewMappings` и т. д.

```json
{
    "dataRoles": [ ... ],
    "dataViewMappings": [ ... ],
    "objects":  { ... },
    "supportsHighlight": true|false,
    "advancedEditModeSupport": 0|1|2,
    "sorting": { ... }
}

```

## <a name="define-the-data-fields-that-your-visual-expects-dataroles"></a>Определение полей данных, ожидаемых вашими визуальными элементами, — dataRoles

Чтобы определить поля, которые можно привязать к данным, используйте `dataRoles`. `dataRoles` принимает массив объектов `DataViewRole`, который определяет все обязательные свойства.

### <a name="properties"></a>Свойства

* **name** — внутреннее имя этого поля данных (должно быть уникальным).
* **kind** — тип поля:
    * `Grouping` — дискретные значения, используемые для группировки полей мер.
    * `Measure` — числовые значения данных.
    * `GroupingOrMeasure` — значения, которые могут использоваться как группирование или как мера.
* **displayName** — имя, отображаемое пользователю на панели **свойств**.
* **description** — краткое описание поля (необязательно).
* **requiredTypes** — требуемый тип данных для этой роли данных. Несоответствующим значениям присваивается значение null (необязательно).
* **preferredTypes** — предпочтительный тип данных для этой роли данных (необязательно).

### <a name="valid-data-types-in-requiredtypes-and-preferredtypes"></a>Допустимые типы данных для requiredTypes и preferredTypes

* **bool** — логическое значение.
* **integer** — целочисленное значение (целое число).
* **numeric** — числовое значение.
* **text** — текстовое значение.
* **geography** — географические данные.

### <a name="example"></a>Пример

```json
"dataRoles": [
    {
        "displayName": "My Category Data",
        "name": "myCategory",
        "kind": "Grouping",
        "requiredTypes": [
            {
                "text": true
            },
            {
                "numeric": true
            },
            {
                "integer": true
            }
        ],
        "preferredTypes": [
            {
                "text": true
            }
        ]
    },
    {
        "displayName": "My Measure Data",
        "name": "myMeasure",
        "kind": "Measure",
        "requiredTypes": [
            {
                "integer": true
            },
            {
                "numeric": true
            }
        ],
        "preferredTypes": [
            {
                "integer": true
            }
        ]
    },
    {
        "displayNameKey": "Visual_Location",
        "name": "Locations",
        "kind": "Measure",
        "displayName": "Locations",
        "requiredTypes": [
            {
                "geography": {
                    "address": true
                }
            },
            {
                "geography": {
                    "city": true
                }
            },
            {
                "geography": {
                    "continent": true
                }
            },
            {
                "geography": {
                    "country": true
                }
            },
            {
                "geography": {
                    "county": true
                }
            },
            {
                "geography": {
                    "place": true
                }
            },
            {
                "geography": {
                    "postalCode": true
                }
            },
            {
                "geography": {
                    "region": true
                }
            },
            {
                "geography": {
                    "stateOrProvince": true
                }
            }
        ]
    }
]
```

Приведенные выше роли данных используются для создания полей, которые показаны на следующем рисунке:

![Поля ролей данных](./media/data-role-display.png)

## <a name="define-how-you-want-the-data-mapped-dataviewmappings"></a>Определение сопоставления данных — dataViewMappings

Свойство DataViewMapping описывает связь между ролями данных и позволяет указать для них условные требования.

Большинство визуальных элементов предоставляют одиночное сопоставление, но вы можете указать несколько DataViewMappings. Каждое допустимое сопоставление создает представление данных. 

```json
"dataViewMappings": [
    {
        "conditions": [ ... ],
        "categorical": { ... },
        "table": { ... },
        "single": { ... },
        "matrix": { ... }
    }
]
```

Дополнительные сведения см. в статье [Общие сведения о сопоставлениях представлений данных в визуальных элементах Power BI](dataview-mappings.md).

## <a name="define-property-pane-options-objects"></a>Определение параметров панели свойств — objects

Объекты описывают настраиваемые свойства, связанные с визуальным элементом. Каждый объект может иметь несколько свойств, и с каждым свойством сопоставлен тип. Типы указывают, что будет представлять собой свойство. 

```json
"objects": {
    "myCustomObject": {
        "displayName": "My Object Name",
        "properties": { ... }
    }
}
```

Дополнительные сведения см. в статье [Объекты и свойства в визуальных элементах Power BI](objects-properties.md).

## <a name="handle-partial-highlighting-supportshighlight"></a>Использование частичного выделения — supportsHighlight

По умолчанию этому параметру присваивается значение `false`, которое задает автоматическую фильтрацию значений при выборе любого элемента на странице. В свою очередь, в результате автоматической фильтрации в визуальном элементе отображается только выбранное значение. Если вы хотите отобразить полные данные и выделить только выбранные элементы, нужно задать значение `supportsHighlight` для `true` в файле *capabilities.json*.

Дополнительные сведения см. в статье [Выделение точек данных в визуальных элементах Power BI](highlight.md).

## <a name="handle-advanced-edit-mode-advancededitmodesupport"></a>Поддержка режима расширенного редактирования — advancedEditModeSupport

Визуальный элемент может объявить поддержку режима расширенного редактирования. По умолчанию визуальный элемент не поддерживает режим расширенного редактирования, если иное не указано в файле *capabilities.json*.

Дополнительные сведения см. в статье [Режим расширенного редактирования для визуальных элементов Power BI](advanced-edit-mode.md).

## <a name="data-sorting-options-for-visual-sorting"></a>Параметры сортировки данных для визуального элемента — sorting

Визуальный элемент может определить свое поведение сортировки с помощью возможностей. По умолчанию визуальный элемент не поддерживает изменение порядка сортировки, если иное не указано в файле *capabilities.json*.

Дополнительные сведения см. в статье [Параметры сортировки для визуальных элементов Power BI](sort-options.md).
