---
title: Объекты и свойства визуальных элементов Power BI
description: В этой статье описываются настраиваемые свойства визуальных элементов Power BI.
author: KesemSharabi
ms.author: kesharab
manager: rkarlin
ms.reviewer: sranins
ms.service: powerbi
ms.subservice: powerbi-custom-visuals
ms.topic: conceptual
ms.date: 06/18/2019
ms.openlocfilehash: b2043c6727e4cf8c5c46c4e277b01a9ea04a969b
ms.sourcegitcommit: a1c994bc8fa5f072fce13a6f35079e87d45f00f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72424122"
---
# <a name="objects-and-properties-of-power-bi-visuals"></a>Объекты и свойства визуальных элементов Power BI

Объекты описывают настраиваемые свойства, связанные с визуальным элементом. Объект может иметь несколько свойств, а с каждым свойством связывается тип, который описывает это свойство. В этой статье приводятся сведения об объектах и типах свойств.

`myCustomObject` — это внутреннее имя, используемое для ссылки на объект в `dataView` и `enumerateObjectInstances`.

```json
"objects": {
    "myCustomObject": {
        "displayName": "My Object Name",
        "properties": { ... }
    }
}
```

## <a name="display-name"></a>Отображаемое имя

`displayName` — это имя, которое будет отображаться на панели свойств.

## <a name="properties"></a>Свойства

`properties` — это карта свойств, определенных разработчиком.

```json
"properties": {
    "myFirstProperty": {
        "displayName": "firstPropertyName",
        "type": ValueTypeDescriptor | StructuralTypeDescriptor
    }
}
```

> [!NOTE]
> `show` — это специальное свойство, позволяющее переключать объект.

Пример:

```json
"properties": {
    "show": {
        "displayName": "My Property Switch",
        "type": {"bool": true}
    }
}
```

### <a name="property-types"></a>Типы свойств

Существует два типа свойств: `ValueTypeDescriptor` и `StructuralTypeDescriptor`.

#### <a name="value-type-descriptor"></a>Дескриптор типа значения

Типы `ValueTypeDescriptor` являются главным образом типами-примитивами и обычно используются в качестве статического объекта.

Ниже показаны типичные элементы `ValueTypeDescriptor`:

```typescript
export interface ValueTypeDescriptor {
    text?: boolean;
    numeric?: boolean;
    integer?: boolean;
    bool?: boolean;
}
```

#### <a name="structural-type-descriptor"></a>Дескриптор структурного типа

Типы `StructuralTypeDescriptor` в основном используются для объектов, привязанных к данным.
Наиболее распространенный тип `StructuralTypeDescriptor` — *fill*.

```typescript
export interface StructuralTypeDescriptor {
    fill?: FillTypeDescriptor;
}
```

## <a name="gradient-property"></a>Градиентное свойство

Градиентное свойство — это свойство, которое невозможно задать как стандартное свойство. Вместо этого нужно задать правило для замены свойства палитры (тип *fill*).

Пример показан в следующем коде:

```json
"properties": {
    "showAllDataPoints": {
        "displayName": "Show all",
        "displayNameKey": "Visual_DataPoint_Show_All",
        "type": {
            "bool": true
        }
    },
    "fill": {
        "displayName": "Fill",
        "displayNameKey": "Visual_Fill",
        "type": {
            "fill": {
                "solid": {
                    "color": true
                }
            }
        }
    },
    "fillRule": {
        "displayName": "Color saturation",
        "displayNameKey": "Visual_ColorSaturation",
        "type": {
            "fillRule": {}
        },
        "rule": {
            "inputRole": "Gradient",
            "output": {
                "property": "fill",
                    "selector": [
                        "Category"
                    ]
            }
        }
    }
}
```

Обратите внимание на свойства *fill* и *fillRule*. Первое — это палитра, второе — правило подстановки для градиента, которое заменит *свойство fill*, `visually`, при соблюдении условий правила.

