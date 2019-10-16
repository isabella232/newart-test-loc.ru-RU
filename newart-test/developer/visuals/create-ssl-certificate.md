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
# <a name="create-an-ssl-certificate"></a>Создание SSL-сертификата

В этой статье описывается, как создать SSL-сертификат.

Чтобы создать сертификат с помощью командлета PowerShell `New-SelfSignedCertificate` в Windows 8 или более поздней версии, выполните следующую команду:

```cmd
pbiviz --create-cert
```

Этому средству требуется установка OpenSSL для Windows 7. Служебная программа OpenSSL должна быть доступна из командной строки.

Чтобы установить OpenSSL, перейдите на сайт [OpenSSL](https://www.openssl.org) или [OpenSSL Binaries](https://wiki.openssl.org/index.php/Binaries).



## <a name="create-a-certificate-mac-os-x"></a>Создание сертификата (Mac OS X)

Как правило, служебная программа OpenSSL доступна в операционной системе Linux или Mac OS X.

Для установки этой служебной программы также можно выполнить любую из следующих команд:
* Из диспетчера пакетов *Brew*:

    ```cmd
    brew install openssl
    brew link openssl --force
    ```

* С помощью *MacPorts*:

    ```cmd
    sudo port install openssl
    ```

После установки служебной программы OpenSSL для создания нового сертификата выполните следующую команду:

```cmd
pbiviz --create-cert
```

## <a name="create-a-certificate-linux"></a>Создание сертификата (Linux)

Если служебная программа OpenSSL недоступна в операционной системе Linux, вы можете установить ее, выполнив одну из следующих команд:

* Для диспетчера пакетов *APT*:

    ```cmd
    sudo apt-get install openssl
    ```

* Для *Yellowdog Updater*:

    ```cmd
    yum install openssl
    ```

* Для *Redhat Package Manager*:

    ```cmd
    rpm install openssl
    ```

Если служебная программа OpenSSL уже доступна в вашей операционной системе, создайте новый сертификат, выполнив следующую команду:

```cmd
pbiviz --create-cert
```

Также для получения служебной программы OpenSSL можно воспользоваться сайтом [OpenSSL](https://www.openssl.org) или [OpenSSL Binaries](https://wiki.openssl.org/index.php/Binaries).

## <a name="generate-the-certificate-manually"></a>Создание сертификата вручную

Вы можете указать сертификаты, созданные с помощью любых средств.

Если служебная программа OpenSSL уже установлена в вашей системе, создайте новый сертификат, выполнив следующие команды:

```cmd
openssl req -x509 -newkey rsa:4096 -keyout PowerBICustomVisualTest_private.key -out PowerBICustomVisualTest_public.crt -days 365
```

Для поиска сертификатов веб-сервера PowerBI-visuals-tools в большинстве случаев можно выполнить следующую команду:

* Глобальные экземпляры средств:

    ```cmd
    %appdata%\npm\node_modules\PowerBI-visuals-tools\certs
    ```

* Локальные экземпляры средств:

    ```cmd
    <custom visual project root>\node_modules\PowerBI-visuals-tools\certs
    ```

Если вы используете формат PEM, сохраните файл сертификата под именем *PowerBICustomVisualTest_public.crt* и закрытый ключ под именем *PowerBICustomVisualTest_public.key*.

Если используется формат PFX, сохраните файл сертификата под именем *PowerBICustomVisualTest_public.pfx*.

Если для файла сертификата PFX требуется парольная фраза, выполните следующие действия:
1. В файле конфигурации укажите:

    ```cmd
    \PowerBI-visuals-tools\config.json
    ```

1. В разделе `server` укажите парольную фразу, заменив местозаполнитель "*ВАША ПАРОЛЬНАЯ ФРАЗА*":

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
