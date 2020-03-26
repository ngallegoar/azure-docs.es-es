---
title: 'Tutorial: Implementación desde un registro con replicación geográfica'
description: Implemente una aplicación web basada en Linux en dos regiones de Azure diferentes con una imagen de contenedor de un registro de contenedor de Azure con replicación geográfica. Segunda parte de una serie de tres partes.
ms.topic: tutorial
ms.date: 08/20/2018
ms.custom: seodec18, mvc
ms.openlocfilehash: 5b075e1065ef8c30837000f490cc93525b4b61cc
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "74456111"
---
# <a name="tutorial-deploy-a-web-app-from-a-geo-replicated-azure-container-registry"></a>Tutorial: Implementación de una aplicación web de un registro de contenedor de Azure con replicación geográfica

Esta es la segunda parte de una serie de tutoriales de tres partes. En la [primera parte](container-registry-tutorial-prepare-registry.md), se creó un registro de contenedor privado con replicación geográfica y se compiló una imagen de contenedor a partir del origen, que se insertó en el registro. En este artículo, se implementa el contenedor en dos instancias de Web App en dos regiones de Azure diferentes para aprovechar el aspecto cercano a la red del registro con replicación geográfica. Cada instancia extrae la imagen del contenedor del registro más cercano.

En este tutorial, segunda parte de la serie, se realizan las siguientes operaciones:

> [!div class="checklist"]
> * Implementar una imagen de contenedor en dos instancias de *Web App for Containers*.
> * Comprobar la aplicación implementada.

Si aún no ha creado un registro con replicación geográfica e insertado la imagen de la aplicación de ejemplo en contenedor en el registro, vuelva al tutorial anterior de la serie, [Preparar un registro de contenedor de Azure con replicación geográfica](container-registry-tutorial-prepare-registry.md).

En el siguiente artículo de la serie, actualizará la aplicación y, a continuación, insertará una nueva imagen de contenedor actualizada en el registro. Por último, examinará cada una de las instancias de la aplicación web en ejecución para ver el cambio reflejado automáticamente en ambas, para mostrar la replicación geográfica de Azure Container Registry y los webhooks en acción.

## <a name="automatic-deployment-to-web-apps-for-containers"></a>Implementación automática en Web App for Containers

Azure Container Registry proporciona compatibilidad para implementar aplicaciones en contenedor directamente en [Web Apps for Containers](../app-service/containers/index.yml). En este tutorial, usará Azure Portal para implementar la imagen de contenedor que se creó en el tutorial anterior en dos planes de aplicaciones web ubicados en distintas regiones de Azure.

Si implementa una aplicación web a partir de una imagen de contenedor en el registro y tiene un registro con replicación geográfica en la misma región, Azure Container Registry crea un [webhook](container-registry-webhook.md) de implementación de imagen para usted. Cuando se inserta una nueva imagen en el repositorio de contenedor, el webhook captura el cambio e implementa automáticamente la nueva imagen de contenedor en la aplicación web.

## <a name="deploy-a-web-app-for-containers-instance"></a>Implementación de una instancia de Web App for Containers

En este paso, creará una instancia de Web App for Containers en la región *Oeste de EE. UU*.