Эта связь между свойством *fill* и правилом подстановки определяется в разделе `"rule"`>`"output"` свойства *fillRule*.

Свойство `"Rule"`>`"InputRole"` задает, какая роль данных активирует правило (условие). В этом примере если роль данных `"Gradient"` содержит данные, правило будет применено к свойству `"fill"`.

В следующем коде показан пример роли данных, которая активирует правило заливки (`the last item`):

```json
{
    "dataRoles": [
            {
                "name": "Category",
                "kind": "Grouping",
                "displayName": "Details",
                "displayNameKey": "Role_DisplayName_Details"
            },
            {
                "name": "Series",
                "kind": "Grouping",
                "displayName": "Legend",
                "displayNameKey": "Role_DisplayName_Legend"
            },
            {
                "name": "Gradient",
                "kind": "Measure",
                "displayName": "Color saturation",
                "displayNameKey": "Role_DisplayName_Gradient"
            }
    ]
}
```

## <a name="the-enumerateobjectinstances-method"></a>Метод enumerateObjectInstances

Чтобы эффективно использовать объекты, вам потребуется функция в пользовательском визуальном элементе с именем `enumerateObjectInstances`. Эта функция заполняет панель свойств объектами, а также определяет, куда должны быть привязаны объекты внутри dataView.  

Вот как выглядит обычная настройка:

```typescript
public enumerateObjectInstances(options: EnumerateVisualObjectInstancesOptions): VisualObjectInstanceEnumeration {
    let objectName: string = options.objectName;
    let objectEnumeration: VisualObjectInstance[] = [];

    switch( objectName ) {
        case 'myCustomObject':
            objectEnumeration.push({
                objectName: objectName,
                properties: { ... },
                selector: { ... }
            });
            break;
    };

    return objectEnumeration;
}
```

### <a name="properties"></a>Свойства

Свойства в `enumerateObjectInstances` отражают свойства, определенные в ваших возможностях. См. пример в конце этой статьи.

### <a name="objects-selector"></a>Селектор объектов

Селектор в `enumerateObjectInstances` определяет, куда привязывается каждый объект в dataView. Существует четыре отдельных параметра.

#### <a name="static"></a>static

Этот объект привязывается к метаданным `dataviews[index].metadata.objects`, как показано здесь.

```typescript
selector: null
```

#### <a name="columns"></a>columns

Этот объект привязывается к столбцам с соответствующим `QueryName`.

```typescript
selector: {
    metadata: 'QueryName'
}
```

#### <a name="selector"></a>selector

Этот объект привязывается к элементу, для которого вы создали `selectionID`. В этом примере предполагается, что мы создали `selectionID` для некоторых точек данные и выполняем их перебор в цикле.

```typescript
for (let dataPoint in dataPoints) {
    ...
    selector: dataPoint.selectionID.getSelector()
}
```

#### <a name="scope-identity"></a>scope identity

Этот объект привязывается к конкретным значениям на пересечении групп. Например, если у вас есть категории `["Jan", "Feb", "March", ...]` и ряды `["Small", "Medium", "Large"]`, вам может потребоваться объект на пересечении значений, соответствующих `Feb` и `Large`. Для этого вы можете получить `DataViewScopeIdentity` обоих столбцов, отправить их в переменную `identities` и использовать этот синтаксис с селектором.

```typescript
selector: {
    data: <DataViewScopeIdentity[]>identities
}
```

##### <a name="example"></a>Пример

В этом примере показано, как будет выглядеть один objectEnumeration для объекта customColor с одним свойством *fill*. Мы хотим, чтобы этот объект был статически привязан к `dataViews[index].metadata.objects`, как показано ниже:

```typescript
objectEnumeration.push({
    objectName: "customColor",
    displayName: "Custom Color",
    properties: {
        fill: {
            solid: {
                color: dataPoint.color
            }
        }
    },
    selector: null
});
```
