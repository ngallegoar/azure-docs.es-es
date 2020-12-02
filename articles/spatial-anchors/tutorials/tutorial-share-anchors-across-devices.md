---
title: 'Tutorial: Uso compartido de delimitadores entre sesiones y dispositivos'
description: En este tutorial, obtendrá información sobre cómo compartir los identificadores espaciales de Azure Spatial Anchors entre dispositivos Android/iOS de Unity con un servicio de back-end.
author: msftradford
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: parkerra
ms.date: 11/20/2020
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 3048eb1ca7f9312a43bc1ab5885bf19d4c2e7931
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96185464"
---
# <a name="tutorial-share-spatial-anchors-across-sessions-and-devices"></a>Tutorial: Uso compartido de Spatial Anchors entre sesiones y dispositivos

Azure Spatial Anchors es un servicio multiplataforma para desarrolladores que permite crear experiencias de realidad mixta mediante objetos cuya ubicación persiste en todos los dispositivos a lo largo del tiempo. 

En este tutorial se usa [Azure Spatial Anchors](../overview.md) para crear delimitadores durante una sesión y, a continuación, buscarlos en el mismo dispositivo o en otro. Estos mismos delimitadores los pueden encontrar varios dispositivos en el mismo lugar y al mismo tiempo.

![La animación muestra delimitadores espaciales con un dispositivo móvil que se utilizan con un dispositivo diferente según pasan los días.](./media/persistence.gif)


En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Implementar una aplicación web de ASP.NET Core en Azure para compartir delimitadores y almacenarlos en memoria durante un tiempo determinado.
> * Configurar la escena AzureSpatialAnchorsLocalSharedde demostración en el ejemplo de Unity de nuestros inicios rápidos para aprovechar las ventajas de la aplicación web de uso compartido de delimitadores.
> * Implementar y ejecutar los delimitadores en uno o varios dispositivos.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [Share Anchors Sample Prerequisites](../../../includes/spatial-anchors-share-sample-prereqs.md)]

> [!NOTE]
> Aunque se va a usar Unity y una aplicación web de ASP.NET Core en este tutorial, es solo para mostrar un ejemplo sobre cómo compartir los identificadores de Azure Spatial Anchors con otros dispositivos. Se pueden usar otros lenguajes y tecnologías de back-end para lograr el mismo objetivo.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-the-sample-project"></a>Descarga del proyecto de ejemplo

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

## <a name="deploy-the-sharing-anchors-service"></a>Implementación del servicio de delimitadores de uso compartido

## <a name="visual-studio"></a>[Visual Studio](#tab/VS)

Abra Visual Studio y el proyecto de la carpeta *Sharing\SharingServiceSample*.

[!INCLUDE [Publish Azure](../../../includes/spatial-anchors-publish-azure.md)]

## <a name="visual-studio-code"></a>Tener [Visual Studio Code](#tab/VSC)

Tendrá que crear un grupo de recursos y un plan de App Service antes de implementar el servicio en Visual Studio Code.

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Vaya a <a href="https://portal.azure.com/" target="_blank">Azure Portal</a> e inicie sesión en la suscripción de Azure.

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

[!INCLUDE [resource group intro text](../../../includes/resource-group.md)]

Junto a **Grupo de recursos**, seleccione **Nuevo**.

Asígnele el nombre **myResourceGroup** al grupo de recursos y seleccione **Aceptar**.

### <a name="create-an-app-service-plan"></a>Creación de un plan de App Service

[!INCLUDE [app-service-plan](../../../includes/app-service-plan.md)]

Junto a **Plan de hospedaje**, seleccione **Nuevo**.

En el panel **Configurar un plan de hospedaje**, use estos valores:

| Parámetro | Valor sugerido | Descripción |
|-|-|-|
|Plan de App Service| MySharingServicePlan | Nombre del plan de App Service |
| Location | Oeste de EE. UU. | El centro de datos donde se hospeda la aplicación web. |
| Size | Gratuito | El [plan de tarifa](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) que determina las características de hospedaje. |

Seleccione **Aceptar**.

Abra Visual Studio Code y el proyecto de la carpeta *Sharing\SharingServiceSample*. 

Para implementar el servicio de uso compartido mediante Visual Studio Code, siga las instrucciones de <a href="/aspnet/core/tutorials/publish-to-azure-webapp-using-vscode?view=aspnetcore-2.2#open-it-with-visual-studio-code" target="_blank">Publicación de una aplicación de ASP.NET Core en Azure con Visual Studio Code</a>. Comience en la sección "Abrirlo con Visual Studio Code". Tal como se explicó en el paso anterior, no cree otro proyecto de ASP.NET, porque ya tiene uno para implementarlo y publicarlo: SharingServiceSample.

---

## <a name="deploy-the-sample-app"></a>Implementación de la aplicación de ejemplo

[!INCLUDE [Run Share Anchors Sample](../../../includes/spatial-anchors-run-share-sample.md)]

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha implementado una aplicación web de ASP.NET Core en Azure y ha configurado e implementado una aplicación de Unity. Ha creado delimitadores espaciales con la aplicación y los ha compartido con otros dispositivos mediante la aplicación web de ASP.NET Core.

Puede mejorar la aplicación web de ASP.NET Core para que use Azure Cosmos DB y que el almacenamiento de los identificadores de delimitador espacial compartidos persista. Al agregar compatibilidad con Azure Cosmos DB, puede crear hoy mismo un delimitador con la aplicación web de ASP.NET Core. Después, mediante el identificador de delimitador almacenado en la aplicación web, puede hacer que la aplicación vuelva a encontrar el delimitador días más tarde.

> [!div class="nextstepaction"]
> [Uso de Azure Cosmos DB para almacenar delimitadores](./tutorial-use-cosmos-db-to-store-anchors.md)