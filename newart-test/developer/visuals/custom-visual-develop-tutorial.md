---
title: Разработка визуального элемента Power BI
description: Руководство по разработке пользовательского визуального элемента Power BI
author: KesemSharabi
ms.author: kesharab
manager: rkarlin
ms.reviewer: ''
ms.service: powerbi
ms.subservice: powerbi-custom-visuals
ms.topic: tutorial
ms.date: 03/15/2019
ms.openlocfilehash: 1aa269bc738b873ac36498e2ecf52f2cf06c209d
ms.sourcegitcommit: a1c994bc8fa5f072fce13a6f35079e87d45f00f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72424482"
---
# <a name="tutorial-developing-a-power-bi-visual"></a><span data-ttu-id="944ea-103">Учебник. Разработка Power BIного визуального элемента</span><span class="sxs-lookup"><span data-stu-id="944ea-103">Tutorial: Developing a Power BI visual</span></span>

<span data-ttu-id="944ea-104">Мы предоставляем разработчикам возможность легко добавлять визуальные элементы Power BI в Power BI и использовать их для информационных панелей и отчетов.</span><span class="sxs-lookup"><span data-stu-id="944ea-104">We’re enabling developers to easily add Power BI visuals into Power BI for use in dashboard and reports.</span></span> <span data-ttu-id="944ea-105">Мы опубликовали код для всех визуальных элементов на GitHub, чтобы помочь вам приступить к работе.</span><span class="sxs-lookup"><span data-stu-id="944ea-105">To help you get started, we’ve published the code for all of our visualizations to GitHub.</span></span>

<span data-ttu-id="944ea-106">Кроме платформы визуализации мы предоставили набор тестов и инструменты, которые помогут участникам сообщества создавать высококачественные визуальные элементы Power BI.</span><span class="sxs-lookup"><span data-stu-id="944ea-106">Along with the visualization framework, we’ve provided our test suite and tools to help the community build high-quality Power BI visuals for Power BI.</span></span>

<span data-ttu-id="944ea-107">В этом руководстве показано, как разработать пользовательский визуальный элемент Power BI с именем Circle Card, который отображает форматированное значение внутри круга.</span><span class="sxs-lookup"><span data-stu-id="944ea-107">This tutorial shows you how to develop a Power BI custom visual named Circle Card to display a formatted measure value inside a circle.</span></span> <span data-ttu-id="944ea-108">Визуальный элемент Circle Card поддерживает настройку цвета заливки и толщины линии круга.</span><span class="sxs-lookup"><span data-stu-id="944ea-108">The Circle Card visual supports customization of fill color and thickness of its outline.</span></span>

<span data-ttu-id="944ea-109">В отчете Power BI Desktop карточки будут изменены на элементы Circle Card.</span><span class="sxs-lookup"><span data-stu-id="944ea-109">In the Power BI Desktop report, the cards are modified to become Circle Cards.</span></span>

  ![Пример готового пользовательского визуального элемента Power BI](media/custom-visual-develop-tutorial/circle-cards.png)

<span data-ttu-id="944ea-111">Из этого руководства вы узнаете, как выполнять следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="944ea-111">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="944ea-112">Создание пользовательского визуального элемента Power BI.</span><span class="sxs-lookup"><span data-stu-id="944ea-112">Create a Power BI custom visual.</span></span>
> * <span data-ttu-id="944ea-113">Разработка пользовательского визуального элемента с на основе визуальных элементов D3.</span><span class="sxs-lookup"><span data-stu-id="944ea-113">Develop the custom visual with D3 visual elements.</span></span>
> * <span data-ttu-id="944ea-114">Настройка привязки данных для визуальных элементов.</span><span class="sxs-lookup"><span data-stu-id="944ea-114">Configure data binding with the visual elements.</span></span>
> * <span data-ttu-id="944ea-115">Форматирование значений данных.</span><span class="sxs-lookup"><span data-stu-id="944ea-115">Format data values.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="944ea-116">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="944ea-116">Prerequisites</span></span>

