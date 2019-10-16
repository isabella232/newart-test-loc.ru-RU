---
title: Добавление поддержки закладок для визуальных элементов Power BI
description: Визуальные элементы Power BI могут обрабатывать переключение между закладками
author: KesemSharabi
ms.author: kesharab
manager: rkarlin
ms.reviewer: sranins
ms.service: powerbi
ms.subservice: powerbi-custom-visuals
ms.topic: conceptual
ms.date: 06/18/2019
ms.openlocfilehash: c19b67a59d0ecb4cbfbcf5ad8dd18886f440e164
ms.sourcegitcommit: a1c994bc8fa5f072fce13a6f35079e87d45f00f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72424227"
---
# <a name="add-bookmark-support-for-power-bi-visuals"></a>Добавление поддержки закладок для визуальных элементов Power BI

С помощью закладок в отчете Power BI можно зафиксировать настроенное представление страницы отчета, состояние выбора и состояние фильтрации визуального элемента. Но для поддержки закладок и правильного реагирования на изменения визуальные элементы Power BI требуют некоторой доработки.

Дополнительные сведения о закладках см. в статье [Использование закладок для обмена аналитическими сведениями и создания историй в Power BI](https://docs.microsoft.com/power-bi/desktop-bookmarks).

## <a name="report-bookmarks-support-in-your-visual"></a>Поддержка закладок отчетов в визуальном элементе

Если визуальный элемент взаимодействует с другими визуальными элементами, выбирает точки данных или фильтрует другие визуальные элементы, нужно восстановить состояние из свойств.

## <a name="add-report-bookmarks-support"></a>Добавление поддержки закладок в отчете

1. Установите служебную программу [powerbi-visuals-utils-interactivityutils](https://github.com/Microsoft/PowerBI-visuals-utils-interactivityutils/) версии 3.0.0 или более поздней либо обновите ее до нужной версии. Она содержит дополнительные классы для работы с фильтрацией или выбором состояния. Эта программа необходима для визуальных элементов с фильтром и всех визуальных элементов, использующих `InteractivityService`.

2. Обновите API визуальных элементов до версии 1.11.0, чтобы использовать `registerOnSelectCallback` в экземпляре `SelectionManager`. Это необходимо для визуальных элементов без фильтра, использующих обычный `SelectionManager`, а не `InteractivityService`.

### <a name="how-power-bi-visuals-interact-with-power-bi-in-report-bookmarks"></a>Взаимодействие визуальных элементов Power BI с Power BI в сценарии закладок отчета

Рассмотрим следующий сценарий: вам требуется создать несколько закладок с различными состояниями выбора на странице отчета.

Сначала выберите точку данных в визуальном элементе. Визуальный элемент взаимодействует с Power BI и другими визуальными элементами, передавая выбранные фрагменты на узел. Далее выберите **Добавить** в области **Закладка**. Служба Power BI сохранит текущие выбранные фрагменты для новой закладки.

Это происходит многократно по мере того, как вы изменяете выбор и добавляете новые закладки. После создания закладок вы можете переключаться между ними.

Когда вы выбираете закладку, Power BI восстанавливает сохраненный фильтр или сохраненное состояние выбора и передает их визуальным элементам. Другие визуальные элементы выделяются или фильтруются в соответствии с состоянием, хранящимся в закладке. Реализацию этих действий обеспечивает узел Power BI. Ваш визуальный элемент отвечает за правильное отражение нового состояния выбора (например, за изменение цвета отрисованных точек данных).

Новое состояние выбора передается визуальному элементу через метод `update`. Аргумент `options` содержит специальное свойство: `options.jsonFilters`. Это свойство JSONFilter может содержать `Advanced Filter` и `Tuple Filter`.

Визуальный элемент должен восстановить значения фильтра, чтобы отобразить соответствующее состояние визуального элемента для выбранной закладки. Либо используйте специальный метод `registerOnSelectCallback` интерфейса ISelectionManager, зарегистрированный в вызове функции обратного вызова, если визуальный элемент использует только выбранные фрагменты.

### <a name="visuals-with-selection"></a>Визуальные элементы с выбранными фрагментами

Если ваш визуальный элемент взаимодействует с другими визуальными элементами посредством [выбора](https://github.com/Microsoft/PowerBI-visuals/blob/master/Tutorial/Selection.md), вы можете добавить закладки любым из двух способов:

* Если визуальный элемент еще не использовал [InteractivityService](https://github.com/Microsoft/powerbi-visuals-utils-interactivityutils/blob/master/docs/api/interactivityService.md), вы можете использовать метод `FilterManager.restoreSelectionIds`.

* Если визуальный элемент уже использует [InteractivityService](https://github.com/Microsoft/powerbi-visuals-utils-interactivityutils/blob/master/docs/api/interactivityService.md) для управления выбранными фрагментами, следует использовать метод `applySelectionFromFilter` в экземпляре `InteractivityService`.

#### <a name="use-iselectionmanagerregisteronselectcallback"></a>Использование ISelectionManager.registerOnSelectCallback

Когда вы выбираете закладку, Power BI вызывает метод `callback` визуального элемента с соответствующими выбранными фрагментами. 

```typescript
this.selectionManager.registerOnSelectCallback(
    (ids: ISelectionId[]) => {
        //called when a selection was set by Power BI
    });
);
```

Предположим, что в вашем визуальном элементе присутствует точка данных, созданная в методе [visualTransform](https://github.com/Microsoft/PowerBI-visuals-sampleBarChart/blob/master/src/barChart.ts#L74).

При этом `datapoints` выглядит следующим образом:

```typescript
visualDataPoints.push({
    category: categorical.categories[0].values[i],
    color: getCategoricalObjectValue<Fill>(categorical.categories[0], i, 'colorSelector', 'fill', defaultColor).solid.color,
    selectionId: host.createSelectionIdBuilder()
        .withCategory(categorical.categories[0], i)
        .createSelectionId(),
    selected: false
});
```

У вас есть `visualDataPoints` в качестве точек данных и массив `ids`, переданный в функцию `callback`.

На этом этапе визуальный элемент должен сравнить массив `ISelectionId[]` с выбранными фрагментами в массиве `visualDataPoints` и отметить соответствующие точки данных как выбранные.

```typescript
this.selectionManager.registerOnSelectCallback(
    (ids: ISelectionId[]) => {
        visualDataPoints.forEach(dataPoint => {
            ids.forEach(bookmarkSelection => {
                if (bookmarkSelection.equals(dataPoint.selectionId)) {
                    dataPoint.selected = true;
                }
            });
        });
    });
);
```

После обновления точек данных они будут отражать текущее состояние выбора, сохраненное в объекте `filter`. Затем при отрисовке точек данных состояние выбора пользовательского визуального элемента будет соответствовать состоянию закладки.

### <a name="use-interactivityservice-for-control-selections-in-the-visual"></a>Использование InteractivityService для управления выбранными фрагментами в визуальном элементе

Если визуальный элемент использует `InteractivityService`, дополнительные действия для обеспечения в нем поддержки закладок не требуются.

Когда вы выбираете закладку, программа обрабатывает состояние выбора визуального элемента.

### <a name="visuals-with-a-filter"></a>Визуальные элементы с фильтром

Предположим, что визуальный элемент создает фильтр данных по диапазону дат. `startDate` и `endDate` выступают в качестве начальной и конечной дат диапазона.

Визуальный элемент создает расширенный фильтр и вызывает метод узла `applyJsonFilter` для фильтрации данных по соответствующим условиям.

В качестве целевого объекта выступает таблица, которая используется для фильтрации.

```typescript
import { AdvancedFilter } from "powerbi-models";

const filter: IAdvancedFilter = new AdvancedFilter(
    target,
    "And",
    {
        operator: "GreaterThanOrEqual",
        value: startDate
            ? startDate.toJSON()
            : null
    },
    {
        operator: "LessThanOrEqual",
        value: endDate
            ? endDate.toJSON()
            : null
    });

this.host.applyJsonFilter(
    filter,
    "general",
    "filter",
    (startDate && endDate)
        ? FilterAction.merge
        : FilterAction.remove
);
```

Каждый раз, когда вы щелкаете закладку, пользовательский визуальный элемент получает вызов `update`.

Пользовательский визуальный элемент должен проверить фильтр в объекте:

```typescript
const filter: IAdvancedFilter = FilterManager.restoreFilter(
    && options.jsonFilters
    && options.jsonFilters[0] as any
) as IAdvancedFilter;
```

Если объект `filter` не имеет значение NULL, визуальный элемент должен восстановить условия фильтра из объекта:

```typescript
const jsonFilters: AdvancedFilter = this.options.jsonFilters as AdvancedFilter[];

if (jsonFilters
    && jsonFilters[0]
    && jsonFilters[0].conditions
    && jsonFilters[0].conditions[0]
    && jsonFilters[0].conditions[1]
) {
    const startDate: Date = new Date(`${jsonFilters[0].conditions[0].value}`);
    const endDate: Date = new Date(`${jsonFilters[0].conditions[1].value}`);

    // apply restored conditions
} else {
    // apply default settings
}
```

После этого визуальный элемент должен изменить свое внутреннее состояние, чтобы отразить текущие условия. Внутреннее состояние включает в себя точки данных и объекты визуализации (линии, прямоугольники и т. д.).

> [!IMPORTANT]
> В сценарии с закладками отчета визуальный элемент не должен вызывать `applyJsonFilter` для фильтрации других визуальных элементов. Они уже будут фильтроваться Power BI.

Визуальный элемент среза временной шкалы изменяет селектор диапазонов на соответствующие диапазоны данных.

Дополнительные сведения см. в [репозитории среза временной шкалы](https://github.com/Microsoft/powerbi-visuals-timeline/commit/606f1152f59f82b5b5a367ff3b117372d129e597?diff=unified#diff-b6ef9a9ac3a3225f8bd0de84bee0a0df).

### <a name="filter-the-state-to-save-visual-properties-in-bookmarks"></a>Фильтрация состояния для сохранения свойств визуальных элементов в закладках

Свойство `filterState` включает свойство в фильтрацию. Визуальный элемент может хранить различные значения в закладках.

Чтобы сохранить значение свойства как состояние фильтра, пометьте свойство объекта как `"filterState": true` в файле *capabilities.json*.

Например, в срезе временной шкалы хранятся значения свойства `Granularity` в фильтре. Это позволяет изменить текущую степень детализации при изменении закладок.

Дополнительные сведения см. в [репозитории среза временной шкалы](https://github.com/microsoft/powerbi-visuals-timeline/commit/8b7d82dd23cd2bd71817f1bc5d1e1732347a185e#diff-290828b604cfa62f1cb310f2e90c52fdR334).
