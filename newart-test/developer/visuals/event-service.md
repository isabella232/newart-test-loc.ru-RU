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
# <a name="render-events-in-power-bi-visuals"></a><span data-ttu-id="959af-103">События отрисовки в визуальных элементах Power BI</span><span class="sxs-lookup"><span data-stu-id="959af-103">Render events in Power BI visuals</span></span>

<span data-ttu-id="959af-104">Новый API состоит из трех методов (`started`, `finished` или `failed`), которые должны вызываться во время отрисовки.</span><span class="sxs-lookup"><span data-stu-id="959af-104">The new API consists of three methods (`started`, `finished`, or `failed`) that should be called during rendering.</span></span>

<span data-ttu-id="959af-105">При запуске отрисовки код визуального элемента Power BI вызывает метод `renderingStarted`, чтобы указать, что процесс отрисовки запущен.</span><span class="sxs-lookup"><span data-stu-id="959af-105">When rendering starts, the Power BI visual code calls the `renderingStarted` method to indicate that the rendering process has started.</span></span>

<span data-ttu-id="959af-106">Если отрисовка успешно завершена, код визуального элемента Power BI немедленно вызовет метод `renderingFinished`, уведомляющий прослушиватели (в основном это *экспорт в PDF* и *экспорт в PowerPoint*) о том, что изображение визуального элемента готово для экспорта.</span><span class="sxs-lookup"><span data-stu-id="959af-106">If rendering is completed successfully, the Power BI visual code immediately calls the `renderingFinished` method, notifying the listeners (primarily, *export to PDF* and *export to PowerPoint*) that the visual's image is ready for export.</span></span>

<span data-ttu-id="959af-107">Если во время этого процесса возникает проблема, визуальный элемент Power BI не отрисовывается.</span><span class="sxs-lookup"><span data-stu-id="959af-107">If a problem occurs during the process, the Power BI visual is prevented from being rendered successfully.</span></span> <span data-ttu-id="959af-108">Код визуального элемента Power BI должен вызывать метод `renderingFailed`, уведомляющий прослушиватели о том, что процесс отрисовки не завершен.</span><span class="sxs-lookup"><span data-stu-id="959af-108">To notify the listeners that the rendering process hasn't been completed, the Power BI visual code should call the `renderingFailed` method.</span></span> <span data-ttu-id="959af-109">Этот метод также предоставляет необязательную строку, указывающую на причину сбоя.</span><span class="sxs-lookup"><span data-stu-id="959af-109">This method also provides an optional string to provide a reason for the failure.</span></span>

## <a name="usage"></a><span data-ttu-id="959af-110">Usage</span><span class="sxs-lookup"><span data-stu-id="959af-110">Usage</span></span>

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

### <a name="sample-the-visual-displays-no-animations"></a><span data-ttu-id="959af-111">Пример. Визуальный элемент не отображает анимацию</span><span class="sxs-lookup"><span data-stu-id="959af-111">Sample: The visual displays no animations</span></span>

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

### <a name="sample-the-visual-displays-animations"></a><span data-ttu-id="959af-112">Пример. Визуальный элемент отображает анимацию</span><span class="sxs-lookup"><span data-stu-id="959af-112">Sample: The visual displays animations</span></span>

<span data-ttu-id="959af-113">Если визуальный элемент содержит анимации или асинхронные функции для отрисовки, метод `renderingFinished` должен вызываться после анимации или внутри асинхронной функции.</span><span class="sxs-lookup"><span data-stu-id="959af-113">If the visual has animations or async functions for rendering, the `renderingFinished` method should be called after the animation or inside async function.</span></span>

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

## <a name="rendering-events-for-visual-certification"></a><span data-ttu-id="959af-114">События отрисовки для сертификации визуального элемента</span><span class="sxs-lookup"><span data-stu-id="959af-114">Rendering events for visual certification</span></span>

<span data-ttu-id="959af-115">Поддержка событий отрисовки визуальным элементом является одним из требований при сертификации визуальных элементов.</span><span class="sxs-lookup"><span data-stu-id="959af-115">One requirement of visuals certification is the support of rendering events by the visual.</span></span> <span data-ttu-id="959af-116">Дополнительные сведения см. в статье [Требования к сертификации](https://docs.microsoft.com/power-bi/power-bi-custom-visuals-certified?#certification-requirements).</span><span class="sxs-lookup"><span data-stu-id="959af-116">For more information, see [certification requirements](https://docs.microsoft.com/power-bi/power-bi-custom-visuals-certified?#certification-requirements).</span></span>
