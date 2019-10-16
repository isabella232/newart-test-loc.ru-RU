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
# <a name="objects-and-properties-of-power-bi-visuals"></a><span data-ttu-id="6651a-103">Объекты и свойства визуальных элементов Power BI</span><span class="sxs-lookup"><span data-stu-id="6651a-103">Objects and properties of Power BI visuals</span></span>

<span data-ttu-id="6651a-104">Объекты описывают настраиваемые свойства, связанные с визуальным элементом.</span><span class="sxs-lookup"><span data-stu-id="6651a-104">Objects describe customizable properties that are associated with a visual.</span></span> <span data-ttu-id="6651a-105">Объект может иметь несколько свойств, а с каждым свойством связывается тип, который описывает это свойство.</span><span class="sxs-lookup"><span data-stu-id="6651a-105">An object can have multiple properties, and each property has an associated type that describes what the property will be.</span></span> <span data-ttu-id="6651a-106">В этой статье приводятся сведения об объектах и типах свойств.</span><span class="sxs-lookup"><span data-stu-id="6651a-106">This article provides information about objects and property types.</span></span>

<span data-ttu-id="6651a-107">`myCustomObject` — это внутреннее имя, используемое для ссылки на объект в `dataView` и `enumerateObjectInstances`.</span><span class="sxs-lookup"><span data-stu-id="6651a-107">`myCustomObject` is the internal name that's used to reference the object within `dataView` and `enumerateObjectInstances`.</span></span>

```json
"objects": {
    "myCustomObject": {
        "displayName": "My Object Name",
        "properties": { ... }
    }
}
```

## <a name="display-name"></a><span data-ttu-id="6651a-108">Отображаемое имя</span><span class="sxs-lookup"><span data-stu-id="6651a-108">Display name</span></span>

<span data-ttu-id="6651a-109">`displayName` — это имя, которое будет отображаться на панели свойств.</span><span class="sxs-lookup"><span data-stu-id="6651a-109">`displayName` is the name that will be shown in the property pane.</span></span>

## <a name="properties"></a><span data-ttu-id="6651a-110">Свойства</span><span class="sxs-lookup"><span data-stu-id="6651a-110">Properties</span></span>

<span data-ttu-id="6651a-111">`properties` — это карта свойств, определенных разработчиком.</span><span class="sxs-lookup"><span data-stu-id="6651a-111">`properties` is a map of properties that are defined by the developer.</span></span>

```json
"properties": {
    "myFirstProperty": {
        "displayName": "firstPropertyName",
        "type": ValueTypeDescriptor | StructuralTypeDescriptor
    }
}
```

> [!NOTE]
> <span data-ttu-id="6651a-112">`show` — это специальное свойство, позволяющее переключать объект.</span><span class="sxs-lookup"><span data-stu-id="6651a-112">`show` is a special property that enables a switch to toggle the object.</span></span>

<span data-ttu-id="6651a-113">Пример:</span><span class="sxs-lookup"><span data-stu-id="6651a-113">Example:</span></span>

```json
"properties": {
    "show": {
        "displayName": "My Property Switch",
        "type": {"bool": true}
    }
}
```

### <a name="property-types"></a><span data-ttu-id="6651a-114">Типы свойств</span><span class="sxs-lookup"><span data-stu-id="6651a-114">Property types</span></span>

<span data-ttu-id="6651a-115">Существует два типа свойств: `ValueTypeDescriptor` и `StructuralTypeDescriptor`.</span><span class="sxs-lookup"><span data-stu-id="6651a-115">There are two property types: `ValueTypeDescriptor` and `StructuralTypeDescriptor`.</span></span>

#### <a name="value-type-descriptor"></a><span data-ttu-id="6651a-116">Дескриптор типа значения</span><span class="sxs-lookup"><span data-stu-id="6651a-116">Value type descriptor</span></span>

<span data-ttu-id="6651a-117">Типы `ValueTypeDescriptor` являются главным образом типами-примитивами и обычно используются в качестве статического объекта.</span><span class="sxs-lookup"><span data-stu-id="6651a-117">`ValueTypeDescriptor` types are mostly primitive and are ordinarily used as a static object.</span></span>

<span data-ttu-id="6651a-118">Ниже показаны типичные элементы `ValueTypeDescriptor`:</span><span class="sxs-lookup"><span data-stu-id="6651a-118">Here are some of the common `ValueTypeDescriptor` elements:</span></span>

```typescript
export interface ValueTypeDescriptor {
    text?: boolean;
    numeric?: boolean;
    integer?: boolean;
    bool?: boolean;
}
```

#### <a name="structural-type-descriptor"></a><span data-ttu-id="6651a-119">Дескриптор структурного типа</span><span class="sxs-lookup"><span data-stu-id="6651a-119">Structural type descriptor</span></span>

