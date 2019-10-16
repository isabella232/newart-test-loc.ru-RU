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
# <a name="advanced-edit-mode-in-power-bi-visuals"></a>Режим расширенного редактирования для визуальных элементов Power BI

Если вы хотите использовать расширенные элементы управления пользовательского интерфейса для визуального элемента Power BI, примените режим расширенного редактирования. В режиме редактирования отчета нажмите кнопку **Изменить**, чтобы перейти в режим редактирования **Расширенный**. Визуальный элемент может использовать флаг `EditMode`, чтобы определить, нужно ли отображать такой элемент управления пользовательского интерфейса.

По умолчанию визуальный элемент не поддерживает режим расширенного редактирования. Если требуется другое поведение, вы можете явно задать его в файле *capabilities.json* визуального элемента с помощью свойства `advancedEditModeSupport`.

Возможные значения:

- `0` — NotSupported

- `1` — SupportedNoAction

- `2` — SupportedInFocus

## <a name="enter-advanced-edit-mode"></a>Вход в режим расширенного редактирования

Кнопка **Изменить** отображается в следующих случаях:

* Свойству `advancedEditModeSupport` в файле *capabilities.json* присвоено значение `SupportedNoAction` или `SupportedInFocus`.

* Визуальный элемент открыт в режиме редактирования отчета.

Если свойство `advancedEditModeSupport` в файле *capabilities.json* отсутствует или имеет значение `NotSupported`, кнопка **Изменить** не отображается.

![Вход в режим редактирования](./media/edit-mode.png)

Если нажать кнопку **Изменить**, визуальный элемент получает вызов update(), в котором элементу EditMode присвоено значение `Advanced`. В зависимости от заданного в файле *capabilities.json* значения выполняются следующие действия:

* `SupportedNoAction` — не требуется никаких дальнейших действий со стороны узла.
* `SupportedInFocus` — узел развертывает визуальный элемент в режим фокусировки.

## <a name="exit-advanced-edit-mode"></a>Выход из режима расширенного редактирования

Кнопка **Назад к отчету** отображается в следующих случаях:

* Свойству `advancedEditModeSupport` в файле *capabilities.json* присвоено значение `SupportedInFocus`.
