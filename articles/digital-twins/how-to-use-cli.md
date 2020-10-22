---
title: Uso de la CLI de Azure Digital Twins
titleSuffix: Azure Digital Twins
description: Consulte el procedimiento de inicio y para usar la CLI de Azure Digital Twins.
author: baanders
ms.author: baanders
ms.date: 05/25/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 118e137f06a49f2c125b1ca156877514d65af86f
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92047070"
---
# <a name="use-the-azure-digital-twins-cli"></a>Uso de la CLI de Azure Digital Twins

Además de administrar la instancia de Azure Digital Twins en Azure Portal, Azure Digital Twins tiene una **interfaz de la línea de comandos (CLI)** que puede usar para realizar la mayoría de las acciones principales con el servicio, entre las que se incluyen:
* Administración de una instancia de Azure Digital Twins
* Administración de modelos
* Administración de gemelos digitales
* Administración de relaciones gemelas
* Configuración de puntos de conexión
* Administración de [rutas](concepts-route-events.md)
* Configuración de la [seguridad](concepts-security.md) mediante el control de acceso basado en rol (RBAC)

[!INCLUDE [digital-twins-known-issue-cloud-shell](../../includes/digital-twins-known-issue-cloud-shell.md)]

## <a name="uses-deploy-and-validate"></a>Usos (implementación y validación)

Además de administrar la instancia, generalmente, la CLI es también una herramienta útil para la implementación y la validación.
* Los comandos de plano de control se pueden usar para hacer que la implementación de una nueva instancia sea repetible o automatizada.
* Los comandos de plano de datos se pueden usar para comprobar rápidamente los valores de la instancia y que las operaciones se completaron según lo previsto.

## <a name="get-the-extension"></a>Obtener la extensión

Los comandos de Azure Digital Twins forman parte de la [extensión de Azure IoT para la CLI de Azure](https://github.com/Azure/azure-iot-cli-extension). Puede ver la lista completa de comandos y su uso como parte de la documentación de referencia del conjunto de comandos `az iot`: [referencia del comando *az dt*](/cli/azure/ext/azure-iot/dt?preserve-view=true&view=azure-cli-latest).

Puede asegurarse de que tiene la última versión de la extensión con estos pasos. Puede ejecutar estos comandos en [Azure Cloud Shell](../cloud-shell/overview.md) o en una [CLI de Azure local](/cli/azure/install-azure-cli?preserve-view=true&view=azure-cli-latest).

[!INCLUDE [digital-twins-cloud-shell-extensions.md](../../includes/digital-twins-cloud-shell-extensions.md)]

## <a name="next-steps"></a>Pasos siguientes

Explore la CLI y su conjunto completo de comandos a través de los documentos de referencia:
* [Referencia del comando *az dt*](/cli/azure/ext/azure-iot/dt?preserve-view=true&view=azure-cli-latest)