<span data-ttu-id="6651a-120">Типы `StructuralTypeDescriptor` в основном используются для объектов, привязанных к данным.</span><span class="sxs-lookup"><span data-stu-id="6651a-120">`StructuralTypeDescriptor` types are mostly used for data-bound objects.</span></span>
<span data-ttu-id="6651a-121">Наиболее распространенный тип `StructuralTypeDescriptor` — *fill*.</span><span class="sxs-lookup"><span data-stu-id="6651a-121">The most common `StructuralTypeDescriptor` type is *fill*.</span></span>

```typescript
export interface StructuralTypeDescriptor {
    fill?: FillTypeDescriptor;
}
```

## <a name="gradient-property"></a><span data-ttu-id="6651a-122">Градиентное свойство</span><span class="sxs-lookup"><span data-stu-id="6651a-122">Gradient property</span></span>

<span data-ttu-id="6651a-123">Градиентное свойство — это свойство, которое невозможно задать как стандартное свойство.</span><span class="sxs-lookup"><span data-stu-id="6651a-123">The gradient property is a property that can't be set as a standard property.</span></span> <span data-ttu-id="6651a-124">Вместо этого нужно задать правило для замены свойства палитры (тип *fill*).</span><span class="sxs-lookup"><span data-stu-id="6651a-124">Instead, you need to set a rule for the substitution of the color picker property (*fill* type).</span></span>

<span data-ttu-id="6651a-125">Пример показан в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="6651a-125">An example is shown in the following code:</span></span>

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

<span data-ttu-id="6651a-126">Обратите внимание на свойства *fill* и *fillRule*.</span><span class="sxs-lookup"><span data-stu-id="6651a-126">Pay attention to the *fill* and *fillRule* properties.</span></span> <span data-ttu-id="6651a-127">Первое — это палитра, второе — правило подстановки для градиента, которое заменит *свойство fill*, `visually`, при соблюдении условий правила.</span><span class="sxs-lookup"><span data-stu-id="6651a-127">The first is the color picker, and the second is the substitution rule for the gradient that will replace the *fill property*, `visually`, when the rule conditions are met.</span></span>

<span data-ttu-id="6651a-128">Эта связь между свойством *fill* и правилом подстановки определяется в разделе `"rule"`>`"output"` свойства *fillRule*.</span><span class="sxs-lookup"><span data-stu-id="6651a-128">This link between the *fill* property and the substitution rule is set in the `"rule"`>`"output"` section of the *fillRule* property.</span></span>

<span data-ttu-id="6651a-129">Свойство `"Rule"`>`"InputRole"` задает, какая роль данных активирует правило (условие).</span><span class="sxs-lookup"><span data-stu-id="6651a-129">`"Rule"`>`"InputRole"` property sets which data role triggers the rule (condition).</span></span> <span data-ttu-id="6651a-130">В этом примере если роль данных `"Gradient"` содержит данные, правило будет применено к свойству `"fill"`.</span><span class="sxs-lookup"><span data-stu-id="6651a-130">In this example, if data role `"Gradient"` contains data, the rule is applied for the `"fill"` property.</span></span>

<span data-ttu-id="6651a-131">В следующем коде показан пример роли данных, которая активирует правило заливки (`the last item`):</span><span class="sxs-lookup"><span data-stu-id="6651a-131">An example of the data role that triggers the fill rule (`the last item`) is shown in the following code:</span></span>

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

## <a name="the-enumerateobjectinstances-method"></a><span data-ttu-id="6651a-132">Метод enumerateObjectInstances</span><span class="sxs-lookup"><span data-stu-id="6651a-132">The enumerateObjectInstances method</span></span>

<span data-ttu-id="6651a-133">Чтобы эффективно использовать объекты, вам потребуется функция в пользовательском визуальном элементе с именем `enumerateObjectInstances`.</span><span class="sxs-lookup"><span data-stu-id="6651a-133">To use objects effectively, you need a function in your custom visual called `enumerateObjectInstances`.</span></span> <span data-ttu-id="6651a-134">Эта функция заполняет панель свойств объектами, а также определяет, куда должны быть привязаны объекты внутри dataView.</span><span class="sxs-lookup"><span data-stu-id="6651a-134">This function populates the property pane with objects and also determines where your objects should be bound within the dataView.</span></span>  

<span data-ttu-id="6651a-135">Вот как выглядит обычная настройка:</span><span class="sxs-lookup"><span data-stu-id="6651a-135">Here is what a typical setup looks like:</span></span>

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

### <a name="properties"></a><span data-ttu-id="6651a-136">Свойства</span><span class="sxs-lookup"><span data-stu-id="6651a-136">Properties</span></span>

<span data-ttu-id="6651a-137">Свойства в `enumerateObjectInstances` отражают свойства, определенные в ваших возможностях.</span><span class="sxs-lookup"><span data-stu-id="6651a-137">The properties in `enumerateObjectInstances` reflect the properties that you defined in your capabilities.</span></span> <span data-ttu-id="6651a-138">См. пример в конце этой статьи.</span><span class="sxs-lookup"><span data-stu-id="6651a-138">For an example, go to the end of this article.</span></span>

