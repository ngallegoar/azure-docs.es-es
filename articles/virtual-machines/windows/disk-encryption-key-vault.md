---
title: Creación y configuración de un almacén de claves para Azure Disk Encryption
description: En este artículo se proporcionan los pasos necesarios para crear y configurar un almacén de claves para su uso con Azure Disk Encryption
ms.service: virtual-machines
ms.subservice: security
ms.topic: article
author: msmbaldwin
ms.author: mbaldwin
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: f1317989434cca9375c612c8af0525323c6dda3c
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "82081666"
---
# <a name="creating-and-configuring-a-key-vault-for-azure-disk-encryption"></a>Creación y configuración de un almacén de claves para Azure Disk Encryption

Azure Disk Encryption usa Azure Key Vault para controlar y administrar las claves y los secretos de cifrado de discos.  Para más información sobre los almacenes de claves, consulte [Introducción a Azure Key Vault](../../key-vault/key-vault-get-started.md) y [Protección de un almacén de claves](../../key-vault/general/secure-your-key-vault.md). 

> [!WARNING]
> - Si ya ha usado Azure Disk Encryption con Azure AD para cifrar una VM, debe seguir usando esta opción para cifrar la VM. Para más información, consulte [Creación y configuración de un almacén de claves para Azure Disk Encryption con Azure AD (versión anterior)](disk-encryption-key-vault-aad.md).

La creación y configuración de un almacén de claves para su uso con Azure Disk Encryption conlleva estos tres pasos:

1. Creación de un grupo de recursos, en caso de que sea necesario.
2. Creación de un almacén de claves. 
3. Establecimiento de directivas de acceso avanzadas del almacén de claves.

Estos pasos se muestran en los siguientes tutoriales de inicio rápido:

- [Creación y cifrado de una máquina virtual Windows con la CLI de Azure](disk-encryption-cli-quickstart.md)
- [Creación y cifrado de una máquina virtual Windows con Azure PowerShell](disk-encryption-cli-quickstart.md)

Si lo desea, también puede generar o importar una clave de cifrado de claves (KEK).

> [!Note]
> Los pasos que se describen en este artículo se automatizan en el [script de la CLI de requisitos previos de Azure Disk Encryption](https://github.com/ejarvi/ade-cli-getting-started) y en el [script de PowerShell de requisitos previos de Azure Disk Encryption](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts).

## <a name="install-tools-and-connect-to-azure"></a>Instalación de herramientas y conexión a Azure

Los pasos que se describen en este artículo se pueden completar con la [CLI de Azure](/cli/azure/), el [módulo Az de Azure PowerShell](/powershell/azure/overview) o [Azure Portal](https://portal.azure.com).

Aunque se puede acceder al portal a través de un explorador, tanto la CLI de Azure como Azure PowerShell requieren una instalación local; consulte [Azure Disk Encryption para Windows: instalación de herramientas](disk-encryption-windows.md#install-tools-and-connect-to-azure) para más información.

### <a name="connect-to-your-azure-account"></a>Conexión a la cuenta de Azure

Antes de usar la CLI de Azure o Azure PowerShell, es preciso conectarse a su suscripción de Azure. Para ello, [inicie sesión con la CLI de Azure](/cli/azure/authenticate-azure-cli?view=azure-cli-latest), [inicie sesión con Azure Powershell](/powershell/azure/authenticate-azureps?view=azps-2.5.0) o escriba sus credenciales en Azure Portal cuando se le soliciten.

```azurecli-interactive
az login
```

```azurepowershell-interactive
Connect-AzAccount
```

[!INCLUDE [disk-encryption-key-vault](../../../includes/disk-encryption-key-vault.md)]
 
## <a name="next-steps"></a>Pasos siguientes

- [Script de la CLI de requisitos previos de Azure Disk Encryption](https://github.com/ejarvi/ade-cli-getting-started)
- [Script de PowerShell de requisitos previos de Azure Disk Encryption](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts)
- Información acerca de los [escenarios de Azure Disk Encryption en máquinas virtuales Windows](disk-encryption-windows.md)
- Aprenda a [solucionar los problemas de Azure Disk Encryption](disk-encryption-troubleshooting.md)
- Lea los [scripts de ejemplo de Azure Disk Encryption](disk-encryption-sample-scripts.md)
