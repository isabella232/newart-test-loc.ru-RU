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
# <a name="add-bookmark-support-for-power-bi-visuals"></a><span data-ttu-id="fde96-103">Добавление поддержки закладок для визуальных элементов Power BI</span><span class="sxs-lookup"><span data-stu-id="fde96-103">Add bookmark support for Power BI visuals</span></span>

<span data-ttu-id="fde96-104">С помощью закладок в отчете Power BI можно зафиксировать настроенное представление страницы отчета, состояние выбора и состояние фильтрации визуального элемента.</span><span class="sxs-lookup"><span data-stu-id="fde96-104">With Power BI report bookmarks, you can capture a configured view of a report page, the selection state, and the filtering state of the visual.</span></span> <span data-ttu-id="fde96-105">Но для поддержки закладок и правильного реагирования на изменения визуальные элементы Power BI требуют некоторой доработки.</span><span class="sxs-lookup"><span data-stu-id="fde96-105">But it requires additional action from the Power BI visuals side to support the bookmark and react correctly to changes.</span></span>

<span data-ttu-id="fde96-106">Дополнительные сведения о закладках см. в статье [Использование закладок для обмена аналитическими сведениями и создания историй в Power BI](https://docs.microsoft.com/power-bi/desktop-bookmarks).</span><span class="sxs-lookup"><span data-stu-id="fde96-106">For more information about bookmarks, see [Use bookmarks to share insights and build stories in Power BI](https://docs.microsoft.com/power-bi/desktop-bookmarks).</span></span>

## <a name="report-bookmarks-support-in-your-visual"></a><span data-ttu-id="fde96-107">Поддержка закладок отчетов в визуальном элементе</span><span class="sxs-lookup"><span data-stu-id="fde96-107">Report bookmarks support in your visual</span></span>

<span data-ttu-id="fde96-108">Если визуальный элемент взаимодействует с другими визуальными элементами, выбирает точки данных или фильтрует другие визуальные элементы, нужно восстановить состояние из свойств.</span><span class="sxs-lookup"><span data-stu-id="fde96-108">If your visual interacts with other visuals, selects data points, or filters other visuals, you need to restore the state from properties.</span></span>

## <a name="add-report-bookmarks-support"></a><span data-ttu-id="fde96-109">Добавление поддержки закладок в отчете</span><span class="sxs-lookup"><span data-stu-id="fde96-109">Add report bookmarks support</span></span>

1. <span data-ttu-id="fde96-110">Установите служебную программу [powerbi-visuals-utils-interactivityutils](https://github.com/Microsoft/PowerBI-visuals-utils-interactivityutils/) версии 3.0.0 или более поздней либо обновите ее до нужной версии.</span><span class="sxs-lookup"><span data-stu-id="fde96-110">Install (or update) the required utility, [powerbi-visuals-utils-interactivityutils](https://github.com/Microsoft/PowerBI-visuals-utils-interactivityutils/) version 3.0.0 or later.</span></span> <span data-ttu-id="fde96-111">Она содержит дополнительные классы для работы с фильтрацией или выбором состояния.</span><span class="sxs-lookup"><span data-stu-id="fde96-111">It contains additional classes to manipulate with the state selection or filter.</span></span> <span data-ttu-id="fde96-112">Эта программа необходима для визуальных элементов с фильтром и всех визуальных элементов, использующих `InteractivityService`.</span><span class="sxs-lookup"><span data-stu-id="fde96-112">It's required for filter visuals and any visual that uses `InteractivityService`.</span></span>

2. <span data-ttu-id="fde96-113">Обновите API визуальных элементов до версии 1.11.0, чтобы использовать `registerOnSelectCallback` в экземпляре `SelectionManager`.</span><span class="sxs-lookup"><span data-stu-id="fde96-113">Update the visual API to version 1.11.0 to use `registerOnSelectCallback` in an instance of `SelectionManager`.</span></span> <span data-ttu-id="fde96-114">Это необходимо для визуальных элементов без фильтра, использующих обычный `SelectionManager`, а не `InteractivityService`.</span><span class="sxs-lookup"><span data-stu-id="fde96-114">It's required for non-filter visuals that use the plain `SelectionManager` rather than `InteractivityService`.</span></span>

### <a name="how-power-bi-visuals-interact-with-power-bi-in-report-bookmarks"></a><span data-ttu-id="fde96-115">Взаимодействие визуальных элементов Power BI с Power BI в сценарии закладок отчета</span><span class="sxs-lookup"><span data-stu-id="fde96-115">How Power BI visuals interact with Power BI in report bookmarks</span></span>

<span data-ttu-id="fde96-116">Рассмотрим следующий сценарий: вам требуется создать несколько закладок с различными состояниями выбора на странице отчета.</span><span class="sxs-lookup"><span data-stu-id="fde96-116">Let's consider the following scenario: you want to create several bookmarks on the report page, with a different selection state in each bookmark.</span></span>

<span data-ttu-id="fde96-117">Сначала выберите точку данных в визуальном элементе.</span><span class="sxs-lookup"><span data-stu-id="fde96-117">First, you select a data point in your visual.</span></span> <span data-ttu-id="fde96-118">Визуальный элемент взаимодействует с Power BI и другими визуальными элементами, передавая выбранные фрагменты на узел.</span><span class="sxs-lookup"><span data-stu-id="fde96-118">The visual interacts with Power BI and other visuals by passing selections to the host.</span></span> <span data-ttu-id="fde96-119">Далее выберите **Добавить** в области **Закладка**. Служба Power BI сохранит текущие выбранные фрагменты для новой закладки.</span><span class="sxs-lookup"><span data-stu-id="fde96-119">You then select **Add** in the **Bookmark** pane, and Power BI saves the current selections for the new bookmark.</span></span>

<span data-ttu-id="fde96-120">Это происходит многократно по мере того, как вы изменяете выбор и добавляете новые закладки.</span><span class="sxs-lookup"><span data-stu-id="fde96-120">It happens repeatedly as you change the selection and add new bookmarks.</span></span> <span data-ttu-id="fde96-121">После создания закладок вы можете переключаться между ними.</span><span class="sxs-lookup"><span data-stu-id="fde96-121">After you create the bookmarks, you can switch between them.</span></span>

<span data-ttu-id="fde96-122">Когда вы выбираете закладку, Power BI восстанавливает сохраненный фильтр или сохраненное состояние выбора и передает их визуальным элементам.</span><span class="sxs-lookup"><span data-stu-id="fde96-122">When you select a bookmark, Power BI restores the saved filter or selection state and passes it to the visuals.</span></span> <span data-ttu-id="fde96-123">Другие визуальные элементы выделяются или фильтруются в соответствии с состоянием, хранящимся в закладке.</span><span class="sxs-lookup"><span data-stu-id="fde96-123">Other visuals are highlighted or filtered according to the state that's stored in the bookmark.</span></span> <span data-ttu-id="fde96-124">Реализацию этих действий обеспечивает узел Power BI.</span><span class="sxs-lookup"><span data-stu-id="fde96-124">The Power BI host is responsible for the actions.</span></span> <span data-ttu-id="fde96-125">Ваш визуальный элемент отвечает за правильное отражение нового состояния выбора (например, за изменение цвета отрисованных точек данных).</span><span class="sxs-lookup"><span data-stu-id="fde96-125">Your visual is responsible for correctly reflecting the new selection state (for example, for changing the colors of rendered data points).</span></span>

<span data-ttu-id="fde96-126">Новое состояние выбора передается визуальному элементу через метод `update`.</span><span class="sxs-lookup"><span data-stu-id="fde96-126">The new selection state is communicated to the visual through the `update` method.</span></span> <span data-ttu-id="fde96-127">Аргумент `options` содержит специальное свойство: `options.jsonFilters`.</span><span class="sxs-lookup"><span data-stu-id="fde96-127">The `options` argument contains a special property, `options.jsonFilters`.</span></span> <span data-ttu-id="fde96-128">Это свойство JSONFilter может содержать `Advanced Filter` и `Tuple Filter`.</span><span class="sxs-lookup"><span data-stu-id="fde96-128">Its JSONFilter property can contain `Advanced Filter` and `Tuple Filter`.</span></span>

<span data-ttu-id="fde96-129">Визуальный элемент должен восстановить значения фильтра, чтобы отобразить соответствующее состояние визуального элемента для выбранной закладки.</span><span class="sxs-lookup"><span data-stu-id="fde96-129">The visual should restore the filter values to display the corresponding state of the visual for the selected bookmark.</span></span> <span data-ttu-id="fde96-130">Либо используйте специальный метод `registerOnSelectCallback` интерфейса ISelectionManager, зарегистрированный в вызове функции обратного вызова, если визуальный элемент использует только выбранные фрагменты.</span><span class="sxs-lookup"><span data-stu-id="fde96-130">Or, if the visual uses selections only, you can use the special callback function call that's registered as the `registerOnSelectCallback` method of ISelectionManager.</span></span>

### <a name="visuals-with-selection"></a><span data-ttu-id="fde96-131">Визуальные элементы с выбранными фрагментами</span><span class="sxs-lookup"><span data-stu-id="fde96-131">Visuals with Selection</span></span>

<span data-ttu-id="fde96-132">Если ваш визуальный элемент взаимодействует с другими визуальными элементами посредством [выбора](https://github.com/Microsoft/PowerBI-visuals/blob/master/Tutorial/Selection.md), вы можете добавить закладки любым из двух способов:</span><span class="sxs-lookup"><span data-stu-id="fde96-132">If your visual interacts with other visuals by using [Selection](https://github.com/Microsoft/PowerBI-visuals/blob/master/Tutorial/Selection.md), you can add bookmarks in either of two ways:</span></span>

* <span data-ttu-id="fde96-133">Если визуальный элемент еще не использовал [InteractivityService](https://github.com/Microsoft/powerbi-visuals-utils-interactivityutils/blob/master/docs/api/interactivityService.md), вы можете использовать метод `FilterManager.restoreSelectionIds`.</span><span class="sxs-lookup"><span data-stu-id="fde96-133">If the visual hasn't already used [InteractivityService](https://github.com/Microsoft/powerbi-visuals-utils-interactivityutils/blob/master/docs/api/interactivityService.md), you can use the `FilterManager.restoreSelectionIds` method.</span></span>

* <span data-ttu-id="fde96-134">Если визуальный элемент уже использует [InteractivityService](https://github.com/Microsoft/powerbi-visuals-utils-interactivityutils/blob/master/docs/api/interactivityService.md) для управления выбранными фрагментами, следует использовать метод `applySelectionFromFilter` в экземпляре `InteractivityService`.</span><span class="sxs-lookup"><span data-stu-id="fde96-134">If the visual already uses [InteractivityService](https://github.com/Microsoft/powerbi-visuals-utils-interactivityutils/blob/master/docs/api/interactivityService.md) to manage selections, you should use the `applySelectionFromFilter` method in the instance of `InteractivityService`.</span></span>

#### <a name="use-iselectionmanagerregisteronselectcallback"></a><span data-ttu-id="fde96-135">Использование ISelectionManager.registerOnSelectCallback</span><span class="sxs-lookup"><span data-stu-id="fde96-135">Use ISelectionManager.registerOnSelectCallback</span></span>

<span data-ttu-id="fde96-136">Когда вы выбираете закладку, Power BI вызывает метод `callback` визуального элемента с соответствующими выбранными фрагментами.</span><span class="sxs-lookup"><span data-stu-id="fde96-136">When you select a bookmark, Power BI calls the `callback` method of the visual with the corresponding selections.</span></span> 

```typescript
this.selectionManager.registerOnSelectCallback(
    (ids: ISelectionId[]) => {
        //called when a selection was set by Power BI
    });
);
```

<span data-ttu-id="fde96-137">Предположим, что в вашем визуальном элементе присутствует точка данных, созданная в методе [visualTransform](https://github.com/Microsoft/PowerBI-visuals-sampleBarChart/blob/master/src/barChart.ts#L74).</span><span class="sxs-lookup"><span data-stu-id="fde96-137">Let's assume that you have a data point in your visual that was created in the [visualTransform](https://github.com/Microsoft/PowerBI-visuals-sampleBarChart/blob/master/src/barChart.ts#L74) method.</span></span>

<span data-ttu-id="fde96-138">При этом `datapoints` выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="fde96-138">And `datapoints` looks like:</span></span>

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

<span data-ttu-id="fde96-139">У вас есть `visualDataPoints` в качестве точек данных и массив `ids`, переданный в функцию `callback`.</span><span class="sxs-lookup"><span data-stu-id="fde96-139">You now have `visualDataPoints` as your data points and the `ids` array passed to the `callback` function.</span></span>

<span data-ttu-id="fde96-140">На этом этапе визуальный элемент должен сравнить массив `ISelectionId[]` с выбранными фрагментами в массиве `visualDataPoints` и отметить соответствующие точки данных как выбранные.</span><span class="sxs-lookup"><span data-stu-id="fde96-140">At this point, the visual should compare the `ISelectionId[]` array with the selections in your `visualDataPoints` array and mark the corresponding data points as selected.</span></span>

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

<span data-ttu-id="fde96-141">После обновления точек данных они будут отражать текущее состояние выбора, сохраненное в объекте `filter`.</span><span class="sxs-lookup"><span data-stu-id="fde96-141">After you update the data points, they'll reflect the current selection state that's stored in the `filter` object.</span></span> <span data-ttu-id="fde96-142">Затем при отрисовке точек данных состояние выбора пользовательского визуального элемента будет соответствовать состоянию закладки.</span><span class="sxs-lookup"><span data-stu-id="fde96-142">Then, when the data points are rendered, the custom visual's selection state will match the state of the bookmark.</span></span>

### <a name="use-interactivityservice-for-control-selections-in-the-visual"></a><span data-ttu-id="fde96-143">Использование InteractivityService для управления выбранными фрагментами в визуальном элементе</span><span class="sxs-lookup"><span data-stu-id="fde96-143">Use InteractivityService for control selections in the visual</span></span>

<span data-ttu-id="fde96-144">Если визуальный элемент использует `InteractivityService`, дополнительные действия для обеспечения в нем поддержки закладок не требуются.</span><span class="sxs-lookup"><span data-stu-id="fde96-144">If your visual uses `InteractivityService`, you don't need any additional actions to support the bookmarks in your visual.</span></span>

<span data-ttu-id="fde96-145">Когда вы выбираете закладку, программа обрабатывает состояние выбора визуального элемента.</span><span class="sxs-lookup"><span data-stu-id="fde96-145">When you select bookmarks, the utility handles the visual's selection state.</span></span>

### <a name="visuals-with-a-filter"></a><span data-ttu-id="fde96-146">Визуальные элементы с фильтром</span><span class="sxs-lookup"><span data-stu-id="fde96-146">Visuals with a filter</span></span>

<span data-ttu-id="fde96-147">Предположим, что визуальный элемент создает фильтр данных по диапазону дат.</span><span class="sxs-lookup"><span data-stu-id="fde96-147">Let's assume that the visual creates a filter of data by date range.</span></span> <span data-ttu-id="fde96-148">`startDate` и `endDate` выступают в качестве начальной и конечной дат диапазона.</span><span class="sxs-lookup"><span data-stu-id="fde96-148">You have `startDate` and `endDate` as the start and end dates of the range.</span></span>

<span data-ttu-id="fde96-149">Визуальный элемент создает расширенный фильтр и вызывает метод узла `applyJsonFilter` для фильтрации данных по соответствующим условиям.</span><span class="sxs-lookup"><span data-stu-id="fde96-149">The visual creates an advanced filter and calls the host method `applyJsonFilter` to filter data by the relevant conditions.</span></span>

<span data-ttu-id="fde96-150">В качестве целевого объекта выступает таблица, которая используется для фильтрации.</span><span class="sxs-lookup"><span data-stu-id="fde96-150">The target is the table that's used for filtering.</span></span>

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

<span data-ttu-id="fde96-151">Каждый раз, когда вы щелкаете закладку, пользовательский визуальный элемент получает вызов `update`.</span><span class="sxs-lookup"><span data-stu-id="fde96-151">Each time you select a bookmark, the custom visual gets an `update` call.</span></span>

<span data-ttu-id="fde96-152">Пользовательский визуальный элемент должен проверить фильтр в объекте:</span><span class="sxs-lookup"><span data-stu-id="fde96-152">The custom visual should check the filter in the object:</span></span>

```typescript
const filter: IAdvancedFilter = FilterManager.restoreFilter(
    && options.jsonFilters
    && options.jsonFilters[0] as any
) as IAdvancedFilter;
```

<span data-ttu-id="fde96-153">Если объект `filter` не имеет значение NULL, визуальный элемент должен восстановить условия фильтра из объекта:</span><span class="sxs-lookup"><span data-stu-id="fde96-153">If the `filter` object isn't null, the visual should restore the filter conditions from the object:</span></span>

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

<span data-ttu-id="fde96-154">После этого визуальный элемент должен изменить свое внутреннее состояние, чтобы отразить текущие условия.</span><span class="sxs-lookup"><span data-stu-id="fde96-154">After that, the visual should change its internal state to reflect the current conditions.</span></span> <span data-ttu-id="fde96-155">Внутреннее состояние включает в себя точки данных и объекты визуализации (линии, прямоугольники и т. д.).</span><span class="sxs-lookup"><span data-stu-id="fde96-155">The internal state includes the data points and visualization objects (lines, rectangles, and so on).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fde96-156">В сценарии с закладками отчета визуальный элемент не должен вызывать `applyJsonFilter` для фильтрации других визуальных элементов.</span><span class="sxs-lookup"><span data-stu-id="fde96-156">In the report bookmarks scenario, the visual shouldn't call `applyJsonFilter` to filter the other visuals.</span></span> <span data-ttu-id="fde96-157">Они уже будут фильтроваться Power BI.</span><span class="sxs-lookup"><span data-stu-id="fde96-157">They will already be filtered by Power BI.</span></span>

<span data-ttu-id="fde96-158">Визуальный элемент среза временной шкалы изменяет селектор диапазонов на соответствующие диапазоны данных.</span><span class="sxs-lookup"><span data-stu-id="fde96-158">The Timeline Slicer visual changes the range selector to the corresponding data ranges.</span></span>

<span data-ttu-id="fde96-159">Дополнительные сведения см. в [репозитории среза временной шкалы](https://github.com/Microsoft/powerbi-visuals-timeline/commit/606f1152f59f82b5b5a367ff3b117372d129e597?diff=unified#diff-b6ef9a9ac3a3225f8bd0de84bee0a0df).</span><span class="sxs-lookup"><span data-stu-id="fde96-159">For more information, see the [Timeline Slicer repository](https://github.com/Microsoft/powerbi-visuals-timeline/commit/606f1152f59f82b5b5a367ff3b117372d129e597?diff=unified#diff-b6ef9a9ac3a3225f8bd0de84bee0a0df).</span></span>

### <a name="filter-the-state-to-save-visual-properties-in-bookmarks"></a><span data-ttu-id="fde96-160">Фильтрация состояния для сохранения свойств визуальных элементов в закладках</span><span class="sxs-lookup"><span data-stu-id="fde96-160">Filter the state to save visual properties in bookmarks</span></span>

<span data-ttu-id="fde96-161">Свойство `filterState` включает свойство в фильтрацию.</span><span class="sxs-lookup"><span data-stu-id="fde96-161">The `filterState` property makes a property of a part of filtering.</span></span> <span data-ttu-id="fde96-162">Визуальный элемент может хранить различные значения в закладках.</span><span class="sxs-lookup"><span data-stu-id="fde96-162">The visual can store a variety of values in bookmarks.</span></span>

<span data-ttu-id="fde96-163">Чтобы сохранить значение свойства как состояние фильтра, пометьте свойство объекта как `"filterState": true` в файле *capabilities.json*.</span><span class="sxs-lookup"><span data-stu-id="fde96-163">To save the property value as a filter state, mark the object property as `"filterState": true` in the *capabilities.json* file.</span></span>

<span data-ttu-id="fde96-164">Например, в срезе временной шкалы хранятся значения свойства `Granularity` в фильтре.</span><span class="sxs-lookup"><span data-stu-id="fde96-164">For example, the Timeline Slicer stores the `Granularity` property values in a filter.</span></span> <span data-ttu-id="fde96-165">Это позволяет изменить текущую степень детализации при изменении закладок.</span><span class="sxs-lookup"><span data-stu-id="fde96-165">It allows the current granularity to change as you change the bookmarks.</span></span>

<span data-ttu-id="fde96-166">Дополнительные сведения см. в [репозитории среза временной шкалы](https://github.com/microsoft/powerbi-visuals-timeline/commit/8b7d82dd23cd2bd71817f1bc5d1e1732347a185e#diff-290828b604cfa62f1cb310f2e90c52fdR334).</span><span class="sxs-lookup"><span data-stu-id="fde96-166">For more information, see the [Timeline Slicer repository](https://github.com/microsoft/powerbi-visuals-timeline/commit/8b7d82dd23cd2bd71817f1bc5d1e1732347a185e#diff-290828b604cfa62f1cb310f2e90c52fdR334).</span></span>