### <a name="objects-selector"></a><span data-ttu-id="6651a-139">Селектор объектов</span><span class="sxs-lookup"><span data-stu-id="6651a-139">Objects selector</span></span>

<span data-ttu-id="6651a-140">Селектор в `enumerateObjectInstances` определяет, куда привязывается каждый объект в dataView.</span><span class="sxs-lookup"><span data-stu-id="6651a-140">The selector in `enumerateObjectInstances` determines where each object is bound in the dataView.</span></span> <span data-ttu-id="6651a-141">Существует четыре отдельных параметра.</span><span class="sxs-lookup"><span data-stu-id="6651a-141">There are four distinct options.</span></span>

#### <a name="static"></a><span data-ttu-id="6651a-142">static</span><span class="sxs-lookup"><span data-stu-id="6651a-142">static</span></span>

<span data-ttu-id="6651a-143">Этот объект привязывается к метаданным `dataviews[index].metadata.objects`, как показано здесь.</span><span class="sxs-lookup"><span data-stu-id="6651a-143">This object is bound to metadata `dataviews[index].metadata.objects`, as shown here.</span></span>

```typescript
selector: null
```

#### <a name="columns"></a><span data-ttu-id="6651a-144">columns</span><span class="sxs-lookup"><span data-stu-id="6651a-144">columns</span></span>

<span data-ttu-id="6651a-145">Этот объект привязывается к столбцам с соответствующим `QueryName`.</span><span class="sxs-lookup"><span data-stu-id="6651a-145">This object is bound to columns with the matching `QueryName`.</span></span>

```typescript
selector: {
    metadata: 'QueryName'
}
```

#### <a name="selector"></a><span data-ttu-id="6651a-146">selector</span><span class="sxs-lookup"><span data-stu-id="6651a-146">selector</span></span>

<span data-ttu-id="6651a-147">Этот объект привязывается к элементу, для которого вы создали `selectionID`.</span><span class="sxs-lookup"><span data-stu-id="6651a-147">This object is bound to the element that you created a `selectionID` for.</span></span> <span data-ttu-id="6651a-148">В этом примере предполагается, что мы создали `selectionID` для некоторых точек данные и выполняем их перебор в цикле.</span><span class="sxs-lookup"><span data-stu-id="6651a-148">In this example, let's assume that we've created `selectionID`s for some data points, and that we're looping through them.</span></span>

```typescript
for (let dataPoint in dataPoints) {
    ...
    selector: dataPoint.selectionID.getSelector()
}
```

#### <a name="scope-identity"></a><span data-ttu-id="6651a-149">scope identity</span><span class="sxs-lookup"><span data-stu-id="6651a-149">Scope identity</span></span>

<span data-ttu-id="6651a-150">Этот объект привязывается к конкретным значениям на пересечении групп.</span><span class="sxs-lookup"><span data-stu-id="6651a-150">This object is bound to particular values at the intersection of groups.</span></span> <span data-ttu-id="6651a-151">Например, если у вас есть категории `["Jan", "Feb", "March", ...]` и ряды `["Small", "Medium", "Large"]`, вам может потребоваться объект на пересечении значений, соответствующих `Feb` и `Large`.</span><span class="sxs-lookup"><span data-stu-id="6651a-151">For example, if you have categories `["Jan", "Feb", "March", ...]` and series `["Small", "Medium", "Large"]`, you might want to have an object at the intersection of values that match `Feb` and `Large`.</span></span> <span data-ttu-id="6651a-152">Для этого вы можете получить `DataViewScopeIdentity` обоих столбцов, отправить их в переменную `identities` и использовать этот синтаксис с селектором.</span><span class="sxs-lookup"><span data-stu-id="6651a-152">To accomplish this, you could get the `DataViewScopeIdentity` of both columns, push them to variable `identities`, and use this syntax with the selector.</span></span>

```typescript
selector: {
    data: <DataViewScopeIdentity[]>identities
}
```

##### <a name="example"></a><span data-ttu-id="6651a-153">Пример</span><span class="sxs-lookup"><span data-stu-id="6651a-153">Example</span></span>

<span data-ttu-id="6651a-154">В этом примере показано, как будет выглядеть один objectEnumeration для объекта customColor с одним свойством *fill*.</span><span class="sxs-lookup"><span data-stu-id="6651a-154">The following example shows what one objectEnumeration would look like for a customColor object with one property, *fill*.</span></span> <span data-ttu-id="6651a-155">Мы хотим, чтобы этот объект был статически привязан к `dataViews[index].metadata.objects`, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="6651a-155">We want this object bound statically to `dataViews[index].metadata.objects`, as shown:</span></span>

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
