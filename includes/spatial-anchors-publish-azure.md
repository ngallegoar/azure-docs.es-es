---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 0dab71b6d169e26a3d7dc208dd09efe1143fbe13
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "67186004"
---
### <a name="open-the-publish-wizard"></a>Apertura del Asistente para publicación

En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **SharingService** y seleccione **Publicar**.

Se inicia el Asistente para publicación. Seleccione **App Service** > **Publicar** para abrir el cuadro de diálogo **Crear servicio de aplicaciones**.

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

En el cuadro de diálogo **Crear servicio de aplicaciones** , seleccione **Agregar una cuenta** e inicie sesión en su suscripción de Azure. Si ya ha iniciado sesión, seleccione la cuenta que desee en la lista desplegable.

> [!NOTE]
> Si ya ha iniciado sesión, no seleccione **Crear** todavía.
>

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

[!INCLUDE [resource group intro text](resource-group.md)]

Junto a **Grupo de recursos**, seleccione **Nuevo**.

Asigne el nombre **myResourceGroup** al grupo de recursos y seleccione **Aceptar**.

### <a name="create-an-app-service-plan"></a>Creación de un plan de App Service

[!INCLUDE [app-service-plan](app-service-plan.md)]

Junto a **Plan de hospedaje**, seleccione **Nuevo**.

En el cuadro de diálogo **Configurar un plan de hospedaje**, use estos valores:

| Configuración | Valor sugerido | Descripción |
|-|-|-|
|Plan de servicio de aplicación| MySharingServicePlan | Nombre del plan de App Service. |
| Location | Oeste de EE. UU. | El centro de datos donde se hospeda la aplicación web. |
| Size | Gratuito | El [plan de tarifa](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) que determina las características de hospedaje. |

Seleccione **Aceptar**.

### <a name="create-and-publish-the-web-app"></a>Creación y publicación de la aplicación web

En **Nombre de la aplicación**, escriba un nombre único de aplicación (los caracteres válidos son `a-z`, `0-9` y `-`) o acepte el nombre único generado automáticamente. La dirección URL de la aplicación web es `https://<app_name>.azurewebsites.net`, donde `<app_name>` es el nombre de la aplicación.

Seleccione **Crear** para comenzar a crear los recursos de Azure.

Cuando el asistente finaliza, publica la aplicación web ASP.NET Core en Azure y la inicia en su explorador predeterminado.

![Aplicación web de ASP.NET publicada en Azure](./media/spatial-anchors-azure/web-app-running-live.png)

El nombre de la aplicación que ha utilizado en esta sección se usa como prefijo de dirección URL con el formato `https://<app_name>.azurewebsites.net`. Tome nota de esta dirección URL, ya que necesitará.