* <span data-ttu-id="944ea-117">Если вы не зарегистрированы в **Power BI**, перед началом работы [пройдите бесплатную регистрацию](https://powerbi.microsoft.com/pricing/).</span><span class="sxs-lookup"><span data-stu-id="944ea-117">If you're not signed up for **Power BI Pro**, [sign up for a free trial](https://powerbi.microsoft.com/pricing/) before you begin.</span></span>
* <span data-ttu-id="944ea-118">У вас должен быть установлен редактор [Visual Studio Code](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="944ea-118">You need [Visual Studio Code](https://www.visualstudio.com/) installed.</span></span>
* <span data-ttu-id="944ea-119">Вам потребуется [Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6) версии 4 или более поздней для пользователей Windows либо [Terminal](https://macpaw.com/how-to/use-terminal-on-mac) для пользователей OSX.</span><span class="sxs-lookup"><span data-stu-id="944ea-119">You need [Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6) version 4 or later for windows users OR the [Terminal](https://macpaw.com/how-to/use-terminal-on-mac) for OSX users.</span></span>

## <a name="setting-up-the-developer-environment"></a><span data-ttu-id="944ea-120">Настройка среды разработки</span><span class="sxs-lookup"><span data-stu-id="944ea-120">Setting up the developer environment</span></span>

<span data-ttu-id="944ea-121">Вам нужно установить еще несколько средств, помимо указанных в предварительных требованиях.</span><span class="sxs-lookup"><span data-stu-id="944ea-121">In addition to the prerequisites, there are a few more tools you need to install.</span></span>

### <a name="installing-nodejs"></a><span data-ttu-id="944ea-122">Установка Node.js</span><span class="sxs-lookup"><span data-stu-id="944ea-122">Installing node.js</span></span>

1. <span data-ttu-id="944ea-123">Чтобы установить Node.js, откройте в веб-браузере страницу [Node.js](https://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="944ea-123">To install Node.js, in a web browser, navigate to [Node.js](https://nodejs.org).</span></span>

2. <span data-ttu-id="944ea-124">Скачайте последнюю версию установщика MSI.</span><span class="sxs-lookup"><span data-stu-id="944ea-124">Download the latest feature MSI installer.</span></span>

3. <span data-ttu-id="944ea-125">Запустите этот установщик и следуйте инструкциям.</span><span class="sxs-lookup"><span data-stu-id="944ea-125">Run the installer, and then follow the installation steps.</span></span> <span data-ttu-id="944ea-126">Примите условия лицензионного соглашения и сохраните все значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="944ea-126">Accept the terms of the license agreement and all defaults.</span></span>

   ![ <span data-ttu-id="944ea-127">Установка Node.js</span><span class="sxs-lookup"><span data-stu-id="944ea-127">Node.js setup</span></span>](media/custom-visual-develop-tutorial/node-js-setup.png)

4. <span data-ttu-id="944ea-128">Перезагрузите компьютер.</span><span class="sxs-lookup"><span data-stu-id="944ea-128">Restart the computer.</span></span>

### <a name="installing-packages"></a><span data-ttu-id="944ea-129">Установка пакетов</span><span class="sxs-lookup"><span data-stu-id="944ea-129">Installing packages</span></span>

<span data-ttu-id="944ea-130">Теперь нужно установить пакет **pbiviz**.</span><span class="sxs-lookup"><span data-stu-id="944ea-130">Now you need to install the **pbiviz** package.</span></span>

1. <span data-ttu-id="944ea-131">После перезагрузки компьютера откройте Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="944ea-131">Open Windows PowerShell after the computer has been restarted.</span></span>

2. <span data-ttu-id="944ea-132">Чтобы установить pbiviz, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="944ea-132">To install pbiviz, enter the following command.</span></span>

    ```powershell
    npm i -g powerbi-visuals-tools
    ```

### <a name="creating-and-installing-a-certificate"></a><span data-ttu-id="944ea-133">Создание и установка сертификата</span><span class="sxs-lookup"><span data-stu-id="944ea-133">Creating and installing a certificate</span></span>

#### <a name="windows"></a><span data-ttu-id="944ea-134">Windows</span><span class="sxs-lookup"><span data-stu-id="944ea-134">Windows</span></span>

1. <span data-ttu-id="944ea-135">Чтобы создать и установить сертификат, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="944ea-135">To create and install a certificate, enter the following command.</span></span>

    ```powershell
    pbiviz --install-cert
    ```

    <span data-ttu-id="944ea-136">Она возвращает результат с *парольной фразой*.</span><span class="sxs-lookup"><span data-stu-id="944ea-136">It returns a result that produces a *passphrase*.</span></span> <span data-ttu-id="944ea-137">В нашем примере *парольная фраза* имеет значение **_15105661266553327_** .</span><span class="sxs-lookup"><span data-stu-id="944ea-137">In this case, the *passphrase* is **_15105661266553327_**.</span></span> <span data-ttu-id="944ea-138">Она также запускает мастер импорта сертификатов.</span><span class="sxs-lookup"><span data-stu-id="944ea-138">It also starts the Certificate Import Wizard.</span></span>

    ![Сертификат, созданный с помощью PowerShell](media/custom-visual-develop-tutorial/cert-create.png)

2. <span data-ttu-id="944ea-140">В мастере импорта сертификатов убедитесь, что в качестве расположения хранилища выбрано значение "Текущий пользователь".</span><span class="sxs-lookup"><span data-stu-id="944ea-140">In the Certificate Import Wizard, verify that the store location is set to Current User.</span></span> <span data-ttu-id="944ea-141">Нажмите кнопку *Далее*.</span><span class="sxs-lookup"><span data-stu-id="944ea-141">Then select *Next*.</span></span>

      ![Установка сертификата](media/custom-visual-develop-tutorial/install-cert-PowerShell.png)

3. <span data-ttu-id="944ea-143">На этапе **Импортируемый файл** выберите *Далее*.</span><span class="sxs-lookup"><span data-stu-id="944ea-143">At the **File to Import** step, select *Next*.</span></span>

4. <span data-ttu-id="944ea-144">На шаге **защита закрытого ключа** в поле пароль вставьте парольную фразу, полученную при создании сертификата.  Опять же, в данном случае это **_15105661266553327_** .</span><span class="sxs-lookup"><span data-stu-id="944ea-144">At the **Private Key Protection** step, in the Password box, paste the passphrase you received from creating the cert.  Again, in this case it is **_15105661266553327_**.</span></span>

      ![Копирование парольной фразы](media/custom-visual-develop-tutorial/cert-install-wizard-show-passphrase.png)

5. <span data-ttu-id="944ea-146">На этапе **Хранилище сертификатов** выберите вариант **Поместить все сертификаты в следующее хранилище**.</span><span class="sxs-lookup"><span data-stu-id="944ea-146">At the **Certificate Store** step, select the **Place all certificates in the Following store** option.</span></span> <span data-ttu-id="944ea-147">Затем нажмите кнопку *Обзор*.</span><span class="sxs-lookup"><span data-stu-id="944ea-147">Then select *Browse*.</span></span>

      ![Все сертификаты в следующее хранилище](media/custom-visual-develop-tutorial/all-certs-in-the-following-store.png)

6. <span data-ttu-id="944ea-149">В окне **Выбор хранилища сертификата** выберите вариант **Доверенные корневые центры сертификации** и щелкните *ОК*.</span><span class="sxs-lookup"><span data-stu-id="944ea-149">In the **Select Certificate Store** window, select **Trusted Root Certification Authorities** and then select *OK*.</span></span> <span data-ttu-id="944ea-150">Затем на экране *Хранилище сертификатов* нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="944ea-150">Then select *Next* on the **Certificate Store** screen.</span></span>

      ![Доверенный корневой сертификат](media/custom-visual-develop-tutorial/trusted-root-cert.png)

7. <span data-ttu-id="944ea-152">Чтобы завершить импорт, щелкните **Готово**.</span><span class="sxs-lookup"><span data-stu-id="944ea-152">To complete the import, select **Finish**.</span></span>

8. <span data-ttu-id="944ea-153">Если появится предупреждение системы безопасности, выберите **Да**.</span><span class="sxs-lookup"><span data-stu-id="944ea-153">If you receive a security warning, select **Yes**.</span></span>

    ![Предупреждение системы безопасности](media/custom-visual-develop-tutorial/cert-security-warning.png)

9. <span data-ttu-id="944ea-155">Получив уведомление об успешном выполнении импорта, щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="944ea-155">When notified that the import was successful, select **OK**.</span></span>

    ![Импорт сертификата успешно завершен](media/custom-visual-develop-tutorial/cert-import-successful.png)

> [!Important]
> <span data-ttu-id="944ea-157">Не закрывайте сеанс Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="944ea-157">Do not close the Windows PowerShell session.</span></span>

#### <a name="osx"></a><span data-ttu-id="944ea-158">OSX</span><span class="sxs-lookup"><span data-stu-id="944ea-158">OSX</span></span>

1. <span data-ttu-id="944ea-159">Если в верхнем левом углу установлена блокировка, снимите ее.</span><span class="sxs-lookup"><span data-stu-id="944ea-159">If the lock in the upper left is locked, select it to unlock.</span></span> <span data-ttu-id="944ea-160">Найдите *localhost* и дважды щелкните сертификат.</span><span class="sxs-lookup"><span data-stu-id="944ea-160">Search for *localhost* and double-click on the certificate.</span></span>

    ![Установка SSL-сертификата 1 на OSX](media/custom-visual-develop-tutorial/install-ssl-certificate-osx.png)

2. <span data-ttu-id="944ea-162">Выберите пункт **Always Trust** (Всегда доверять) и закройте окно.</span><span class="sxs-lookup"><span data-stu-id="944ea-162">Select **Always Trust** and close the window.</span></span>

    ![Установка SSL-сертификата 2 на OSX](media/custom-visual-develop-tutorial/install-ssl-certificate-osx2.png)

3. <span data-ttu-id="944ea-164">Введите имя пользователя и пароль.</span><span class="sxs-lookup"><span data-stu-id="944ea-164">Enter your username and password.</span></span> <span data-ttu-id="944ea-165">Нажмите кнопку **Обновить параметры**.</span><span class="sxs-lookup"><span data-stu-id="944ea-165">Select **Update Settings**.</span></span>

    ![Установка SSL-сертификата 3 на OSX](media/custom-visual-develop-tutorial/install-ssl-certificate-osx3.png)

4. <span data-ttu-id="944ea-167">Закройте все открытые браузеры.</span><span class="sxs-lookup"><span data-stu-id="944ea-167">Close any browsers that you have open.</span></span>

> [!NOTE]
> <span data-ttu-id="944ea-168">Если сертификат не удается распознать, может потребоваться перезагрузить компьютер.</span><span class="sxs-lookup"><span data-stu-id="944ea-168">If the certificate is not recognized, you may need to restart your computer.</span></span>

## <a name="creating-a-custom-visual"></a><span data-ttu-id="944ea-169">Создание пользовательского визуального элемента</span><span class="sxs-lookup"><span data-stu-id="944ea-169">Creating a custom visual</span></span>

<span data-ttu-id="944ea-170">Итак, мы настроили среду и теперь можем приступать к созданию пользовательского визуального элемента.</span><span class="sxs-lookup"><span data-stu-id="944ea-170">Now that you have set up your environment, it is time to create your custom visual.</span></span>

<span data-ttu-id="944ea-171">Полный исходный код для этого руководства доступен для [скачивания](https://github.com/Microsoft/PowerBI-visuals-circlecard).</span><span class="sxs-lookup"><span data-stu-id="944ea-171">You can [download](https://github.com/Microsoft/PowerBI-visuals-circlecard) the full source code for this tutorial.</span></span>

1. <span data-ttu-id="944ea-172">Проверьте, что установлен пакет визуальных средств Power BI.</span><span class="sxs-lookup"><span data-stu-id="944ea-172">Verify that the Power BI Visual Tools package has been installed.</span></span>

    ```powershell
    pbiviz
    ```
    <span data-ttu-id="944ea-173">Будут выведены данные справки.</span><span class="sxs-lookup"><span data-stu-id="944ea-173">You should see the help output.</span></span>

    <pre><code>
        +syyso+/
    oms/+osyhdhyso/
    ym/       /+oshddhys+/
    ym/              /+oyhddhyo+/
    ym/                     /osyhdho
    ym/                           sm+
    ym/               yddy        om+
    ym/         shho /mmmm/       om+
        /    oys/ +mmmm /mmmm/       om+
    oso  ommmh +mmmm /mmmm/       om+
    ymmmy smmmh +mmmm /mmmm/       om+
    ymmmy smmmh +mmmm /mmmm/       om+
    ymmmy smmmh +mmmm /mmmm/       om+
    +dmd+ smmmh +mmmm /mmmm/       om+
            /hmdo +mmmm /mmmm/ /so+//ym/
                /dmmh /mmmm/ /osyhhy/
                    //   dmmd
                        ++

        PowerBI Custom Visual Tool

    Usage: pbiviz [options] [command]

    Commands:

    new [name]        Create a new visual
    info              Display info about the current visual
    start             Start the current visual
    package           Package the current visual into a pbiviz file
    update [version]  Updates the api definitions and schemas in the current visual. Changes the version if specified
    help [cmd]        display help for [cmd]

    Options:

    -h, --help      output usage information
    -V, --version   output the version number
    --install-cert  Install localhost certificate
    </code></pre>

    <a name="ssl-setup"></a>

2. <span data-ttu-id="944ea-174">Просмотрите выходные данные, включая список поддерживаемых команд.</span><span class="sxs-lookup"><span data-stu-id="944ea-174">Review the output, including the list of supported commands.</span></span>

    ![Поддерживаемые команды](media/custom-visual-develop-tutorial/PowerShell-supported-commands.png)

3. <span data-ttu-id="944ea-176">Чтобы создать проект пользовательского визуального элемента, введите следующую команду.</span><span class="sxs-lookup"><span data-stu-id="944ea-176">To create a custom visual project, enter the following command.</span></span> <span data-ttu-id="944ea-177">Этот проект имеет имя **CircleCard**.</span><span class="sxs-lookup"><span data-stu-id="944ea-177">**CircleCard** is the name of the project.</span></span>

    ```PowerShell
    pbiviz new CircleCard
    ```
    ![Результат создания CircleCard](media/custom-visual-develop-tutorial/new-circle-card-result.png)

    > [!Note]
    > <span data-ttu-id="944ea-179">Вы создаете проект в текущем расположении, указанном в запросе.</span><span class="sxs-lookup"><span data-stu-id="944ea-179">You create the new project at the current location of the prompt.</span></span>

4. <span data-ttu-id="944ea-180">Перейдите в папку проекта.</span><span class="sxs-lookup"><span data-stu-id="944ea-180">Navigate to the project folder.</span></span>

    ```powershell
    cd CircleCard
    ```
5. <span data-ttu-id="944ea-181">Запустите пользовательский визуальный элемент.</span><span class="sxs-lookup"><span data-stu-id="944ea-181">Start the custom visual.</span></span> <span data-ttu-id="944ea-182">Теперь визуальный элемент CircleCard запущен. Он размещен на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="944ea-182">Your CircleCard visual is now running while being hosted on your computer.</span></span>

    ```powershell
    pbiviz start
    ```

    ![Запуск пользовательского визуального элемента](media/custom-visual-develop-tutorial/start-running-custom-visual-PowerShell.png)

> [!Important]
> <span data-ttu-id="944ea-184">Не закрывайте сеанс Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="944ea-184">Do not close the Windows PowerShell session.</span></span>

### <a name="testing-the-custom-visual"></a><span data-ttu-id="944ea-185">Тестирование пользовательского визуального элемента</span><span class="sxs-lookup"><span data-stu-id="944ea-185">Testing the custom visual</span></span>

<span data-ttu-id="944ea-186">В этом разделе мы будем тестировать пользовательский визуальный элемент CircleCard, отправив отчет в Power BI Desktop и добавив в него наш пользовательский визуальный элемент.</span><span class="sxs-lookup"><span data-stu-id="944ea-186">In this section, we are going to test the CircleCard custom visual by uploading a Power BI Desktop report and then editing the report to display the custom visual.</span></span>

1. <span data-ttu-id="944ea-187">Войдите на сайт [PowerBI.com](https://powerbi.microsoft.com/), затем нажмите **значок шестеренки** и выберите элемент **Настройки**.</span><span class="sxs-lookup"><span data-stu-id="944ea-187">Sign in to [PowerBI.com](https://powerbi.microsoft.com/) > go to the **Gear icon** > then select **Settings**.</span></span>

      ![Настройки Power BI](media/custom-visual-develop-tutorial/power-bi-settings.png)

2. <span data-ttu-id="944ea-189">Выберите пункт **Разработчик** и установите флажок **Включить тестирование для визуального элемента разработчика**.</span><span class="sxs-lookup"><span data-stu-id="944ea-189">Select **Developer** then check the **Enable Developer Visual for testing** checkbox.</span></span>

    ![Параметры на странице для разработчиков](media/custom-visual-develop-tutorial/developer-page-settings.png)

3. <span data-ttu-id="944ea-191">Передайте отчет Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="944ea-191">Upload a Power BI Desktop report.</span></span>  

    <span data-ttu-id="944ea-192">Последовательно щелкните "Получить данные" > "Файлы" > "Локальный файл".</span><span class="sxs-lookup"><span data-stu-id="944ea-192">Get Data > Files > Local File.</span></span>

    <span data-ttu-id="944ea-193">Если вы еще не создали отчет Power BI Desktop, [скачайте](https://microsoft.github.io/PowerBI-visuals/docs/step-by-step-lab/images/US_Sales_Analysis.pbix) образец.</span><span class="sxs-lookup"><span data-stu-id="944ea-193">You can [download](https://microsoft.github.io/PowerBI-visuals/docs/step-by-step-lab/images/US_Sales_Analysis.pbix) a sample Power BI Desktop report if you do not have one created already.</span></span>

    <span data-ttu-id="944ea-194">![Получить данные](media/custom-visual-develop-tutorial/get-data.png) ![Локальный файл](media/custom-visual-develop-tutorial/local-file.png)</span><span class="sxs-lookup"><span data-stu-id="944ea-194">![Get Data](media/custom-visual-develop-tutorial/get-data.png) ![Local File](media/custom-visual-develop-tutorial/local-file.png)</span></span>

    <span data-ttu-id="944ea-195">Теперь в области навигации слева в разделе **Отчет** выберите **US_Sales_Analysis**, чтобы просмотреть этот отчет.</span><span class="sxs-lookup"><span data-stu-id="944ea-195">Now to view the report, select **US_Sales_Analysis** from the **Report** section in the navigation pane on the left.</span></span>

    ![Пример пользовательского визуального элемента в Power BI Desktop](media/custom-visual-develop-tutorial/custom-visual-sample.png)

4. <span data-ttu-id="944ea-197">Теперь нам нужно изменить отчет в службе Power BI.</span><span class="sxs-lookup"><span data-stu-id="944ea-197">Now you need to edit the report while in the Power BI service.</span></span>

    <span data-ttu-id="944ea-198">Выберите **Изменить отчет**.</span><span class="sxs-lookup"><span data-stu-id="944ea-198">Go to **Edit report**.</span></span>

    ![Изменить отчет](media/custom-visual-develop-tutorial/edit-report.png)

5. <span data-ttu-id="944ea-200">На панели **Визуализации** выберите **Визуальный элемент разработчика**.</span><span class="sxs-lookup"><span data-stu-id="944ea-200">Select the **Developer Visual** from the **Visualizations** pane.</span></span>

    ![Визуальный элемент разработчика](media/custom-visual-develop-tutorial/developer-visual.png)

    > [!Note]
    > <span data-ttu-id="944ea-202">Этот элемент представляет пользовательский визуальный элемент, который вы ранее запустили на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="944ea-202">This visualization represents the custom visual that you started on your computer.</span></span> <span data-ttu-id="944ea-203">Он станет доступен только после активации параметров разработчика.</span><span class="sxs-lookup"><span data-stu-id="944ea-203">It is only available when the developer settings have been enabled.</span></span>

6. <span data-ttu-id="944ea-204">Обратите внимание, что наш визуальный элемент появился на холсте отчета.</span><span class="sxs-lookup"><span data-stu-id="944ea-204">Notice that a visualization was added to the report canvas.</span></span>

    ![Новый визуальный элемент](media/custom-visual-develop-tutorial/new-visual-in-report.png)

    > [!Note]
    > <span data-ttu-id="944ea-206">Это очень простой визуальный элемент, который отображает число вызовов метода update.</span><span class="sxs-lookup"><span data-stu-id="944ea-206">This is a very simple visual that displays the number of times its Update method has been called.</span></span> <span data-ttu-id="944ea-207">На этом этапе визуальный элемент еще не может извлекать данные.</span><span class="sxs-lookup"><span data-stu-id="944ea-207">At this stage, the visual does not yet retrieve any data.</span></span>

7. <span data-ttu-id="944ea-208">Выделите в отчете новый визуальный элемент, перейдите на панель "Поля", разверните элемент Sales (Продажи) и выберите Quantity (Количество).</span><span class="sxs-lookup"><span data-stu-id="944ea-208">While selecting the new visual in the report, Go to the Fields Pane > expand Sales > select Quantity.</span></span>

    ![Количество продаж](media/custom-visual-develop-tutorial/quantity-sales.png)

8. <span data-ttu-id="944ea-210">Для проверки работы нового визуального элемента измените его размер и убедитесь, что значение update увеличивается.</span><span class="sxs-lookup"><span data-stu-id="944ea-210">Then to test the new visual, resize the visual and notice the update value increments.</span></span>

    ![Изменение размеров визуального элемента](media/custom-visual-develop-tutorial/resize-visual.png)

<span data-ttu-id="944ea-212">Чтобы остановить работу пользовательского визуального элемента в PowerShell, нажмите клавиши CTRL+C.</span><span class="sxs-lookup"><span data-stu-id="944ea-212">To stop the custom visual running in PowerShell, enter Ctrl+C.</span></span> <span data-ttu-id="944ea-213">В ответ на запрос о завершении пакетного задания последовательно нажмите клавиши Y и ВВОД.</span><span class="sxs-lookup"><span data-stu-id="944ea-213">When prompted to terminate the batch job, enter Y, then press Enter.</span></span>

## <a name="adding-visual-elements"></a><span data-ttu-id="944ea-214">Добавление визуальных элементов</span><span class="sxs-lookup"><span data-stu-id="944ea-214">Adding visual elements</span></span>

<span data-ttu-id="944ea-215">Теперь вам нужно установить **библиотеку D3 JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="944ea-215">Now you need to install the **D3 JavaScript library**.</span></span> <span data-ttu-id="944ea-216">Библиотека D3 для JavaScript позволяет создавать динамические интерактивные визуализации данных в веб-браузерах.</span><span class="sxs-lookup"><span data-stu-id="944ea-216">D3 is a JavaScript library for producing dynamic, interactive data visualizations in web browsers.</span></span> <span data-ttu-id="944ea-217">В ней используются широко распространенные стандарты SVG HTML5 и CSS.</span><span class="sxs-lookup"><span data-stu-id="944ea-217">It makes use of widely implemented SVG HTML5, and CSS standards.</span></span>

<span data-ttu-id="944ea-218">Теперь вы можете разработать пользовательский визуальный элемент, отображающий круг с текстом.</span><span class="sxs-lookup"><span data-stu-id="944ea-218">Now you can develop the custom visual to display a circle with text.</span></span>

> [!Note]
> <span data-ttu-id="944ea-219">Многие фрагменты текста для этого руководства можно скопировать [отсюда](https://github.com/Microsoft/powerbi-visuals-circlecard).</span><span class="sxs-lookup"><span data-stu-id="944ea-219">Many text entries in this tutorial can be copied from [here](https://github.com/Microsoft/powerbi-visuals-circlecard).</span></span>

1. <span data-ttu-id="944ea-220">Чтобы установить **библиотеку D3**, выполните в PowerShell следующую команду:</span><span class="sxs-lookup"><span data-stu-id="944ea-220">To install the **D3 library** in PowerShell, enter the command below.</span></span>

    ```powershell
    npm i d3@^5.0.0 --save
    ```

    ```powershell
    PS C:\circlecard>npm i d3@^5.0.0 --save
    + d3@5.11.0
    added 179 packages from 169 contributors and audited 306 packages in 33.25s
    found 0 vulnerabilities

    PS C:\circlecard>
    ```

2. <span data-ttu-id="944ea-221">Чтобы установить определения типов для **библиотеки D3**, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="944ea-221">To install type definitions for the **D3 library**, enter the command below.</span></span>

    ```powershell
    npm i @types/d3@^5.0.0 --save
    ```

    ```powershell
    PS C:\circlecard>npm i @types/d3@^5.0.0 --save
    + @types/d3@5.7.2
    updated 1 package and audited 306 packages in 2.217s
    found 0 vulnerabilities

    PS C:\circlecard>
    ```

    <span data-ttu-id="944ea-222">Эта команда устанавливает определения TypeScript, основанные на файлах JavaScript, что позволяет разрабатывать пользовательские визуальные элементы на TypeScript (супермножество JavaScript).</span><span class="sxs-lookup"><span data-stu-id="944ea-222">This command installs TypeScript definitions based on JavaScript files, enabling you to develop the custom visual in TypeScript (which is a superset of JavaScript).</span></span> <span data-ttu-id="944ea-223">Для разработки приложений TypeScript идеально подходит интегрированная среда разработки Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="944ea-223">Visual Studio Code is an ideal IDE for developing TypeScript applications.</span></span>

3. <span data-ttu-id="944ea-224">Чтобы установить **core-js**, выполните в PowerShell следующую команду:</span><span class="sxs-lookup"><span data-stu-id="944ea-224">To install the **core-js** in PowerShell, enter the command below.</span></span>

    ```powershell
    npm i core-js@3.2.1 --save
    ```

    ```powershell
    PS C:\circlecard> npm i core-js@3.2.1 --save

    > core-js@3.2.1 postinstall F:\circlecard\node_modules\core-js
    > node scripts/postinstall || echo "ignore"

    Thank you for using core-js ( https://github.com/zloirock/core-js ) for polyfilling JavaScript standard library!

    The project needs your help! Please consider supporting of core-js on Open Collective or Patreon:
    > https://opencollective.com/core-js
    > https://www.patreon.com/zloirock

    + core-js@3.2.1
    updated 1 package and audited 306 packages in 6.051s
    found 0 vulnerabilities

    PS C:\circlecard>
    ```

    <span data-ttu-id="944ea-225">Эта команда устанавливает стандартную модульную библиотеку для JavaScript.</span><span class="sxs-lookup"><span data-stu-id="944ea-225">This command installs modular standard library for JavaScript.</span></span> <span data-ttu-id="944ea-226">В библиотеке включены полизаполнения для ECMAScript до версии 2019.</span><span class="sxs-lookup"><span data-stu-id="944ea-226">It includes polyfills for ECMAScript up to 2019.</span></span> <span data-ttu-id="944ea-227">См. дополнительные сведения о [`core-js`](https://www.npmjs.com/package/core-js).</span><span class="sxs-lookup"><span data-stu-id="944ea-227">Read more about [`core-js`](https://www.npmjs.com/package/core-js)</span></span>

4. <span data-ttu-id="944ea-228">Чтобы установить **powerbi-visual-api**, выполните в PowerShell следующую команду:</span><span class="sxs-lookup"><span data-stu-id="944ea-228">To install the **powerbi-visual-api** in PowerShell, enter the command below.</span></span>

    ```powershell
    npm i powerbi-visuals-api --save-dev
    ```

    ```powershell
    PS C:\circlecard>npm i powerbi-visuals-api --save-dev

    + powerbi-visuals-api@2.6.1
    updated 1 package and audited 306 packages in 2.139s
    found 0 vulnerabilities

    PS C:\circlecard>
    ```

    <span data-ttu-id="944ea-229">Эта команда устанавливает определения API визуальных элементов Power BI.</span><span class="sxs-lookup"><span data-stu-id="944ea-229">This command installs Power BI Visuals API definitions.</span></span>

5. <span data-ttu-id="944ea-230">Запустите [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="944ea-230">Launch [Visual Studio Code](https://code.visualstudio.com/).</span></span>

    <span data-ttu-id="944ea-231">Чтобы запустить **Visual Studio Code** из PowerShell, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="944ea-231">You can launch **Visual Studio Code** from PowerShell by using the following command.</span></span>

    ```powershell
    code .
    ```

6. <span data-ttu-id="944ea-232">В **области обозревателя** разверните папку **node_modules** и убедитесь, что **библиотека D3** успешно установлена.</span><span class="sxs-lookup"><span data-stu-id="944ea-232">In the **Explorer pane**, expand the **node_modules** folder to verify that the **d3 library** was installed.</span></span>

    ![Библиотека D3 в Visual Studio Code](media/custom-visual-develop-tutorial/d3-library.png)

7. <span data-ttu-id="944ea-234">Убедитесь, что файл **index.d.ts** добавлен, развернув в @typesобласти обозревателя**узлы node_modules >**  > d3.</span><span class="sxs-lookup"><span data-stu-id="944ea-234">Make sure that file **index.d.ts** was added, by expanding node_modules > @types > d3 in the **Explorer pane**.</span></span>

    ![Файл Index.d.ts](media/custom-visual-develop-tutorial/index-d-ts.png)

### <a name="developing-the-visual-elements"></a><span data-ttu-id="944ea-236">Разработка визуальных элементов</span><span class="sxs-lookup"><span data-stu-id="944ea-236">Developing the visual elements</span></span>

<span data-ttu-id="944ea-237">Теперь мы готовы перейти к разработке пользовательского визуального элемента, который будет отображать круг и текст.</span><span class="sxs-lookup"><span data-stu-id="944ea-237">Now we can explore how to develop the custom visual to show a circle and sample text.</span></span>

1. <span data-ttu-id="944ea-238">В **области обозревателя** разверните папку **src** и выберите файл **settings.ts**.</span><span class="sxs-lookup"><span data-stu-id="944ea-238">In the **Explorer pane**, expand the **src** folder and then select **visual.ts**.</span></span>

    > [!Note]
    > <span data-ttu-id="944ea-239">Обратите внимание на комментарии в верхней части файла **visual.ts**.</span><span class="sxs-lookup"><span data-stu-id="944ea-239">Notice the comments at the top of the **visual.ts** file.</span></span> <span data-ttu-id="944ea-240">Разрешение на использование пакетов пользовательских визуальных элементов Power BI предоставляется бесплатно на условиях лицензии MIT.</span><span class="sxs-lookup"><span data-stu-id="944ea-240">Permission to use the Power BI custom visual packages is granted free of charge under the terms of the MIT License.</span></span> <span data-ttu-id="944ea-241">Это соглашение содержит обязательство добавлять комментарии в верхней части файла.</span><span class="sxs-lookup"><span data-stu-id="944ea-241">As part of the agreement, you must leave the comments at the top of the file.</span></span>

2. <span data-ttu-id="944ea-242">Удалите следующую логику пользовательского визуального элемента из класса Visual:</span><span class="sxs-lookup"><span data-stu-id="944ea-242">Remove the following default custom visual logic from the Visual class.</span></span>
    * <span data-ttu-id="944ea-243">объявление четырех закрытых переменных на уровне класса;</span><span class="sxs-lookup"><span data-stu-id="944ea-243">The four class-level private variable declarations.</span></span>
    * <span data-ttu-id="944ea-244">все строки кода из конструктора;</span><span class="sxs-lookup"><span data-stu-id="944ea-244">All lines of code from the constructor.</span></span>
    * <span data-ttu-id="944ea-245">все строки кода из метода update;</span><span class="sxs-lookup"><span data-stu-id="944ea-245">All lines of code from the update method.</span></span>
    * <span data-ttu-id="944ea-246">все оставшиеся строки в модуле, включая методы parseSettings и enumerateObjectInstances.</span><span class="sxs-lookup"><span data-stu-id="944ea-246">All remaining lines within the module, including the parseSettings and enumerateObjectInstances methods.</span></span>

    <span data-ttu-id="944ea-247">Убедитесь, что код модуля теперь выглядит так:</span><span class="sxs-lookup"><span data-stu-id="944ea-247">Verify that the module code looks like the following.</span></span>

    ```typescript
    "use strict";
    import "core-js/stable";
    import "../style/visual.less";
    import powerbi from "powerbi-visuals-api";
    import IVisual = powerbi.extensibility.IVisual;
    import VisualConstructorOptions = powerbi.extensibility.visual.VisualConstructorOptions;
    import VisualUpdateOptions = powerbi.extensibility.visual.VisualUpdateOptions;

    import * as d3 from "d3";
    type Selection<T extends d3.BaseType> = d3.Selection<T, any,any, any>;

    export class Visual implements IVisual {

        constructor(options: VisualConstructorOptions) {

        }

        public update(options: VisualUpdateOptions) {

        }
    }
    ```

3. <span data-ttu-id="944ea-248">Под объявлением класса *Visual* вставьте следующие свойства уровня класса:</span><span class="sxs-lookup"><span data-stu-id="944ea-248">Beneath the *Visual* class declaration, insert the following class-level properties.</span></span>

    ```typescript
    export class Visual implements IVisual {
        // ...
        private host: IVisualHost;
        private svg: Selection<SVGElement>;
        private container: Selection<SVGElement>;
        private circle: Selection<SVGElement>;
        private textValue: Selection<SVGElement>;
        private textLabel: Selection<SVGElement>;
        // ...
    }
    ```

    ![Свойства уровня класса в файле visual.ts](media/custom-visual-develop-tutorial/visual-ts-file-class-level-properties.png)

4. <span data-ttu-id="944ea-250">Добавьте в *конструктор* следующий код:</span><span class="sxs-lookup"><span data-stu-id="944ea-250">Add the following code to the *constructor*.</span></span>

    ```typescript
    this.svg = d3.select(options.element)
        .append('svg')
        .classed('circleCard', true);
    this.container = this.svg.append("g")
        .classed('container', true);
    this.circle = this.container.append("circle")
        .classed('circle', true);
    this.textValue = this.container.append("text")
        .classed("textValue", true);
    this.textLabel = this.container.append("text")
        .classed("textLabel", true);
    ```

    <span data-ttu-id="944ea-251">Этот код добавляет в визуальный элемент группу SVG и три фигуры: круг и два текстовых элемента.</span><span class="sxs-lookup"><span data-stu-id="944ea-251">This code adds an SVG group inside the visual and then adds three shapes: a circle and two text elements.</span></span>

    <span data-ttu-id="944ea-252">Чтобы отформатировать этот код в документе, щелкните правой кнопкой мыши в любом месте **документа Visual Studio Code** и выберите действие **Форматировать документ**.</span><span class="sxs-lookup"><span data-stu-id="944ea-252">To format the code in the document, right-select anywhere in the **Visual Studio Code document**, and then select **Format Document**.</span></span>

      ![Форматировать документ](media/custom-visual-develop-tutorial/format-document.png)

    <span data-ttu-id="944ea-254">Чтобы повысить удобочитаемость, документ рекомендуется форматировать каждый раз после добавления фрагментов кода.</span><span class="sxs-lookup"><span data-stu-id="944ea-254">To improve readability, it is recommended that you format the document every time that paste in code snippets.</span></span>

5. <span data-ttu-id="944ea-255">Добавьте в метод *update* следующий код:</span><span class="sxs-lookup"><span data-stu-id="944ea-255">Add the following code to the *update* method.</span></span>

    ```typescript
    let width: number = options.viewport.width;
    let height: number = options.viewport.height;
    this.svg.attr("width", width);
    this.svg.attr("height", height);
    let radius: number = Math.min(width, height) / 2.2;
    this.circle
        .style("fill", "white")
        .style("fill-opacity", 0.5)
        .style("stroke", "black")
        .style("stroke-width", 2)
        .attr("r", radius)
        .attr("cx", width / 2)
        .attr("cy", height / 2);
    let fontSizeValue: number = Math.min(width, height) / 5;
    this.textValue
        .text("Value")
        .attr("x", "50%")
        .attr("y", "50%")
        .attr("dy", "0.35em")
        .attr("text-anchor", "middle")
        .style("font-size", fontSizeValue + "px");
    let fontSizeLabel: number = fontSizeValue / 4;
    this.textLabel
        .text("Label")
        .attr("x", "50%")
        .attr("y", height / 2)
        .attr("dy", fontSizeValue / 1.2)
        .attr("text-anchor", "middle")
        .style("font-size", fontSizeLabel + "px");
    ```

    <span data-ttu-id="944ea-256">*Этот код задает ширину и высоту визуального элемента, а затем инициализирует атрибуты и стили для визуальных элементов.*</span><span class="sxs-lookup"><span data-stu-id="944ea-256">*This code sets the width and height of the visual, and then initializes the attributes and styles of the visual elements.*</span></span>

6. <span data-ttu-id="944ea-257">Сохраните файл **visual.ts**.</span><span class="sxs-lookup"><span data-stu-id="944ea-257">Save the **visual.ts** file.</span></span>

7. <span data-ttu-id="944ea-258">Выберите файл **capabilities.json**.</span><span class="sxs-lookup"><span data-stu-id="944ea-258">Select the **capabilities.json** file.</span></span>

    <span data-ttu-id="944ea-259">Удалите весь элемент объекта, размещенный в строках с 14 по 60.</span><span class="sxs-lookup"><span data-stu-id="944ea-259">At line 14, remove the entire objects element (lines 14-60).</span></span>

8. <span data-ttu-id="944ea-260">Сохраните файл **capabilities.json**.</span><span class="sxs-lookup"><span data-stu-id="944ea-260">Save the **capabilities.json** file.</span></span>

9. <span data-ttu-id="944ea-261">Запустите пользовательский визуальный элемент в PowerShell.</span><span class="sxs-lookup"><span data-stu-id="944ea-261">In PowerShell, start the custom visual.</span></span>

    ```powershell
    pbiviz start
    ```

### <a name="toggle-auto-reload"></a><span data-ttu-id="944ea-262">Включить автоматическую перезагрузку</span><span class="sxs-lookup"><span data-stu-id="944ea-262">Toggle auto reload</span></span>

1. <span data-ttu-id="944ea-263">Перейдите обратно к отчету Power BI.</span><span class="sxs-lookup"><span data-stu-id="944ea-263">Navigate back to the Power BI report.</span></span>
2. <span data-ttu-id="944ea-264">На панели инструментов над визуальным элементом разработчика выберите **Включить автоматическую перезагрузку**.</span><span class="sxs-lookup"><span data-stu-id="944ea-264">In the toolbar floating above the developer visual, select the **Toggle Auto Reload**.</span></span>

    ![Включить автоматическую перезагрузку](media/custom-visual-develop-tutorial/toggle-auto-reload.png)

    <span data-ttu-id="944ea-266">Этот параметр отвечает за автоматическую перезагрузку визуального элемента после каждого сохранения изменений в проекте.</span><span class="sxs-lookup"><span data-stu-id="944ea-266">This option ensures that the visual is automatically reloaded each time you save project changes.</span></span>

3. <span data-ttu-id="944ea-267">Из области **Поля** перетащите поле **Quantity** (Количество) в визуальный элемент разработчика.</span><span class="sxs-lookup"><span data-stu-id="944ea-267">From the **Fields pane**, drag the **Quantity** field into the developer visual.</span></span>

4. <span data-ttu-id="944ea-268">Убедитесь, что код визуального элемента теперь выглядит так:</span><span class="sxs-lookup"><span data-stu-id="944ea-268">Verify that the visual looks like the following.</span></span>

    ![Визуальный элемент разработчика в виде круга](media/custom-visual-develop-tutorial/circle-developer-visual.png)

5. <span data-ttu-id="944ea-270">Измените размер визуального элемента.</span><span class="sxs-lookup"><span data-stu-id="944ea-270">Resize the visual.</span></span>

    <span data-ttu-id="944ea-271">Обратите внимание, что круг и текст в нем масштабируются в соответствии с размером визуального элемента.</span><span class="sxs-lookup"><span data-stu-id="944ea-271">Notice that the circle and text value scales to fit the available dimension of the visual.</span></span>

    <span data-ttu-id="944ea-272">При изменении размера визуального элемента метод update вызывается постоянно, что приводит к плавному масштабированию визуальных элементов.</span><span class="sxs-lookup"><span data-stu-id="944ea-272">The update method is called continuously with resizing the visual, and it results in the fluid rescaling of the visual elements.</span></span>

    <span data-ttu-id="944ea-273">Итак, вы разработали визуальный элемент.</span><span class="sxs-lookup"><span data-stu-id="944ea-273">You have now developed the visual elements.</span></span>

6. <span data-ttu-id="944ea-274">Оставьте визуальный элемент в работающем состоянии.</span><span class="sxs-lookup"><span data-stu-id="944ea-274">Continue running the visual.</span></span>

## <a name="process-data-in-the-visual-code"></a><span data-ttu-id="944ea-275">Обработка данных в коде визуального элемента</span><span class="sxs-lookup"><span data-stu-id="944ea-275">Process data in the visual code</span></span>

<span data-ttu-id="944ea-276">Определите роли данных и сопоставления представлений данных, а затем измените логику пользовательского визуального элемента, чтобы он отображал имя и значение меры.</span><span class="sxs-lookup"><span data-stu-id="944ea-276">Define the data roles and data view mappings, and then modify the custom visual logic to display the value and display name of a measure.</span></span>

### <a name="configuring-the-capabilities"></a><span data-ttu-id="944ea-277">Настройка возможностей</span><span class="sxs-lookup"><span data-stu-id="944ea-277">Configuring the capabilities</span></span>

<span data-ttu-id="944ea-278">В файле **capabilities.json** определите роли данных и сопоставления представлений данных.</span><span class="sxs-lookup"><span data-stu-id="944ea-278">Modify the **capabilities.json** file to define the data role and data view mappings.</span></span>

1. <span data-ttu-id="944ea-279">В Visual Studio Code откройте файл **capabilities.json** и удалите из массива **dataRoles** все его содержимое (строки 3—12).</span><span class="sxs-lookup"><span data-stu-id="944ea-279">In Visual Studio code, in the **capabilities.json** file, from inside the **dataRoles** array, remove all content (lines 3-12).</span></span>

2. <span data-ttu-id="944ea-280">Вставьте в массив **dataRoles** следующий код:</span><span class="sxs-lookup"><span data-stu-id="944ea-280">Inside the **dataRoles** array, insert the following code.</span></span>

    ```json
    {
        "displayName": "Measure",
        "name": "measure",
        "kind": "Measure"
    }
    ```

    <span data-ttu-id="944ea-281">Массив **dataRoles** теперь определяет одну роли данных с типом **measure** и именем **measure**, которая отображает значение **Measure**.</span><span class="sxs-lookup"><span data-stu-id="944ea-281">The **dataRoles** array now defines a single data role of type **measure**, that is named **measure**, and displays as **Measure**.</span></span> <span data-ttu-id="944ea-282">Эта роль позволяет передавать значение из поля меры или поля сводных данных.</span><span class="sxs-lookup"><span data-stu-id="944ea-282">This data role allows passing either a measure field, or a field that is summarized.</span></span>

3. <span data-ttu-id="944ea-283">Удалите из массива **dataViewMappings** все его содержимое (строки 10—31).</span><span class="sxs-lookup"><span data-stu-id="944ea-283">From inside the **dataViewMappings** array, remove all content (lines 10-31).</span></span>

4. <span data-ttu-id="944ea-284">Вставьте в массив **dataViewMappings** следующее содержимое:</span><span class="sxs-lookup"><span data-stu-id="944ea-284">Inside the **dataViewMappings** array, insert the following content.</span></span>

    ```json
    {
        "conditions": [
            { "measure": { "max": 1 } }
        ],
        "single": {
            "role": "measure"
        }
    }
    ```

    <span data-ttu-id="944ea-285">Теперь массив **dataViewMappings** определяет одно поле, которое можно передавать в роль данных с именем **measure**.</span><span class="sxs-lookup"><span data-stu-id="944ea-285">The **dataViewMappings** array now defines one field can be passed to the data role named **measure**.</span></span>

5. <span data-ttu-id="944ea-286">Сохраните файл **capabilities.json**.</span><span class="sxs-lookup"><span data-stu-id="944ea-286">Save the **capabilities.json** file.</span></span>

6. <span data-ttu-id="944ea-287">В Power BI вы можете заметить, что для визуального элемента теперь можно настроить элемент **Measure**.</span><span class="sxs-lookup"><span data-stu-id="944ea-287">In Power BI, notice that the visual now can be configured with **Measure**.</span></span>

    ![Количество и мера](media/custom-visual-develop-tutorial/quantity_measure.png)

    > [!Note]
    > <span data-ttu-id="944ea-289">В нашем проекте визуального элемента пока нет логики привязки данных.</span><span class="sxs-lookup"><span data-stu-id="944ea-289">The visual project does not yet include data binding logic.</span></span>

### <a name="exploring-the-dataview"></a><span data-ttu-id="944ea-290">Исследование представления данных</span><span class="sxs-lookup"><span data-stu-id="944ea-290">Exploring the dataview</span></span>

1. <span data-ttu-id="944ea-291">На панели инструментов над визуальным элементом выберите **Показать представление данных**.</span><span class="sxs-lookup"><span data-stu-id="944ea-291">In the toolbar floating above the visual, select **Show Dataview**.</span></span>

    ![Показать представление данных](media/custom-visual-develop-tutorial/show-dataview-toolbar.png)

2. <span data-ttu-id="944ea-293">Разверните представление вниз до узла **single** и обратите внимание на значение.</span><span class="sxs-lookup"><span data-stu-id="944ea-293">Expand down into **single**, and then notice the value.</span></span>

    ![Развертывание до нужного значения](media/custom-visual-develop-tutorial/value-display-in-visual.png)

3. <span data-ttu-id="944ea-295">Разверните представление до узла **metadata** (метаданные) и разверните массив **columns** (столбцы). Обратите особое внимание на значения **format** и **displayName**.</span><span class="sxs-lookup"><span data-stu-id="944ea-295">Expand down into **metadata**, and then into the **columns** array, and in particular notice the **format** and **displayName** values.</span></span>

    ![Значение Displayname](media/custom-visual-develop-tutorial/displayname-and-format-metadata.png)

4. <span data-ttu-id="944ea-297">Чтобы вернуться к отображению визуального элемента, на панели инструментов над визуальным элементом выберите **Показать представление данных**.</span><span class="sxs-lookup"><span data-stu-id="944ea-297">To toggle back to the visual, in the toolbar floating above the visual, select **Show Dataview**.</span></span>

    ![Переключение отображения](media/custom-visual-develop-tutorial/show-dataview-toolbar-revert.png)

### <a name="consume-data-in-the-visual-code"></a><span data-ttu-id="944ea-299">Использование данных в коде визуального элемента</span><span class="sxs-lookup"><span data-stu-id="944ea-299">Consume data in the visual code</span></span>

1. <span data-ttu-id="944ea-300">В **Visual Studio Code** импортируйте в файл **visual.ts**</span><span class="sxs-lookup"><span data-stu-id="944ea-300">In **Visual Studio Code**, in the **visual.ts** file,</span></span>

    <span data-ttu-id="944ea-301">интерфейс `DataView` из модуля `powerbi`.</span><span class="sxs-lookup"><span data-stu-id="944ea-301">import the `DataView` interface from `powerbi` module</span></span>

    ```typescript
    import DataView = powerbi.DataView;
    ```

    <span data-ttu-id="944ea-302">Затем добавьте следующую инструкцию в качестве первой инструкции метода update.</span><span class="sxs-lookup"><span data-stu-id="944ea-302">and add the following statement as the first statement of the update method.</span></span>

    ```typescript
    let dataView: DataView = options.dataViews[0];
    ```

    ![Представление данных в массиве update](media/custom-visual-develop-tutorial/dataview-in-update-array.png)

    <span data-ttu-id="944ea-304">Эта инструкция сохраняет значение *dataView* в переменной для упрощения доступа и объявляет эту переменную, чтобы она ссылалась на объект *dataView*.</span><span class="sxs-lookup"><span data-stu-id="944ea-304">This statement assigns the *dataView* to a variable for easy access, and declares the variable to reference the *dataView* object.</span></span>

2. <span data-ttu-id="944ea-305">В методе **update** замените **.text("Value")** следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="944ea-305">In the **update** method, replace **.text("Value")** with the following.</span></span>

    ```typescript
    .text(<string>dataView.single.value)
    ```

    ![Замена значения textValue](media/custom-visual-develop-tutorial/text-value-replace.png)

3. <span data-ttu-id="944ea-307">В методе **update** замените **text("Label")** следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="944ea-307">In the **update** method, replace **.text("Label")** with the following.</span></span>

    ```typescript
    .text(dataView.metadata.columns[0].displayName)
    ```

    ![Замена значения textLabel](media/custom-visual-develop-tutorial/text-label-replace.png)

4. <span data-ttu-id="944ea-309">Сохраните файл **visual.ts**.</span><span class="sxs-lookup"><span data-stu-id="944ea-309">Save the **visual.ts** file.</span></span>

5. <span data-ttu-id="944ea-310">Посмотрите на визуальный элемент в **Power BI** — на нем теперь отображаются значение и имя.</span><span class="sxs-lookup"><span data-stu-id="944ea-310">In **Power BI**, review the visual, which now displays the value and the display name.</span></span>

<span data-ttu-id="944ea-311">Итак, вы настроили роли данных и привязали визуальный элемент к представлению данных.</span><span class="sxs-lookup"><span data-stu-id="944ea-311">You have now configured the data roles and bound the visual to the dataview.</span></span>

<span data-ttu-id="944ea-312">В следующем руководстве вы узнаете, как в пользовательский визуальный элемент добавить параметры форматирования.</span><span class="sxs-lookup"><span data-stu-id="944ea-312">In the next tutorial you learn how to add formatting options to the custom visual.</span></span>

## <a name="debugging"></a><span data-ttu-id="944ea-313">Отладка</span><span class="sxs-lookup"><span data-stu-id="944ea-313">Debugging</span></span>

<span data-ttu-id="944ea-314">Советы по отладке настраиваемого визуального элемента см. в [руководстве по отладке](https://microsoft.github.io/PowerBI-visuals/docs/how-to-guide/how-to-debug/).</span><span class="sxs-lookup"><span data-stu-id="944ea-314">For tips about debugging your custom visual, see the [debugging guide](https://microsoft.github.io/PowerBI-visuals/docs/how-to-guide/how-to-debug/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="944ea-315">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="944ea-315">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="944ea-316">Добавление параметров форматирования</span><span class="sxs-lookup"><span data-stu-id="944ea-316">Adding formatting options</span></span>](custom-visual-develop-tutorial-format-options.md)
