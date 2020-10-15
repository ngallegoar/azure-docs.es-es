---
title: 'Script de ejemplo: deshabilitación de la eliminación temporal para el recurso compartido de archivos'
description: Obtenga información sobre cómo usar un script para deshabilitar la eliminación temporal de recursos compartidos de archivos en una cuenta de almacenamiento.
ms.topic: sample
ms.date: 02/02/2020
ms.openlocfilehash: 172a7bd19ffd07ca7900a457c447362343e5f185
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84121288"
---
# <a name="disable-soft-delete-for-file-shares-in-a-storage-account"></a>Deshabilitación de la eliminación temporal para recursos compartidos de archivos en una cuenta de almacenamiento

En este documento se explica el proceso para deshabilitar la eliminación temporal de recursos compartidos de archivos en una cuenta de almacenamiento.

Siga estos pasos:

1. Instale ARMclient. Para obtener información sobre cómo instalarlo, visite [este vínculo](https://github.com/projectkudu/ARMClient).

2. Guarde los dos archivos del cuerpo de la solicitud siguientes en una carpeta del equipo.

    ```json
    rqbody-enableSoftDelete.json

    {
    "properties": {
        "shareDeleteRetentionPolicy": {
        "enabled":true,
        "days": 14
        }
    },
    "cors": {
        "corsRules": []
    }
    }

    rqbody-disableSoftDelete.json

    {
    "properties": {
        "shareDeleteRetentionPolicy": {
        "enabled":false,
        "days": 0
        }
    },
    "cors": {
        "corsRules": []
    }
    }
    ```

3. Tenga a mano el identificador de Azure Resource Manager (ARM) de la cuenta de almacenamiento. Por ejemplo: `/subscriptions/37aa2d43-d4f5-4322-bae0-6ee11c627f50/resourceGroups/afsshare/providers/Microsoft.Storage/storageAccounts/inquirytest`

4. Inicie sesión con sus credenciales; para ello, ejecute el **inicio de sesión de armclient**.

5. Obtenga las propiedades de la eliminación temporal actuales de los recursos compartidos de archivos de la cuenta de almacenamiento.

    La siguiente operación GET captura las propiedades de la eliminación temporal de recursos compartidos de archivos en la cuenta *inquirytest*:

    ```cmd
    armclient get /subscriptions/37aa2d43-d4f5-4322-bae0-6ee11c627f50/resourceGroups/afsshare /providers/Microsoft.Storage/storageAccounts/inquirytest/fileServices/default?api-version=2019-04-01
    ```

    ```output
    {
    "id": "/subscriptions/37aa2d43-d4f5-4322-bae0-6ee11c627f50/resourceGroups/Bugbash/providers/Microsoft.Storage/storageAccounts/inquirytest/fileServices/de
    fault",
    "name": "default",
    "type": "Microsoft.Storage/storageAccounts/fileServices",
    "properties": {
        "cors": {
        "corsRules": []
        },
        "shareDeleteRetentionPolicy": {
        "enabled": true,
        "days": 14
        }
    }
    }
    ```

6. Deshabilite la eliminación temporal para los recursos compartidos de archivos de la cuenta de almacenamiento.

    La siguiente operación PUT deshabilita las propiedades de la eliminación temporal para recursos compartidos de archivos en la cuenta *inquirytest*:

    ```cmd
    armclient put /subscriptions/37aa2d43-d4f5-4322-bae0-6ee11c627f50/resourceGroups/afsshare /providers/Microsoft.Storage/storageAccounts/inquirytest/fileServices/default?api-version=2019-04-01 .\rqbody-disableSoftDelete.json
    ```

    ```Output
    {
    "id": "/subscriptions/37aa2d43-d4f5-4322-bae0-6ee11c627f50/resourceGroups/Bugbash/providers/Microsoft.Storage/storageAccounts/inquirytest/fileServices/de
    fault",
    "name": "default",
    "type": "Microsoft.Storage/storageAccounts/fileServices",
    "properties": {
        "shareDeleteRetentionPolicy": {
        "enabled": false,
        "days": 0
        }
    }
    }
    ```

7. Si desea volver a habilitar la eliminación temporal, use el ejemplo siguiente.

    La siguiente operación PUT habilita las propiedades de la eliminación temporal para recursos compartidos de archivos en la cuenta "inquirytest".

    ```cmd
    armclient put /subscriptions/37aa2d43-d4f5-4322-bae0-6ee11c627f50/resourceGroups/afsshare /providers/Microsoft.Storage/storageAccounts/inquirytest/fileServices/default?api-version=2019-04-01 .\rqbody-EnableSoftDelete.json
    ```

    ```Output
    {
    "id": "/subscriptions/37aa2d43-d4f5-4322-bae0-6ee11c627f50/resourceGroups/Bugbash/providers/Microsoft.Storage/storageAccounts/inquirytest/fileServices/default",
    "name": "default",
    "type": "Microsoft.Storage/storageAccounts/fileServices",
    "properties": {
        "shareDeleteRetentionPolicy": {
        "enabled": true,
        "days": 14
        }
    }
    }
    ```
