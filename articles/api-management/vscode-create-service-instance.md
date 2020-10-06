---
title: Creación de una instancia de Azure API Management con Visual Studio Code | Microsoft Docs
description: Uso de Visual Studio Code para crear una instancia de Azure API Management.
ms.service: api-management
ms.workload: integration
author: vladvino
ms.author: apimpm
ms.topic: quickstart
ms.date: 09/14/2020
ms.openlocfilehash: 19080679291b88b693c95bd71f8ddc0e59286356
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2020
ms.locfileid: "90057361"
---
# <a name="quickstart-create-a-new-azure-api-management-service-instance-using-visual-studio-code"></a>Inicio rápido: Creación de una nueva instancia de servicio de Azure API Management mediante Visual Studio Code

Azure API Management (APIM) ayuda a las organizaciones a publicar API para desarrolladores externos, asociados e internos a fin de desbloquear el potencial de sus datos y servicios. API Management proporciona las competencias esenciales para garantizar un programa de API de éxito mediante compromisos con desarrolladores, información detallada empresarial, análisis, seguridad y protección. APIM le permite crear y administrar modernas puertas de enlace de API para los servicios back-end existentes hospedados en cualquier lugar. Para obtener más información, consulte el tema de [introducción](api-management-key-concepts.md).

Este inicio rápido describe los pasos que deben seguirse para crear una nueva instancia de API Management mediante la *versión preliminar de la extensión de Azure API Management* para Visual Studio Code. También puede usar la extensión para realizar operaciones comunes de administración en la instancia de API Management.

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Además, asegúrese de que tenga instalado lo siguiente:

- [Visual Studio Code](https://code.visualstudio.com/)

- [Extensión de Azure API Management para Visual Studio Code (versión preliminar)](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-apimanagement&ssr=false#overview)

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie Visual Studio Code y abra la extensión de Azure. (Si no ve el icono de Azure en la barra de actividades, asegúrese de que esté habilitada la extensión de *Azure API Management*).

Seleccione **Iniciar sesión en Azure...** para iniciar una ventana del explorador e iniciar sesión en su cuenta de Microsoft.

![Inicie sesión en Azure desde la extensión de API Management para VS Code](./media/vscode-create-service-instance/vscode-apim-login.png)

## <a name="create-an-api-management-service"></a>Creación de un servicio de API Management

Una vez que haya iniciado sesión en la cuenta de Microsoft, el panel del explorador *Azure: API Management* mostrará sus suscripciones de Azure.

Haga clic con el botón derecho en la suscripción que desea usar y seleccione **Create API Management in Azure** (Crear una instancia de API Management en Azure).

![Asistente de creación de una instancia de API Management en VS Code](./media/vscode-create-service-instance/vscode-apim-create.png)

En el panel que se abre, escriba un nombre para la nueva instancia de API Management. Debe ser único globalmente dentro de Azure y constar de 1 a 50 caracteres alfanuméricos o guiones, y empezar por una letra y terminar por un carácter alfanumérico.

Se creará una nueva instancia de API Management (y un grupo de recursos primario) con el nombre especificado. De forma predeterminada, la instancia se crea en la región *Oeste de EE. UU.* con la SKU *Consumo*.

> [!TIP]
> Si habilita **Advanced Creation** (Creación avanzada) en la *configuración de la extensión Azure API Management*, podrá también especificar una [SKU de API Management](https://azure.microsoft.com/pricing/details/api-management/), una [región de Azure](https://status.azure.com/en-us/status) y un [grupo de recursos](../azure-resource-manager/management/overview.md) para implementar la instancia de API Management.
>
> Aunque la SKU *Consumo* tarda menos de un minuto en aprovisionarse, otras SKU normalmente tardan de 30 a 40 minutos en crearse.

En este momento, está listo para importar y publicar su primera API. Puede hacer eso y realizar también operaciones de API Management comunes dentro de la extensión para Visual Studio Code. Consulte la documentación sobre la [extensión Azure API Management para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-apimanagement&ssr=false#overview) para más información.

![Instancia de API Management recién creada en el panel de la extensión API Management para VS Code](./media/vscode-create-service-instance/vscode-apim-instance.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no la necesite, elimine la instancia de API Management; para ello, haga clic con el botón derecho y seleccione **Abrir en el portal** para [eliminar el servicio API Management](get-started-create-service-instance.md#clean-up-resources) y su grupo de recursos.

O bien, puede seleccionar **Eliminar API Management** para eliminar solo la instancia de API Management (esta operación no elimina su grupo de recursos).

![Eliminación de la instancia de API Management de VS Code](./media/vscode-create-service-instance/vscode-apim-delete.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Importación y publicación de la primera API](import-and-publish.md)
