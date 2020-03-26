---
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 11/25/2018
ms.author: spelluru
ms.openlocfilehash: 5e3c4622131528fc2c40a1510aeea3092018d182
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "77474268"
---
### <a name="install-via-composer"></a>Instalación mediante el compositor
1. Cree un archivo con el nombre **composer.json** en la raíz del proyecto y agréguele el código siguiente:
   
    ```json
    {
      "require": {
        "microsoft/windowsazure": "*"
      }
    }
    ```
2. Descargue **[composer.phar][composer-phar]** en la raíz del proyecto.
3. Abra un símbolo del sistema y ejecute el siguiente comando en la raíz del proyecto
   
    ```
    php composer.phar install
    ```

También puede ir a la [biblioteca cliente de PHP de Azure Storage][php-sdk-github] en GitHub para clonar el código fuente.

[php-sdk-github]: https://github.com/Azure/azure-storage-php
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[download-SDK-PHP]: https://github.com/Azure/azure-sdk-for-php
[composer-phar]: http://getcomposer.org/composer.phar
