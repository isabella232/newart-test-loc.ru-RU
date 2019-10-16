---
title: API фильтров визуальных элементов в визуальных элементах Power BI
description: В этой статье описывается фильтрация визуальными элементами Power BI других визуальных элементов.
author: KesemSharabi
ms.author: kesharab
manager: rkarlin
ms.reviewer: sranins
ms.service: powerbi
ms.subservice: powerbi-custom-visuals
ms.topic: conceptual
ms.date: 06/18/2019
ms.openlocfilehash: 98ebc87cf5a6b7bf8f0b8b88d4ff498edfd5bf9a
ms.sourcegitcommit: a1c994bc8fa5f072fce13a6f35079e87d45f00f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72424547"
---
# <a name="the-visual-filters-api-in-power-bi-visuals"></a>API фильтров визуальных элементов в визуальных элементах Power BI

API фильтров визуальных элементов позволяет фильтровать данные в визуальных элементах Power BI. Основное отличие от других способов выбора заключается в том, что другие визуальные элементы будут фильтроваться всегда, независимо от поддержки выделения другими визуальными элементами.

Чтобы включить фильтрацию для визуального элемента, он должен содержать объект `filter` в разделе `general` файла *capabilities.json*.

```json
"objects": {
        "general": {
            "displayName": "General",
            "displayNameKey": "formattingGeneral",
            "properties": {
                "filter": {
                    "type": {
                        "filter": true
                    }
                }
            }
        }
    }
```

