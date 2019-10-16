---
title: Добавление целевой страницы в визуальные элементы Power BI
description: В этой статье описывается добавление целевой страницы в визуальные элементы Power BI.
author: KesemSharabi
ms.author: kesharab
manager: rkarlin
ms.reviewer: sranins
ms.service: powerbi
ms.subservice: powerbi-custom-visuals
ms.topic: conceptual
ms.date: 06/18/2019
ms.openlocfilehash: 79e362796e0694022bceb38f111a62bb62ddbabd
ms.sourcegitcommit: a1c994bc8fa5f072fce13a6f35079e87d45f00f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72424417"
---
# <a name="add-a-landing-page-to-your-power-bi-visuals"></a><span data-ttu-id="14e88-103">Добавление целевой страницы в визуальные элементы Power BI</span><span class="sxs-lookup"><span data-stu-id="14e88-103">Add a landing page to your Power BI visuals</span></span>

<span data-ttu-id="14e88-104">С помощью API версии 2.3.0 вы можете добавить целевую страницу в визуальные элементы Power BI.</span><span class="sxs-lookup"><span data-stu-id="14e88-104">With API 2.3.0, you can add a landing page to your Power BI visuals.</span></span> <span data-ttu-id="14e88-105">Для этого необходимо добавить к возможностям элемент `supportsLandingPage` и присвоить ему значение true.</span><span class="sxs-lookup"><span data-stu-id="14e88-105">To do so, add `supportsLandingPage` to the capabilities, and set it to true.</span></span> <span data-ttu-id="14e88-106">В результате этого действия визуальный элемент будет инициализироваться и добавляться до того, как в него будут добавлены данные.</span><span class="sxs-lookup"><span data-stu-id="14e88-106">This action initializes and updates your visual before you add data to it.</span></span> <span data-ttu-id="14e88-107">Поскольку для визуального элемента более не отображается водяной знак, вы можете разработать собственную целевую страницу, которая будет отображаться в этом визуальном элементе в том случае, если в нем отсутствуют данные.</span><span class="sxs-lookup"><span data-stu-id="14e88-107">Because the visual no longer shows a watermark, you can design your own landing page to be displayed in the visual as long as it has no data.</span></span>

```typescript
export class BarChart implements IVisual {
    //...
    private element: HTMLElement;
    private isLandingPageOn: boolean;
    private LandingPageRemoved: boolean;
    private LandingPage: d3.Selection<any>;

    constructor(options: VisualConstructorOptions) {
            //...
            this.element = options.element;
            //...
    }

    public update(options: VisualUpdateOptions) {
    //...
        this.HandleLandingPage(options);
    }

    private HandleLandingPage(options: VisualUpdateOptions) {
        if(!options.dataViews || !options.dataViews.length) {
            if(!this.isLandingPageOn) {
                this.isLandingPageOn = true;
                const SampleLandingPage: Element = this.createSampleLandingPage(); //create a landing page
                this.element.appendChild(SampleLandingPage);
                this.LandingPage = d3.select(SampleLandingPage);
            }

        } else {
                if(this.isLandingPageOn && !this.LandingPageRemoved){
                    this.LandingPageRemoved = true;
                    this.LandingPage.remove();
                }
        }
    }
```

<span data-ttu-id="14e88-108">На следующем рисунке показан пример целевой страницы:</span><span class="sxs-lookup"><span data-stu-id="14e88-108">An example landing page is shown in the following image:</span></span>

![Снимок экрана с целевой страницей](./media/landing-page.png)
