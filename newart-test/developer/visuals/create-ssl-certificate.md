---
title: Создание SSL-сертификата
description: Обходной путь для ручного создания сертификатов для сервера разработчика
author: KesemSharabi
ms.author: kesharab
manager: rkarlin
ms.reviewer: sranins
ms.service: powerbi
ms.subservice: powerbi-custom-visuals
ms.topic: tutorial
ms.date: 06/18/2019
ms.openlocfilehash: c96489e6577f4887d2f22a9e81ea50f46cc9a5a3
ms.sourcegitcommit: a1c994bc8fa5f072fce13a6f35079e87d45f00f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72424277"
---
# <a name="create-an-ssl-certificate"></a><span data-ttu-id="974e3-103">Создание SSL-сертификата</span><span class="sxs-lookup"><span data-stu-id="974e3-103">Create an SSL certificate</span></span>

<span data-ttu-id="974e3-104">В этой статье описывается, как создать SSL-сертификат.</span><span class="sxs-lookup"><span data-stu-id="974e3-104">This article describes how to create an SSL certificate.</span></span>

<span data-ttu-id="974e3-105">Чтобы создать сертификат с помощью командлета PowerShell `New-SelfSignedCertificate` в Windows 8 или более поздней версии, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="974e3-105">To generate the certificate by using the PowerShell `New-SelfSignedCertificate` cmdlet on Windows 8 or later, run the following command:</span></span>

```cmd
pbiviz --create-cert
```

<span data-ttu-id="974e3-106">Этому средству требуется установка OpenSSL для Windows 7.</span><span class="sxs-lookup"><span data-stu-id="974e3-106">The tool requires an OpenSSL installation for Windows 7.</span></span> <span data-ttu-id="974e3-107">Служебная программа OpenSSL должна быть доступна из командной строки.</span><span class="sxs-lookup"><span data-stu-id="974e3-107">The OpenSSL utility must be available from the command line.</span></span>

