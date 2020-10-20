---
author: ramonarguelles
ms.service: spatial-anchors
ms.topic: include
ms.date: 1/30/2019
ms.author: rgarcia
ms.openlocfilehash: 0e6f68daf22659b880827ad027268d9757c60063
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91971432"
---
### <a name="open-the-publish-wizard"></a>Apertura del Asistente para publicación

En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **SharingService** y seleccione **Publicar**.

Se inicia el Asistente para publicación. 

Seleccione **App Service** > **Publicar** para abrir el panel **Crear servicio de aplicaciones**.

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal.

En el panel **Crear servicio de aplicaciones**, seleccione **Agregar una cuenta** e inicie sesión en su suscripción de Azure. Si ya ha iniciado sesión, seleccione la cuenta que desee en la lista desplegable.

   > [!NOTE]
   > Si ya ha iniciado sesión, no seleccione **Crear** todavía.
   >

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

[!INCLUDE [resource group intro text](resource-group.md)]

Junto a **Grupo de recursos**, seleccione **Nuevo**.

Asígnele el nombre **myResourceGroup** al grupo de recursos y seleccione **Aceptar**.

### <a name="create-an-app-service-plan"></a>Creación de un plan de App Service

[!INCLUDE [app-service-plan](app-service-plan.md)]

Junto a **Plan de hospedaje**, seleccione **Nuevo**.

En el panel **Configurar un plan de hospedaje**, use estos valores:

| Parámetro | Valor sugerido | Descripción |
|-|-|-|
|Plan de App Service| MySharingServicePlan | Nombre del plan de App Service |
| Location | Oeste de EE. UU. | El centro de datos donde se hospeda la aplicación web. |
| Size | Gratuito | El [plan de tarifa](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) que determina las características de hospedaje. |

Seleccione **Aceptar**.

### <a name="create-and-publish-the-web-app"></a>Creación y publicación de la aplicación web

En **Nombre de la aplicación**, escriba un nombre único para la aplicación. Los caracteres válidos son a-z, 0-9 y guiones; también puede aceptar el nombre único generado automáticamente. La dirección URL de la aplicación web es `https://<app_name>.azurewebsites.net`, donde `<app_name>` es el nombre de la aplicación.

Seleccione **Crear** para comenzar a crear los recursos de Azure.

   Cuando el asistente finaliza, publica la aplicación web ASP.NET Core en Azure y la inicia en su explorador predeterminado.

  ![Captura de pantalla de una aplicación web de ASP.NET publicada en Azure.](./media/spatial-anchors-azure/web-app-running-live.png)

El nombre de la aplicación que ha utilizado en esta sección se usa como prefijo de dirección URL con el formato `https://<app_name>.azurewebsites.net`. Copie esta la dirección URL en un editor de texto para utilizarla posteriormente.
