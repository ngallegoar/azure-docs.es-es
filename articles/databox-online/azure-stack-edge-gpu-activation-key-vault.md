---
title: Integración de Azure Key Vault con la activación de dispositivos y recursos de Azure Stack Edge
description: Se describe cómo Azure Key Vault está asociado con la administración de secretos durante la activación de dispositivos de Azure Stack Edge Pro.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: alkohli
ms.openlocfilehash: 8957d8982a3bfe1da2811dc10d0c3e77a72fc288
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2020
ms.locfileid: "92367608"
---
# <a name="azure-key-vault-integration-with-azure-stack-edge"></a>Integración de Azure Key Vault con Azure Stack Edge 

Azure Key Vault se integra con los recursos de Azure Stack Edge para la administración de secretos. En este artículo se proporcionan detalles sobre cómo se crea una instancia de Azure Key Vault para recursos de Azure Stack Edge durante la activación de dispositivos y, a continuación, se usa para la administración de secretos. 


## <a name="about-key-vault-and-azure-stack-edge"></a>Acerca de Key Vault y Azure Stack Edge

El servicio en la nube Azure Key Vault se puede utilizar para almacenar de forma segura y controlar el acceso a los tokens, las contraseñas, los certificados, las claves de API y otros secretos. Key Vault también facilita la creación y control de las claves de cifrado utilizadas para cifrar los datos. Para obtener más información sobre las transacciones permitidas y sus correspondientes cargos, consulte [Precios de Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).

Para el servicio Azure Stack Edge, uno de los secretos usados es la clave de integridad del canal (CIK). Esta clave le permite cifrar los secretos. Con la integración de Key Vault, la clave CIK se almacena de forma segura en el almacén de claves. Para más información, consulte [Almacenamiento seguro de secretos y claves](../key-vault/general/overview.md#securely-store-secrets-and-keys).


## <a name="key-vault-creation"></a>Creación de Key Vault

Se crea un almacén de claves para el recurso de Azure Stack Edge durante el proceso de generación de claves de activación. 

- Al crear el recurso de Azure Stack Edge, debe registrar el proveedor de recursos *Microsoft.KeyVault*. El proveedor de recursos se registra automáticamente si tiene acceso de propietario o colaborador a la suscripción. El almacén de claves se crea en la misma suscripción y grupo de recursos que el recurso de Azure Stack Edge. 

- Cuando se crea un recurso de Azure Stack Edge, también se crea una instancia de Managed Service Identity (MSI) que persiste durante la vigencia del recurso y se comunica con el proveedor de recursos en la nube. 

    Cuando se habilita la instancia de MSI, Azure crea una identidad de confianza para el recurso de Azure Stack Edge.

- Después de crear el recurso de Azure Stack Edge y generar una clave de activación desde Azure Portal, se crea un almacén de claves. Este almacén de claves se usa para la administración de secretos y persiste durante el tiempo que exista el recurso de Azure Stack Edge. 

    ![Almacén de claves creado durante la generación de claves de activación](media/azure-stack-edge-gpu-deploy-prep/azure-stack-edge-resource-3.png)

- Puede elegir aceptar el nombre de clave predeterminado o especificar un nombre personalizado para el almacén de claves. El nombre de almacén de claves debe tener entre 3 y 24 caracteres de longitud. No se puede usar un almacén de claves que ya esté en uso. <!--The MSI is then used to authenticate to key vault to retrieve secrets.--> 

    ![MSI creado durante la creación del recurso de Azure Stack Edge](media/azure-stack-edge-gpu-deploy-prep/create-resource-8.png)

- Para ir al almacén de claves de Azure, vaya a **Propiedades** en el recurso de Azure Stack Edge y seleccione el nombre del almacén de claves. 

- Para evitar la eliminación accidental, se habilita un bloqueo de recursos en el almacén de claves. También se habilita una eliminación temporal en el almacén de claves que permite restaurar el almacén de claves en un plazo de 90 días si hay una eliminación accidental. Para más información, consulte [Información general sobre la eliminación temporal de Azure Key Vault](../key-vault/general/soft-delete-overview.md).

    Si el almacén de claves se elimina accidentalmente y no ha transcurrido el período de protección de purga de 90 días, siga estos pasos para la [Recuperación del almacén de claves](../key-vault/general/soft-delete-powershell.md#recovering-a-key-vault). 

- Si existe un recurso de Azure Stack Edge antes de que se integre Azure Key Vault con el recurso de Azure Stack Edge, no se verá afectado. Puede seguir usando el recurso de Azure Stack Edge existente. 

- Cuando se elimina el recurso de Azure Stack Edge, Azure Key Vault también se elimina con el recurso. Se le pedirá confirmación. Si no desea eliminar este almacén de claves, puede elegir no dar su consentimiento. Solo se elimina el recurso de Azure Stack Edge, lo que deja intacto el almacén de claves. 

- Si este almacén de claves se usó para almacenar otras claves, puede restaurarlo en un plazo de 90 días después de la eliminación. Durante ese período de protección de purga, no se puede usar el nombre del almacén de claves para crear un nuevo almacén de claves.

Si tiene problemas relacionados con el almacén de claves y la activación de dispositivos, consulte [Solución de problemas de activación de dispositivos](azure-stack-edge-gpu-troubleshoot-activation.md).

<!--## Key vault secret management

When you generate an activation key, the following events occur:

1. You request an activation key in the Azure portal. The request is then sent to Key Vault resource provider. 
1. A standard tier key vault with access policy is created and is locked by default. This key vault uses the default name or the custom name that you specified.
1. The key vault authenticates with MSI the request to generate activation key. The MSI is also added to the key vault access policy and a channel integrity key is generated and placed in the key vault.
1. The activation key is returned to the Azure portal. You can then copy this key and use it in the local UI to activate your device.-->



## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre la [Generación de la clave de activación](azure-stack-edge-gpu-deploy-prep.md#get-the-activation-key).