<span data-ttu-id="974e3-108">Чтобы установить OpenSSL, перейдите на сайт [OpenSSL](https://www.openssl.org) или [OpenSSL Binaries](https://wiki.openssl.org/index.php/Binaries).</span><span class="sxs-lookup"><span data-stu-id="974e3-108">To install OpenSSL, go to the [OpenSSL](https://www.openssl.org) or [OpenSSL Binaries](https://wiki.openssl.org/index.php/Binaries) site.</span></span>



## <a name="create-a-certificate-mac-os-x"></a><span data-ttu-id="974e3-109">Создание сертификата (Mac OS X)</span><span class="sxs-lookup"><span data-stu-id="974e3-109">Create a certificate (Mac OS X)</span></span>

<span data-ttu-id="974e3-110">Как правило, служебная программа OpenSSL доступна в операционной системе Linux или Mac OS X.</span><span class="sxs-lookup"><span data-stu-id="974e3-110">Usually, the OpenSSL utility is available in the Linux or Mac OS X operating system.</span></span>

<span data-ttu-id="974e3-111">Для установки этой служебной программы также можно выполнить любую из следующих команд:</span><span class="sxs-lookup"><span data-stu-id="974e3-111">You can also install the utility by running either of the following commands:</span></span>
* <span data-ttu-id="974e3-112">Из диспетчера пакетов *Brew*:</span><span class="sxs-lookup"><span data-stu-id="974e3-112">From the *Brew* package manager:</span></span>

    ```cmd
    brew install openssl
    brew link openssl --force
    ```

* <span data-ttu-id="974e3-113">С помощью *MacPorts*:</span><span class="sxs-lookup"><span data-stu-id="974e3-113">By using *MacPorts*:</span></span>

    ```cmd
    sudo port install openssl
    ```

<span data-ttu-id="974e3-114">После установки служебной программы OpenSSL для создания нового сертификата выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="974e3-114">After you install the OpenSSL utility for generating a new certificate, run the following command:</span></span>

```cmd
pbiviz --create-cert
```

## <a name="create-a-certificate-linux"></a><span data-ttu-id="974e3-115">Создание сертификата (Linux)</span><span class="sxs-lookup"><span data-stu-id="974e3-115">Create a certificate (Linux)</span></span>

<span data-ttu-id="974e3-116">Если служебная программа OpenSSL недоступна в операционной системе Linux, вы можете установить ее, выполнив одну из следующих команд:</span><span class="sxs-lookup"><span data-stu-id="974e3-116">If the OpenSSL utility isn't available in your Linux operating system, you can install it by using one of the following commands:</span></span>

* <span data-ttu-id="974e3-117">Для диспетчера пакетов *APT*:</span><span class="sxs-lookup"><span data-stu-id="974e3-117">For *APT* package manager:</span></span>

    ```cmd
    sudo apt-get install openssl
    ```

* <span data-ttu-id="974e3-118">Для *Yellowdog Updater*:</span><span class="sxs-lookup"><span data-stu-id="974e3-118">For *Yellowdog Updater*:</span></span>

    ```cmd
    yum install openssl
    ```

* <span data-ttu-id="974e3-119">Для *Redhat Package Manager*:</span><span class="sxs-lookup"><span data-stu-id="974e3-119">For *Redhat Package Manager*:</span></span>

    ```cmd
    rpm install openssl
    ```

<span data-ttu-id="974e3-120">Если служебная программа OpenSSL уже доступна в вашей операционной системе, создайте новый сертификат, выполнив следующую команду:</span><span class="sxs-lookup"><span data-stu-id="974e3-120">If the OpenSSL utility is already available in your operating system, generate a new certificate by running the following command:</span></span>

```cmd
pbiviz --create-cert
```

<span data-ttu-id="974e3-121">Также для получения служебной программы OpenSSL можно воспользоваться сайтом [OpenSSL](https://www.openssl.org) или [OpenSSL Binaries](https://wiki.openssl.org/index.php/Binaries).</span><span class="sxs-lookup"><span data-stu-id="974e3-121">Or you can get the OpenSSL utility by going to the [OpenSSL](https://www.openssl.org) or [OpenSSL Binaries](https://wiki.openssl.org/index.php/Binaries) site.</span></span>

## <a name="generate-the-certificate-manually"></a><span data-ttu-id="974e3-122">Создание сертификата вручную</span><span class="sxs-lookup"><span data-stu-id="974e3-122">Generate the certificate manually</span></span>

<span data-ttu-id="974e3-123">Вы можете указать сертификаты, созданные с помощью любых средств.</span><span class="sxs-lookup"><span data-stu-id="974e3-123">You can specify that your certificates be generated by any tools.</span></span>

<span data-ttu-id="974e3-124">Если служебная программа OpenSSL уже установлена в вашей системе, создайте новый сертификат, выполнив следующие команды:</span><span class="sxs-lookup"><span data-stu-id="974e3-124">If the OpenSSL utility is already installed in your system, generate a new certificate by running the following commands:</span></span>

```cmd
openssl req -x509 -newkey rsa:4096 -keyout PowerBICustomVisualTest_private.key -out PowerBICustomVisualTest_public.crt -days 365
```

<span data-ttu-id="974e3-125">Для поиска сертификатов веб-сервера PowerBI-visuals-tools в большинстве случаев можно выполнить следующую команду:</span><span class="sxs-lookup"><span data-stu-id="974e3-125">You can usually find the PowerBI-visuals-tools web server certificates by running one the following:</span></span>

* <span data-ttu-id="974e3-126">Глобальные экземпляры средств:</span><span class="sxs-lookup"><span data-stu-id="974e3-126">For the global instance of the tools:</span></span>

    ```cmd
    %appdata%\npm\node_modules\PowerBI-visuals-tools\certs
    ```

* <span data-ttu-id="974e3-127">Локальные экземпляры средств:</span><span class="sxs-lookup"><span data-stu-id="974e3-127">For the local instance of the tools:</span></span>

    ```cmd
    <custom visual project root>\node_modules\PowerBI-visuals-tools\certs
    ```

<span data-ttu-id="974e3-128">Если вы используете формат PEM, сохраните файл сертификата под именем *PowerBICustomVisualTest_public.crt* и закрытый ключ под именем *PowerBICustomVisualTest_public.key*.</span><span class="sxs-lookup"><span data-stu-id="974e3-128">If you use the PEM format, save the certificate file as *PowerBICustomVisualTest_public.crt*, and save privateKey as *PowerBICustomVisualTest_public.key*.</span></span>

<span data-ttu-id="974e3-129">Если используется формат PFX, сохраните файл сертификата под именем *PowerBICustomVisualTest_public.pfx*.</span><span class="sxs-lookup"><span data-stu-id="974e3-129">If you use the PFX format, save the certificate file as *PowerBICustomVisualTest_public.pfx*.</span></span>

<span data-ttu-id="974e3-130">Если для файла сертификата PFX требуется парольная фраза, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="974e3-130">If your PFX certificate file requires a passphrase, do the following:</span></span>
1. <span data-ttu-id="974e3-131">В файле конфигурации укажите:</span><span class="sxs-lookup"><span data-stu-id="974e3-131">In the config file, specify:</span></span>

    ```cmd
    \PowerBI-visuals-tools\config.json
    ```

1. <span data-ttu-id="974e3-132">В разделе `server` укажите парольную фразу, заменив местозаполнитель "*ВАША ПАРОЛЬНАЯ ФРАЗА*":</span><span class="sxs-lookup"><span data-stu-id="974e3-132">In the `server` section, specify the passphrase by replacing the "*YOUR PASSPHRASE*" placeholder:</span></span>

    ```cmd
    "server":{
        "root":"webRoot",
        "assetsRoute":"/assets",
        "privateKey":"certs/PowerBICustomVisualTest_private.key",
        "certificate":"certs/PowerBICustomVisualTest_public.crt",
        "pfx":"certs/PowerBICustomVisualTest_public.pfx",
        "port":"8080",
        "passphrase":"YOUR PASSPHRASE"
    }
    ```