Inicie sesión en [Azure Portal](https://portal.azure.com) y navegue hasta el registro que creó en el tutorial anterior.

Seleccione **Repositorios** > **acr-helloworld**, haga clic con el botón derecho en la etiqueta **v1** en **Etiquetas** y seleccione **Deploy to web app** (Implementar en aplicación web):

![Implementar en App Service en Azure Portal][deploy-app-portal-01]

Si la opción "Implementar en aplicación web" está deshabilitada, es posible que no haya habilitado el usuario de administrador del registro, tal como se indicaba en [Creación de un registro de contenedor](container-registry-tutorial-prepare-registry.md#create-a-container-registry) en el primer tutorial. El usuario administrador se puede habilitar en **Configuración** > **Claves de acceso** en Azure Portal.

En el cuadro **Web App for Containers** que aparece al seleccionar "Implementar en aplicación web", especifique los siguientes valores en cada ajuste:

| Configuración | Value |
|---|---|
| **Nombre del sitio** | Nombre único global para la aplicación web. En este ejemplo, se usa el formato `<acrName>-westus` para identificar fácilmente el registro y la región desde los que se implementó la aplicación web. |
| **Grupo de recursos** | **Usar existente** > `myResourceGroup` |
| **Plan de App Service/Ubicación** | Cree un nuevo plan denominado `plan-westus` en la región **Oeste de EE. UU**. |
| **Imagen** | `acr-helloworld:v1` |
| **Sistema operativo** | Linux |

> [!NOTE]
> Cuando se crea un plan de App Service para implementar una aplicación en contenedor, se selecciona automáticamente un plan predeterminado para hospedar la aplicación. El plan predeterminado depende de la configuración del sistema operativo.

Seleccione **Crear** para aprovisionar la aplicación web en la región *Oeste de EE. UU*.

![Configuración de Web app on Linux en Azure Portal][deploy-app-portal-02]

## <a name="view-the-deployed-web-app"></a>Visualización de la aplicación web implementada

Al finalizar la implementación, puede navegar a la URL de la aplicación en ejecución en el explorador para verla.

En el portal, seleccione **App Services** y, a continuación, la aplicación web que aprovisionó en el paso anterior. En este ejemplo, la aplicación web se denomina *uniqueregistryname-westus*.

Seleccione la dirección URL con hipervínculo de la aplicación web en la parte superior derecha de la sección de información general de **App Service** para ver la aplicación en ejecución en el explorador.

![Configuración de Web app on Linux en Azure Portal][deploy-app-portal-04]

Una vez implementada la imagen de Docker desde el registro de contenedor con replicación geográfica, el sitio muestra una imagen que representa la región de Azure que hospeda el registro de contenedor.

![Visualización de la aplicación web implementada en un explorador][deployed-app-westus]

## <a name="deploy-second-web-app-for-containers-instance"></a>Implementación de una segunda instancia de Web App for Containers

Use el procedimiento que se describe en la sección anterior para implementar una segunda aplicación web para la región *Este de EE. UU*. Debajo de **Web App for Containers**, especifique los siguientes valores:

| Configuración | Value |
|---|---|
| **Nombre del sitio** | Nombre único global para la aplicación web. En este ejemplo, se usa el formato `<acrName>-eastus` para identificar fácilmente el registro y la región desde los que se implementó la aplicación web. |
| **Grupo de recursos** | **Usar existente** > `myResourceGroup` |
| **Plan de App Service/Ubicación** | Cree un nuevo plan denominado `plan-eastus` en la región **Este de EE. UU**. |
| **Imagen** | `acr-helloworld:v1` |
| **Sistema operativo** | Linux |

Seleccione **Crear** para aprovisionar la aplicación web en la región *Este de EE. UU*.

![Configuración de Web app on Linux en Azure Portal][deploy-app-portal-06]

## <a name="view-the-deployed-web-app"></a>Visualización de la aplicación web implementada

Igual que antes, puede navegar a la URL de la aplicación en ejecución en el explorador para verla.

En el portal, seleccione **App Services** y, a continuación, la aplicación web que aprovisionó en el paso anterior. En este ejemplo, la aplicación web se denomina *uniqueregistryname-eastus*.

Seleccione la dirección URL con hipervínculo de la aplicación web en la parte superior derecha de **Introducción de App Service** para ver la aplicación en ejecución en el explorador.

![Configuración de Web app on Linux en Azure Portal][deploy-app-portal-07]

Una vez implementada la imagen de Docker desde el registro de contenedor con replicación geográfica, el sitio muestra una imagen que representa la región de Azure que hospeda el registro de contenedor.

![Visualización de la aplicación web implementada en un explorador][deployed-app-eastus]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, se implementaron dos instancias de Web App for Containers desde un registro de contenedor de Azure con replicación geográfica.

Pase al siguiente tutorial para actualizar e implementar una nueva imagen de contenedor en el registro de contenedor y, a continuación, compruebe que las aplicaciones web que se ejecutan en ambas regiones se actualizaron automáticamente.

> [!div class="nextstepaction"]
> [Implementar una actualización en la imagen de contenedor con replicación geográfica](./container-registry-tutorial-deploy-update.md)

<!-- IMAGES -->
[deploy-app-portal-01]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-01.png
[deploy-app-portal-02]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-02.png
[deploy-app-portal-03]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-03.png
[deploy-app-portal-04]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-04.png
[deploy-app-portal-05]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-05.png
[deploy-app-portal-06]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-06.png
[deploy-app-portal-07]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-07.png
[deployed-app-westus]: ./media/container-registry-tutorial-deploy-app/deployed-app-westus.png
[deployed-app-eastus]: ./media/container-registry-tutorial-deploy-app/deployed-app-eastus.png