Интерфейсы API фильтров визуальных фильтров доступны в пакете [powerbi-models](https://www.npmjs.com/package/powerbi-models). Этот пакет также содержит классы для создания экземпляров фильтра.

```cmd
npm install powerbi-models --save
```

Если вы используете более раннюю версию средств (до 3. x. x), следует включить `powerbi-models` в пакет визуальных элементов. Дополнительные сведения см. в кратком руководстве [Добавление API расширенного фильтра к пользовательскому визуальному элементу](https://github.com/Microsoft/powerbi-visuals-sampleslicer/blob/master/doc/AddingAdvancedFilterAPI.md).

Все фильтры расширяют интерфейс `IFilter`, как показано в следующем коде:

```typescript
export interface IFilter {
    $schema: string;
    target: IFilterTarget;
}
```
Здесь:
* `target` — это столбец таблицы в источнике данных.

## <a name="the-basic-filter-api"></a>API базового фильтра

Интерфейс базового фильтра показан в следующем коде:

```typescript
export interface IBasicFilter extends IFilter {
    operator: BasicFilterOperators;
    values: (string | number | boolean)[];
}
```

Здесь:
* `operator` — это перечисление со значениями *In*, *NotIn* и *All*.
* `values` — это значения для условия.

Пример базового фильтра:

```typescript
let basicFilter = {
    target: {
        column: "Col1"
    },
    operator: "In",
    values: [1,2,3]
}
```

Этот фильтр имеет следующее значение: "вывести все строки, где `col1` равняется 1, 2 или 3".

Эквивалент SQL:

```sql
SELECT * FROM table WHERE col1 IN ( 1 , 2 , 3 )
```

Чтобы создать фильтр, можно использовать класс BasicFilter в `powerbi-models`.

Если вы используете более старую версию средства, следует получить экземпляр моделей в объекте окна с помощью `window['powerbi-models']`, как показано в следующем коде:

```javascript
let categories: DataViewCategoricalColumn = this.dataView.categorical.categories[0];

let target: IFilterColumnTarget = {
    table: categories.source.queryName.substr(0, categories.source.queryName.indexOf('.')),
    column: categories.source.displayName
};

let values = [ 1, 2, 3 ];

let filter: IBasicFilter = new window['powerbi-models'].BasicFilter(target, "In", values);
```

Визуальный элемент вызывает фильтр, используя метод applyJsonFilter() в интерфейсе узла IVisualHost, предоставляемом визуальному элементу в конструкторе.

```typescript
visualHost.applyJsonFilter(filter, "general", "filter", FilterAction.merge);
```

## <a name="the-advanced-filter-api"></a>API расширенного фильтра

[API расширенного фильтра](https://github.com/Microsoft/powerbi-models) позволяет выполнять сложные запросы выборки и фильтрации точек данных на основе нескольких визуальных элементов (например, *LessThan*, *Contains*, *Is*, *IsBlank* и т. д.).

Этот фильтр появился в API визуальных элементов 1.7.0.

API расширенного фильтра также требует `target` с `table` и именем `column`. Однако операторами API расширенного фильтра являются *And* и *Or*. 

Кроме того, фильтр использует условия вместо значений с интерфейсом:

```typescript
interface IAdvancedFilterCondition {
    value: (string | number | boolean);
    operator: AdvancedFilterConditionOperators;
}
```

Условными операторами для параметра `operator` являются *None*, *LessThan*, *LessThanOrEqual*, *GreaterThan*, *GreaterThanOrEqual*, *Contains*, *DoesNotContain*, *StartsWith*, *DoesNotStartWith*, *Is*, *IsNot*, *IsBlank* и IsNotBlank.

```javascript
let categories: DataViewCategoricalColumn = this.dataView.categorical.categories[0];

let target: IFilterColumnTarget = {
    table: categories.source.queryName.substr(0, categories.source.queryName.indexOf('.')), // table
    column: categories.source.displayName // col1
};

let conditions: IAdvancedFilterCondition[] = [];

conditions.push({
    operator: "LessThan",
    value: 0
});

let filter: IAdvancedFilter = new window['powerbi-models'].AdvancedFilter(target, "And", conditions);

// invoke the filter
visualHost.applyJsonFilter(filter, "general", "filter", FilterAction.merge);
```

Эквивалент SQL:

```sql
SELECT * FROM table WHERE col1 < 0;
```

Полный пример кода, в котором используется пример API расширенного фильтра, см. в статье [Репозиторий визуальных элементов Sampleslicer](https://github.com/Microsoft/powerbi-visuals-sampleslicer).

## <a name="the-tuple-filter-api-multi-column-filter"></a>API фильтра кортежей (фильтр для нескольких столбцов)

API фильтра кортежей появился в API визуальных элементов 2.3.0. Он аналогичен API базового фильтра, но позволяет определять условия для нескольких столбцов и таблиц.

Интерфейс фильтра показан в следующем коде: 

```typescript
interface ITupleFilter extends IFilter {
    $schema: string;
    filterType: FilterType;
    operator: TupleFilterOperators;
    target: ITupleFilterTarget;
    values: TupleValueType[];
}
```

Здесь:
* `target` — это массив столбцов с именами таблиц:

    ```typescript
    declare type ITupleFilterTarget = IFilterTarget[];
    ```

  Фильтр может обращаться к столбцам из разных таблиц.

* `$schema` — это http://powerbi.com/product/schema#tuple.

* `filterType` — это *FilterType.Tuple*.

* `operator` допускает использование только в операторе *In*.

* `values` — это массив кортежей значений, где каждый кортеж представляет одно разрешенное сочетание значений целевого столбца. 

```typescript
declare type TupleValueType = ITupleElementValue[];

interface ITupleElementValue {
    value: PrimitiveValueType
}
```

Полный пример:

```typescript
let target: ITupleFilterTarget = [
    {
        table: "DataTable",
        column: "Team"
    },
    {
        table: "DataTable",
        column: "Value"
    }
];

let values = [
    [
        // the first column combination value (or the column tuple/vector value) that the filter will pass through
        {
            value: "Team1" // the value for the `Team` column of the `DataTable` table
        },
        {
            value: 5 // the value for the `Value` column of the `DataTable` table
        }
    ],
    [
        // the second column combination value (or the column tuple/vector value) that the filter will pass through
        {
            value: "Team2" // the value for `Team` column of `DataTable` table
        },
        {
            value: 6 // the value for `Value` column of `DataTable` table
        }
    ]
];

let filter: ITupleFilter = {
    $schema: "http://powerbi.com/product/schema#tuple",
    filterType: FilterType.Tuple,
    operator: "In",
    target: target,
    values: values
}

// invoke the filter
visualHost.applyJsonFilter(filter, "general", "filter", FilterAction.merge);
```

> [!IMPORTANT]
> Учитывается порядок имен столбцов и значений условий.

Эквивалент SQL:

```sql
SELECT * FROM DataTable WHERE ( Team = "Team1" AND Value = 5 ) OR ( Team = "Team2" AND Value = 6 );
```  

## <a name="restore-the-json-filter-from-the-data-view"></a>Восстановление фильтра JSON из представления данных

Начиная с API версии 2.2 можно восстановить фильтр JSON из *VisualUpdateOptions*, как показано в следующем коде:

```typescript
export interface VisualUpdateOptions extends extensibility.VisualUpdateOptions {
    viewport: IViewport;
    dataViews: DataView[];
    type: VisualUpdateType;
    viewMode?: ViewMode;
    editMode?: EditMode;
    operationKind?: VisualDataChangeOperationKind;
    jsonFilters?: IFilter[];
}
```

Power BI вызывает метод `update` визуального элемента, когда использует переключение между закладками. При этом визуальный элемент получает соответствующий объект `filter`. Дополнительные сведения см. в статье [Добавление поддержки закладок для визуальных элементов Power BI](bookmarks-support.md).

### <a name="sample-json-filter"></a>Пример фильтра JSON

Пример с кодом фильтра JSON показан на следующем рисунке:

![Код фильтра JSON](./media/json-filter.png)

### <a name="clear-the-json-filter"></a>Очистка фильтра JSON

API фильтра принимает значение `null` фильтра в качестве *сброса* или *очистки*.

```typescript
// invoke the filter
visualHost.applyJsonFilter(null, "general", "filter", FilterAction.merge);
```
