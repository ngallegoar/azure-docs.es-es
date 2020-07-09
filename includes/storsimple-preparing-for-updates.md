---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 8c60e0275853f3c879db22f5414f0fbbbdb47b85
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86050421"
---
## <a name="preparing-for-updates"></a>Preparación de las actualizaciones
Deberá realizar los siguientes pasos antes de buscar y aplicar la actualización:

1. Tome una instantánea de la nube de los datos del dispositivo.
2. Las direcciones IP fijas del controlador son enrutables y se pueden conectar a Internet. Estas direcciones IP fijas se usarán para atender las actualizaciones en el dispositivo. Para comprobarlo, ejecute el siguiente cmdlet en cada controlador de la interfaz de Windows PowerShell del dispositivo:
   
     `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter network>`
   
    **Salida de ejemplo para la conexión de prueba cuando los IP fijos pueden conectarse a Internet**

    ```output
    Controller0>Test-Connection -Source 10.126.173.91 -Destination bing.com

    Source      Destination     IPV4Address      IPV6Address
    ----------------- -----------  -----------
    HCSNODE0  bing.com        204.79.197.200
    HCSNODE0  bing.com        204.79.197.200
    HCSNODE0  bing.com        204.79.197.200
    HCSNODE0  bing.com        204.79.197.200

    Controller0>Test-Connection -Source 10.126.173.91 -Destination  204.79.197.200

    Source      Destination       IPV4Address    IPV6Address
    ----------------- -----------  -----------
    HCSNODE0  204.79.197.200  204.79.197.200
    HCSNODE0  204.79.197.200  204.79.197.200
    HCSNODE0  204.79.197.200  204.79.197.200
    HCSNODE0  204.79.197.200  204.79.197.200
    ```

Después de haber completado correctamente estas comprobaciones previas manuales, puede buscar e instalar las actualizaciones.

