---
title: Creación de recursos técnicos de una aplicación de Azure | Azure Marketplace
description: Creación de recursos técnicos para una oferta de aplicación de Azure.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: dsindona
ms.openlocfilehash: 59f9fecb3d949d9cdf48719e6329b066d9eb3fc5
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393532"
---
# <a name="prepare-your-azure-application-technical-assets"></a>Preparación de los recursos técnicos de aplicaciones de Azure

En este artículo se describen los recursos para preparar los recursos técnicos para su oferta de aplicaciones de Azure.

## <a name="before-you-begin"></a>Antes de empezar

Consulte el siguiente vídeo, [Building Solution Templates, and Managed Applications for the Azure Marketplace](https://channel9.msdn.com/Events/Build/2018/BRK3603) (Creación de plantillas de soluciones y aplicaciones administradas para Azure Marketplace), se ofrece información general sobre cómo crear una plantilla de Azure Resource Manager para definir una solución de aplicación de Azure y cómo publicar posteriormente la oferta de aplicación en Azure Marketplace.

>[!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3603/player]


Revise la documentación de la aplicación de Azure siguiente, que proporciona artículos de inicio rápido, tutoriales y ejemplos.

- [Nociones sobre las plantillas de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)
- Guías de inicio rápido:

  - [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/documentation/templates/)
  - [Plantillas de inicio rápido de Azure en GitHub](https://github.com/azure/azure-quickstart-templates)
  - [Publicación de definición de aplicación](https://docs.microsoft.com/azure/managed-applications/publish-service-catalog-app)
  - [Implementación de la aplicación de catálogo de servicios](https://docs.microsoft.com/azure/managed-applications/deploy-service-catalog-quickstart)

  
- Tutoriales:

  - [Creación de archivos de definición](https://docs.microsoft.com/azure/managed-applications/publish-service-catalog-app)
  - [Publicación de la aplicación Marketplace](https://docs.microsoft.com/azure/managed-applications/publish-marketplace-app)

  - Ejemplos:

    - [CLI de Azure](https://docs.microsoft.com/azure/managed-applications/cli-samples)
    - [Azure PowerShell](https://docs.microsoft.com/azure/managed-applications/powershell-samples)
    - [Soluciones para aplicaciones administradas](https://docs.microsoft.com/azure/managed-applications/sample-projects)

## <a name="fundamental-technical-knowledge"></a>Conocimientos técnicos básicos

El diseño, la compilación y las pruebas de estos recursos lleva tiempo y requiere conocimientos técnicos de la plataforma de Azure y las tecnologías que se usan para crear la oferta.

El equipo de ingeniería debe tener conocimientos sobre las tecnologías de Microsoft siguientes:

- Conocimientos básicos de los [Servicios de Azure](https://azure.microsoft.com/services/).
- Cómo [diseñar y estructurar las aplicaciones de Azure](https://azure.microsoft.com/solutions/architecture/).
- Conocimientos prácticos de [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/), [Azure Storage](https://azure.microsoft.com/services/?filter=storage) y [redes de Azure](https://azure.microsoft.com/services/?filter=networking).
- Conocimientos prácticos de [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/).
- Conocimientos prácticos de [JSON](https://www.json.org/)

## <a name="suggested-tools"></a>Herramientas sugeridas

Elija uno o ambos de los entornos de scripting siguientes para ayudar a administrar la aplicación de Azure:

- [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
- [CLI de Azure](https://docs.microsoft.com/cli/azure)

Se recomienda agregar las herramientas siguientes al entorno de desarrollo:

- [Explorador de Azure Storage](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
- [Visual Studio Code](https://code.visualstudio.com/) con las siguientes extensiones:

  - Extensión: [Herramientas de Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
  - Extensión: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
  - Extensión: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

También se recomienda revisar las herramientas disponibles en la página [Herramientas de desarrollo de Azure](https://azure.microsoft.com/tools/) y, si usa Visual Studio, en [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

## <a name="next-steps"></a>Pasos siguientes

[Creación de una oferta de aplicación de Azure](./cpp-create-offer.md)

