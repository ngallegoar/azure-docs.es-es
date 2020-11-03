---
title: Uso de la CLI de Azure Digital Twins
titleSuffix: Azure Digital Twins
description: Consulte el procedimiento de inicio y para usar la CLI de Azure Digital Twins.
author: baanders
ms.author: baanders
ms.date: 05/25/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 5e4c49e7aea05b6f430860eb6975713f59ad8080
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92635990"
---
# <a name="use-the-azure-digital-twins-cli"></a>Uso de la CLI de Azure Digital Twins

Además de administrar la instancia de Azure Digital Twins en Azure Portal, Azure Digital Twins tiene un **conjunto de comandos para la [CLI de Azure](/cli/azure/what-is-azure-cli)** que puede usar para realizar la mayoría de las acciones principales con el servicio, entre las que se incluyen:
* Administración de una instancia de Azure Digital Twins
* Administración de modelos
* Administración de gemelos digitales
* Administración de relaciones gemelas
* Configuración de puntos de conexión
* Administración de [rutas](concepts-route-events.md)
* Configuración de la [seguridad](concepts-security.md) mediante el control de acceso basado en rol de Azure (RBAC de Azure)

El conjunto de comandos se llama **az dt** y forma parte de la [extensión de Azure IoT para la CLI de Azure](https://github.com/Azure/azure-iot-cli-extension). Puede ver la lista completa de comandos y su uso como parte de la documentación de referencia del conjunto de comandos `az iot`: [referencia del comando *az dt*](/cli/azure/ext/azure-iot/dt?preserve-view=true&view=azure-cli-latest).

## <a name="uses-deploy-and-validate"></a>Usos (implementación y validación)

Además de administrar la instancia, generalmente, la CLI es también una herramienta útil para la implementación y la validación.
* Los comandos de plano de control se pueden usar para hacer que la implementación de una nueva instancia sea repetible o automatizada.
* Los comandos de plano de datos se pueden usar para comprobar rápidamente los valores de la instancia y que las operaciones se completaron según lo previsto.

## <a name="get-the-command-set"></a>Obtención del conjunto de comandos

Los comandos de Azure Digital Twins forman parte de la [extensión de Azure IoT para la CLI de Azure (azure-iot)](https://github.com/Azure/azure-iot-cli-extension), por lo que debe seguir estos pasos para asegurarse de que tiene la extensión `azure-iot` más reciente con los comandos **az dt**.

### <a name="cli-version-requirements"></a>Requisitos de versión de la CLI

Si usa la CLI de Azure con PowerShell, el paquete de extensión requiere que la versión de la CLI de Azure sea la **2.3.1** o superior.

Puede comprobar la versión de la CLI de Azure con este comando de la CLI:
```azurecli
az --version
```

Para obtener instrucciones sobre cómo instalar o actualizar la CLI de Azure a una versión más reciente, consulte [*Instalación de la CLI de Azure*](/cli/azure/install-azure-cli).

### <a name="get-the-extension"></a>Obtener la extensión

Puede asegurarse de que tiene la última versión de la extensión `azure-iot` con estos pasos. Puede ejecutar estos comandos en [Azure Cloud Shell](../cloud-shell/overview.md) o en una [CLI de Azure local](/cli/azure/install-azure-cli?preserve-view=true&view=azure-cli-latest).

[!INCLUDE [digital-twins-cloud-shell-extensions.md](../../includes/digital-twins-cloud-shell-extensions.md)]

## <a name="next-steps"></a>Pasos siguientes

Explore la CLI y su conjunto completo de comandos a través de los documentos de referencia:
* [Referencia del comando *az dt*](/cli/azure/ext/azure-iot/dt?preserve-view=true&view=azure-cli-latest)