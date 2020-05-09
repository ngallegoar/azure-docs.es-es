---
title: Movimiento de un almacén a una región diferente en Azure Key Vault | Microsoft Docs
description: Instrucciones para mover un almacén de claves a una región diferente.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 04/24/2020
ms.author: sudbalas
Customer intent: As a key vault administrator, I want to move my vault to another region.
ms.openlocfilehash: e65a723ac9daafdc09896a50e197034104408df2
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82254073"
---
# <a name="moving-an-azure-key-vault-across-regions"></a>Movimiento de un almacén de Azure Key Vault entre regiones

## <a name="overview"></a>Información general

Key Vault no admite una operación de movimiento de recursos que permita mover un almacén de claves a otra región. En este artículo se tratan las soluciones alternativas si tiene una necesidad empresarial de trasladar un almacén de claves a otra región. Cada opción tiene limitaciones y es fundamental comprender las implicaciones de estas soluciones alternativas antes de intentarlas en un entorno de producción.

Si tiene que trasladar un almacén de claves a otra región, la solución es crear un nuevo almacén de claves en la región deseada y copiar manualmente cada secreto individual del almacén de claves existente al nuevo. Esta operación puede realizarse de una de las siguientes maneras.

## <a name="design-considerations"></a>Consideraciones de diseño

* Los nombres de Key Vault son únicos globalmente. No podrá volver a usar el mismo nombre de almacén.

* Tendrá que volver a configurar las directivas de acceso y la configuración de red en el nuevo almacén de claves.

* Tendrá que volver a configurar la protección de eliminación temporal y de purga en el nuevo almacén de claves.

* La operación de copia de seguridad y restauración no conservará la configuración de autogiro. Es posible que tenga que volver a configurar estos valores.

## <a name="option-1---use-the-key-vault-backup-and-restore-commands"></a>Opción 1: uso de los comandos para copia de seguridad y restauración de almacenes de claves

Puede realizar una copia de seguridad de cada secreto, clave y certificado individual del almacén mediante el comando de copia de seguridad. Los secretos se descargarán como un blob cifrado. Después, puede restaurar el blob en el nuevo almacén de claves. Los comandos se documentan en el vínculo siguiente.

[Comandos de Azure Key Vault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-6.13.0#key_vault)

### <a name="limitations"></a>Limitaciones

* No se puede realizar una copia de seguridad de un almacén de claves en una ubicación geográfica y restaurarlo en otra. Más información sobre las ubicaciones geográficas de Azure. [Vínculo](https://azure.microsoft.com/global-infrastructure/geographies/)

* El comando backup realiza una copia de seguridad de todas las versiones de cada secreto. Si tiene un secreto con un gran número de versiones anteriores (más de 10), existe la posibilidad de que la solicitud supere el tamaño máximo permitido de la solicitud y que se produzca un error en la operación.

## <a name="option-2---manually-download-and-upload-secrets"></a>Opción 2: descarga y carga manual de los secretos

Algunos tipos de secreto se pueden descargar manualmente. Por ejemplo, puede descargar certificados como un archivo .pfx. Esta opción elimina las restricciones geográficas de algunos tipos de secretos, como los certificados. Puede cargar los archivos .pfx en cualquier almacén de claves de cualquier región. El secreto se descargará en un formato sin protección con contraseña. Será responsable de proteger los secretos una vez que dejen Key Vault mientras se realiza el traslado.
