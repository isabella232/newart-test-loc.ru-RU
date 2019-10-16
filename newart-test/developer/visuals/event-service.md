---
title: События отрисовки в визуальных элементах Power BI
description: Визуальные элементы Power BI могут уведомлять Power BI о том, что они готовы к экспорту в PowerPoint или PDF.
author: Yarovinsky
ms.author: alexyar
manager: rkarlin
ms.reviewer: sranins
ms.service: powerbi
ms.subservice: powerbi-custom-visuals
ms.topic: conceptual
ms.date: 06/18/2019
ms.openlocfilehash: b481ce94e5025045466a05d71e30a00f02be7ead
ms.sourcegitcommit: a1c994bc8fa5f072fce13a6f35079e87d45f00f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72424302"
---
# <a name="render-events-in-power-bi-visuals"></a>События отрисовки в визуальных элементах Power BI

Новый API состоит из трех методов (`started`, `finished` или `failed`), которые должны вызываться во время отрисовки.

При запуске отрисовки код визуального элемента Power BI вызывает метод `renderingStarted`, чтобы указать, что процесс отрисовки запущен.

Если отрисовка успешно завершена, код визуального элемента Power BI немедленно вызовет метод `renderingFinished`, уведомляющий прослушиватели (в основном это *экспорт в PDF* и *экспорт в PowerPoint*) о том, что изображение визуального элемента готово для экспорта.

Если во время этого процесса возникает проблема, визуальный элемент Power BI не отрисовывается. Код визуального элемента Power BI должен вызывать метод `renderingFailed`, уведомляющий прослушиватели о том, что процесс отрисовки не завершен. Этот метод также предоставляет необязательную строку, указывающую на причину сбоя.

## <a name="usage"></a>Usage

```typescript
export interface IVisualHost extends extensibility.IVisualHost {
    eventService: IVisualEventService ;
}

/**
 * An interface for reporting rendering events
 */
export interface IVisualEventService {
    /**
     * Should be called just before the actual rendering starts, 
     * usually at the start of the update method
     *
     * @param options - the visual update options received as an update parameter
     */
    renderingStarted(options: VisualUpdateOptions): void;

    /**
     * Should be called immediately after rendering finishes successfully
     * 
     * @param options - the visual update options received as an update parameter
     */
    renderingFinished(options: VisualUpdateOptions): void;

    /**
     * Called when rendering fails, with an optional reason string
     * 
     * @param options - the visual update options received as an update parameter
     * @param reason - the optional failure reason string
     */
    renderingFailed(options: VisualUpdateOptions, reason?: string): void;
}
```

### <a name="sample-the-visual-displays-no-animations"></a>Пример. Визуальный элемент не отображает анимацию

```typescript
    export class Visual implements IVisual {
        ...
        private events: IVisualEventService;
        ...

        constructor(options: VisualConstructorOptions) {
            ...
            this.events = options.host.eventService;
            ...
        }

        public update(options: VisualUpdateOptions) {
            this.events.renderingStarted(options);
            ...
            this.events.renderingFinished(options);
        }
```

### <a name="sample-the-visual-displays-animations"></a>Пример. Визуальный элемент отображает анимацию

Если визуальный элемент содержит анимации или асинхронные функции для отрисовки, метод `renderingFinished` должен вызываться после анимации или внутри асинхронной функции.

```typescript
    export class Visual implements IVisual {
        ...
        private events: IVisualEventService;
        private element: HTMLElement;
        ...

        constructor(options: VisualConstructorOptions) {
            ...
            this.events = options.host.eventService;
            this.element = options.element;
            ...
        }

        public update(options: VisualUpdateOptions) {
            this.events.renderingStarted(options);
            ...
            // Learn more at https://github.com/d3/d3-transition/blob/master/README.md#transition_end
            d3.select(this.element).transition().duration(100).style("opacity","0").end().then(() => {
                // renderingFinished called after transition end
                this.events.renderingFinished(options);
            });
        }
```

## <a name="rendering-events-for-visual-certification"></a>События отрисовки для сертификации визуального элемента

Поддержка событий отрисовки визуальным элементом является одним из требований при сертификации визуальных элементов. Дополнительные сведения см. в статье [Требования к сертификации](https://docs.microsoft.com/power-bi/power-bi-custom-visuals-certified?#certification-requirements).
