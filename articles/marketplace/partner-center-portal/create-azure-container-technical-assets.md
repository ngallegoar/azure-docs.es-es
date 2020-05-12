---
title: 'Conceptos técnicos de las ofertas de contenedores de Azure: marketplace comercial de Microsoft'
description: Recursos técnicos e instrucciones para ayudarle a configurar una oferta de contenedor en Azure Marketplace.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/09/2020
ms.openlocfilehash: b3c6f88df151cc497f0de670d5d78a05c7477459
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/05/2020
ms.locfileid: "82791891"
---
# <a name="create-an-azure-container-offer"></a>Creación de una oferta de contenedor de Azure

> [!IMPORTANT]
> Estamos trasladando la administración de las ofertas de contenedores de Azure de Cloud Partner Portal al Centro de partners. Hasta que se migren las ofertas, siga las instrucciones de [Preparación de los recursos técnicos de contenedor](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/containers/cpp-create-technical-assets) para Cloud Partner Portal para administrar las ofertas.

En este artículo se proporcionan recursos técnicos y recomendaciones para ayudarle a crear una oferta de contenedor en Azure Marketplace.

## <a name="before-you-begin"></a>Antes de empezar

Para ver guías de inicio rápido, tutoriales y ejemplos, consulte [Documentación de Azure Container Instances](https://docs.microsoft.com/azure/container-instances).

## <a name="fundamental-technical-knowledge"></a>Conocimientos técnicos básicos

El diseño, la compilación y las pruebas de estos recursos lleva tiempo y requiere conocimientos técnicos de la plataforma de Azure y las tecnologías que se usan para crear la oferta.

Además del dominio de la solución, el equipo de ingeniería debe tener conocimientos sobre las siguientes tecnologías de Microsoft:

- Conocimientos básicos de los [Servicios de Azure](https://azure.microsoft.com/services/).
- Cómo [diseñar y estructurar las aplicaciones de Azure](https://azure.microsoft.com/solutions/architecture/).
- Conocimientos prácticos de [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage) y [redes de Azure](https://azure.microsoft.com/services/?filter=networking).
- Conocimientos prácticos de [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/).
- Conocimientos prácticos de [JSON](https://www.json.org/).

## <a name="suggested-tools"></a>Herramientas sugeridas

Elija uno o ambos de los entornos de scripting siguientes para ayudar a administrar la imagen de contenedor:

- [Azure PowerShell](https://docs.microsoft.com/powershell/azure/?view=azps-3.7.0&viewFallbackFrom=azps-3.6.1)
- [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

Se recomienda agregar estas herramientas al entorno de desarrollo:

- [Explorador de Azure Storage](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows)
- [Visual Studio Code](https://code.visualstudio.com/)
  - Extensión: [Herramientas de Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
  - Extensión: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
  - Extensión: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json).

Revise las herramientas disponibles en la página [Herramientas de desarrollo de Azure](https://azure.microsoft.com/). Si usa Visual Studio, revise las herramientas disponibles en [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

## <a name="create-the-container-image"></a>Creación de la imagen de contenedor

Para obtener más información, consulte los siguientes tutoriales:

- [Tutorial: Creación de una imagen de contenedor para su implementación en Azure Container Instances](https://docs.microsoft.com/azure/container-instances/container-instances-tutorial-prepare-app)
- [Tutorial: Compilación e implementación de imágenes de contenedor en la nube con Azure Container Registry Tasks](https://docs.microsoft.com/azure/container-registry/container-registry-tutorial-quick-task).

## <a name="next-steps"></a>Pasos siguientes

- [Creación de la oferta de contenedor](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-azure-container-offer).
