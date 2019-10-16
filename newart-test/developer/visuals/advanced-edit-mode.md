---
title: Режим расширенного редактирования для визуальных элементов Power BI
description: В этой статье описывается настройка расширенных элементов управления пользовательского интерфейса для визуальных элементов Power BI.
author: KesemSharabi
ms.author: kesharab
manager: rkarlin
ms.reviewer: sranins
ms.service: powerbi
ms.subservice: powerbi-custom-visuals
ms.topic: conceptual
ms.date: 06/18/2019
ms.openlocfilehash: da72cf603027bc97060e7a00ed4a4e959a3a92e2
ms.sourcegitcommit: a1c994bc8fa5f072fce13a6f35079e87d45f00f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72424202"
---
# <a name="advanced-edit-mode-in-power-bi-visuals"></a><span data-ttu-id="c39a5-103">Режим расширенного редактирования для визуальных элементов Power BI</span><span class="sxs-lookup"><span data-stu-id="c39a5-103">Advanced edit mode in Power BI visuals</span></span>

<span data-ttu-id="c39a5-104">Если вы хотите использовать расширенные элементы управления пользовательского интерфейса для визуального элемента Power BI, примените режим расширенного редактирования.</span><span class="sxs-lookup"><span data-stu-id="c39a5-104">If you require advanced UI controls in your Power BI visual, you can take advantage of advanced edit mode.</span></span> <span data-ttu-id="c39a5-105">В режиме редактирования отчета нажмите кнопку **Изменить**, чтобы перейти в режим редактирования **Расширенный**.</span><span class="sxs-lookup"><span data-stu-id="c39a5-105">When you're in report editing mode, you select an **Edit** button to set the edit mode to **Advanced**.</span></span> <span data-ttu-id="c39a5-106">Визуальный элемент может использовать флаг `EditMode`, чтобы определить, нужно ли отображать такой элемент управления пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="c39a5-106">The visual can use the `EditMode` flag to determine whether it should display this UI control.</span></span>

<span data-ttu-id="c39a5-107">По умолчанию визуальный элемент не поддерживает режим расширенного редактирования.</span><span class="sxs-lookup"><span data-stu-id="c39a5-107">By default, the visual doesn't support advanced edit mode.</span></span> <span data-ttu-id="c39a5-108">Если требуется другое поведение, вы можете явно задать его в файле *capabilities.json* визуального элемента с помощью свойства `advancedEditModeSupport`.</span><span class="sxs-lookup"><span data-stu-id="c39a5-108">If a different behavior is required, you can explicitly state this in the visual's *capabilities.json* file by setting the `advancedEditModeSupport` property.</span></span>

<span data-ttu-id="c39a5-109">Возможные значения:</span><span class="sxs-lookup"><span data-stu-id="c39a5-109">The possible values are:</span></span>

- <span data-ttu-id="c39a5-110">`0` — NotSupported</span><span class="sxs-lookup"><span data-stu-id="c39a5-110">`0` - NotSupported</span></span>

- <span data-ttu-id="c39a5-111">`1` — SupportedNoAction</span><span class="sxs-lookup"><span data-stu-id="c39a5-111">`1` - SupportedNoAction</span></span>

- <span data-ttu-id="c39a5-112">`2` — SupportedInFocus</span><span class="sxs-lookup"><span data-stu-id="c39a5-112">`2` - SupportedInFocus</span></span>

## <a name="enter-advanced-edit-mode"></a><span data-ttu-id="c39a5-113">Вход в режим расширенного редактирования</span><span class="sxs-lookup"><span data-stu-id="c39a5-113">Enter advanced edit mode</span></span>

<span data-ttu-id="c39a5-114">Кнопка **Изменить** отображается в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="c39a5-114">An **Edit** button is displayed if:</span></span>

* <span data-ttu-id="c39a5-115">Свойству `advancedEditModeSupport` в файле *capabilities.json* присвоено значение `SupportedNoAction` или `SupportedInFocus`.</span><span class="sxs-lookup"><span data-stu-id="c39a5-115">The `advancedEditModeSupport` property is set in the *capabilities.json* file to either `SupportedNoAction` or `SupportedInFocus`.</span></span>

* <span data-ttu-id="c39a5-116">Визуальный элемент открыт в режиме редактирования отчета.</span><span class="sxs-lookup"><span data-stu-id="c39a5-116">The visual is viewed in report editing mode.</span></span>

<span data-ttu-id="c39a5-117">Если свойство `advancedEditModeSupport` в файле *capabilities.json* отсутствует или имеет значение `NotSupported`, кнопка **Изменить** не отображается.</span><span class="sxs-lookup"><span data-stu-id="c39a5-117">If `advancedEditModeSupport` property is missing from the *capabilities.json* file or set to `NotSupported`, the **Edit** button is not displayed.</span></span>

![Вход в режим редактирования](./media/edit-mode.png)

<span data-ttu-id="c39a5-119">Если нажать кнопку **Изменить**, визуальный элемент получает вызов update(), в котором элементу EditMode присвоено значение `Advanced`.</span><span class="sxs-lookup"><span data-stu-id="c39a5-119">When you select **Edit**, the visual gets an update() call with EditMode set to `Advanced`.</span></span> <span data-ttu-id="c39a5-120">В зависимости от заданного в файле *capabilities.json* значения выполняются следующие действия:</span><span class="sxs-lookup"><span data-stu-id="c39a5-120">Depending on the value that's set in the *capabilities.json* file, the following actions occur:</span></span>

* <span data-ttu-id="c39a5-121">`SupportedNoAction` — не требуется никаких дальнейших действий со стороны узла.</span><span class="sxs-lookup"><span data-stu-id="c39a5-121">`SupportedNoAction`: No further action is required by the host.</span></span>
* <span data-ttu-id="c39a5-122">`SupportedInFocus` — узел развертывает визуальный элемент в режим фокусировки.</span><span class="sxs-lookup"><span data-stu-id="c39a5-122">`SupportedInFocus`: The host pops out the visual into in focus mode.</span></span>

## <a name="exit-advanced-edit-mode"></a><span data-ttu-id="c39a5-123">Выход из режима расширенного редактирования</span><span class="sxs-lookup"><span data-stu-id="c39a5-123">Exit advanced edit mode</span></span>

<span data-ttu-id="c39a5-124">Кнопка **Назад к отчету** отображается в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="c39a5-124">The **Back to report** button is displayed if:</span></span>

* <span data-ttu-id="c39a5-125">Свойству `advancedEditModeSupport` в файле *capabilities.json* присвоено значение `SupportedInFocus`.</span><span class="sxs-lookup"><span data-stu-id="c39a5-125">The `advancedEditModeSupport` property is set in the *capabilities.json* file to `SupportedInFocus`.</span></span>
