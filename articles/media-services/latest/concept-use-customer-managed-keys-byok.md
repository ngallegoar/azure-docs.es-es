---
title: Bring Your Own Key (claves administradas por el cliente) con Media Services
description: Puede usar una clave administrada por el cliente (es decir, Bring Your Own Key) con Media Services.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: conceptual
ms.date: 10/14/2020
ms.openlocfilehash: a56922c972efeb21c188413522bd05f83b74ca12
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94681829"
---
# <a name="bring-your-own-key-customer-managed-keys-with-media-services"></a>Bring Your Own Key (claves administradas por el cliente) con Media Services

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Bring Your Own Key (BYOK) es una iniciativa de Azure para ayudar a los clientes a trasladar sus cargas de trabajo a la nube. Las claves administradas por el cliente permiten a este cumplir las normas del sector y mejorar el aislamiento de los inquilinos de un servicio. Conceder a los clientes el control de las claves de cifrado es una manera de minimizar el acceso y el control innecesarios y construir confianza en los servicios de Microsoft.

## <a name="keys-and-key-management"></a>Claves y administración de claves

Puede usar su propia clave con Media Services cuando use la API Media Services 2020-05-01. Se crea una clave de cuenta predeterminada para todas las cuentas que se cifran mediante una clave del sistema propiedad de Media Services. Cuando use su propia clave, la clave de cuenta se cifrará con la clave. Las claves de contenido se cifran mediante la clave de cuenta. También se cifran las direcciones URL de JobInputHttp y las claves de validación de tokens simétricos.

:::image type="content" source="./media/customer-managed-key/customer-managed-key.svg" alt-text="Una clave administrada por el cliente reemplaza una clave administrada por el sistema":::.

Media Services usa la identidad administrada de la cuenta de Media Services para leer la clave de una instancia de Key Vault que le pertenece. Media Services requiere que el almacén Key Vault esté en la misma región que la cuenta y que tenga habilitada la eliminación temporal y la protección de purgas.

La clave puede ser una clave RSA 2048, 3072 o 4096, y se admiten las claves de software y HSM.

> [!NOTE]
> No se admiten las claves de EC.

Puede especificar un nombre de clave y una versión de clave, o simplemente un nombre de clave. Cuando use solo un nombre de clave, Media Services usará la versión más reciente de la clave. Las nuevas versiones de las claves de cliente se detectan automáticamente, y la clave de cuenta se vuelve a cifrar.

> [!WARNING]
> Media Services supervisa el acceso a la clave de cliente. Si la clave de cliente deja de estar accesible (por ejemplo, se ha eliminado la clave, se ha eliminado el almacén Key Vault o se ha quitado la concesión de acceso), Media Services pasará la cuenta al estado inaccesible de la clave de cliente (con lo que se deshabilitará la cuenta). Sin embargo, la cuenta se puede eliminar en este estado. Las únicas operaciones admitidas en la cuenta son GET, LIST y DELETE; todas las demás solicitudes (codificación, streaming, etc.) producirán un error hasta que se restaure el acceso a la clave de cuenta.

## <a name="tutorials"></a>Tutoriales

- [Uso de Azure Portal para usar claves administradas por el cliente o BYOK con Media Services](tutorial-byok-portal.md)
- [Uso de claves administradas por el cliente o BYOK con la API de REST de Media Services](tutorial-byok-postman.md).

## <a name="next-steps"></a>Pasos siguientes

[Protección del contenido mediante el cifrado dinámico de Media Services](content-protection-overview.md)
