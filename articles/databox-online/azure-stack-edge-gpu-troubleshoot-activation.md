---
title: Uso de Azure Portal para solucionar problemas de errores de activación relacionados con Azure Stack Edge Pro con GPU | Microsoft Docs
description: Describe cómo solucionar los problemas relacionados con el almacén de claves y la activación de Azure Stack Edge Pro con GPU.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: troubleshooting
ms.date: 10/08/2020
ms.author: alkohli
ms.openlocfilehash: e93a7fd7aec5463a3d77bd9d6bb17d7072097870
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96447627"
---
# <a name="troubleshoot-activation-issues-on-your-azure-stack-edge-pro-gpu-device"></a>Solución de problemas de activación en un dispositivo Azure Stack Edge Pro con GPU 

<!--[!INCLUDE [applies-to-skus](../../includes/azure-stack-edge-applies-to-all-sku.md)]-->

En este artículo se describe cómo solucionar los problemas de activación de un dispositivo Azure Stack Edge Pro con GPU. 


## <a name="activation-errors"></a>Errores de activación

En la tabla siguiente se resumen los errores relacionados con la activación del dispositivo y la solución recomendada correspondiente.

| Mensaje de error| Resolución recomendada |
|------------------------------------------------------|--------------------------------------|
| Si la instancia de Azure Key Vault que se usa para la activación se elimina antes de que el dispositivo se active con la clave de activación, recibirá este error. <br> ![Error del almacén de claves 1](./media/azure-stack-edge-gpu-troubleshoot-activation/key-vault-error-1.png)  | Si se ha eliminado el almacén de claves, puede recuperarlo si está en duración de la protección de purga. Siga los pasos descritos en [Recuperación de un almacén de claves](../key-vault/general/key-vault-recovery.md#list-recover-or-purge-soft-deleted-secrets-keys-and-certificates). <br>Si ha transcurrido la duración de la protección de purga, no se puede recuperar el almacén de claves. Póngase en contacto con el servicio de soporte técnico de Microsoft para conocer los pasos siguientes. |
| Si la instancia de Azure Key Vault se elimina después de activar el dispositivo y, a continuación, intenta realizar cualquier operación que implique el cifrado, como **Agregar usuario**, **Agregar recurso compartido** o **Configurar proceso**, recibirá este error. <br> ![Error del almacén de claves 2](./media/azure-stack-edge-gpu-troubleshoot-activation/key-vault-error-2.png)    | Si se ha eliminado el almacén de claves, puede recuperarlo si está en duración de la protección de purga. Siga los pasos descritos en Recuperación de un almacén de claves. <br>Si ha transcurrido la duración de la protección de purga, no se puede recuperar el almacén de claves. Póngase en contacto con el servicio de soporte técnico de Microsoft para conocer los pasos siguientes. |
| Si se elimina la clave de integridad de canal en Azure Key Vault y, a continuación, intenta realizar cualquier operación que implique el cifrado, como **Agregar usuario**, **Agregar recurso compartido** o **Configurar proceso**, recibirá este error. <br> ![Error del almacén de claves 3](./media/azure-stack-edge-gpu-troubleshoot-activation/key-vault-error-3.png) | Si se elimina la clave de integridad de canal en el almacén de claves, pero sigue estando dentro de la duración de la purga, siga los pasos descritos en [Deshacer eliminación de la clave de Key Vault](/powershell/module/az.keyvault/undo-azkeyvaultkeyremoval). <br>Si ha transcurrido la duración de la protección de purga y se ha realizado una copia de seguridad de la clave, puede restaurar desde la copia de seguridad. De lo contrario, no podrá recuperar la clave. Póngase en contacto con el servicio de soporte técnico de Microsoft para conocer los pasos siguientes. |
| Si se produce un problema en la generación de la clave de activación debido a un error, recibirá este error. Hay detalles adicionales en la notificación. <br> ![Error del almacén de claves 4](./media/azure-stack-edge-gpu-troubleshoot-activation/key-vault-error-4.png)   | Espere unos minutos y vuelva a intentar la operación. Si el persiste el problema, póngase en contacto con el Soporte técnico de Microsoft. |
| Si el usuario tiene permisos de solo lectura, no puede generar una clave de activación y se presenta este error. <br> ![Error del almacén de claves 5](./media/azure-stack-edge-gpu-troubleshoot-activation/key-vault-error-5.png) | Esto puede deberse a que no tiene el acceso correcto o a que *Microsoft.KeyVault* no está registrado.<li>Asegúrese de que tiene acceso de propietario o colaborador en el nivel de grupo de recursos que se usa para el recurso de Azure Stack Edge.</li><li>Asegúrese de que el proveedor de recursos de Microsoft.KeyVault esté registrado. Para registrar un proveedor de recursos, vaya a la suscripción usada para el recurso de Azure Stack Edge. Vaya a **Proveedores de recursos**, busque *Microsoft.KeyVault* y seleccione **Registrar**.</li> |

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre la [Solución de problemas en un dispositivo](azure-stack-edge-gpu-troubleshoot.md).