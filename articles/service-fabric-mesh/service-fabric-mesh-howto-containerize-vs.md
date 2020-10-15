---
title: Inclusión de una aplicación .NET existente en un contenedor para Service Fabric Mesh
description: Agregue compatibilidad con la orquestación de contenedores de Service Fabric Mesh a ASP.NET y a proyectos de consola que usen el marco .NET completo.
author: georgewallace
ms.author: gwallace
ms.date: 11/08/2018
ms.topic: conceptual
ms.openlocfilehash: a995f30872216a8b704d3d1714bbece4bb8271f8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91840071"
---
# <a name="containerize-an-existing-net-app-for-service-fabric-mesh"></a>Inclusión de una aplicación .NET existente en un contenedor para Service Fabric Mesh

En este artículo se muestra cómo agregar compatibilidad con la orquestación de contenedores de Service Fabric Mesh a una aplicación .NET existente.

En Visual Studio 2017, puede agregar compatibilidad con la inclusión de los proyectos ASP.NET y de consola en un contenedor que utilizan la plataforma .NET completa.

> [!NOTE]
> Actualmente no se admiten los proyectos .NET **Core**.

## <a name="prerequisites"></a>Prerrequisitos

* Si no tiene una suscripción a Azure, puede [crear una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

* Asegúrese de haber [configurado el entorno de desarrollo](service-fabric-mesh-howto-setup-developer-environment-sdk.md). Esto incluye la instalación del entorno de ejecución de Service Fabric, SDK, Docker, Visual Studio 2017 y la creación de un clúster local.

## <a name="open-an-existing-net-app"></a>Apertura de una aplicación .NET existente

Abra la aplicación a la que desea agregar la compatibilidad con la orquestación de contenedores.

Si desea probar un ejemplo, puede utilizar el ejemplo de código [eShop](https://github.com/MikkelHegn/ContainersSFLab). En el resto de este artículo se asumirá que estamos usando ese proyecto, aunque puede aplicar estos pasos a su propio proyecto.

Obtenga una copia del proyecto **eShop**:

```git
git clone https://github.com/MikkelHegn/ContainersSFLab.git
```

Una vez descargado, en Visual Studio 2017 abra **ContainersSFLab\eShopLegacyWebFormsSolution\eShopLegacyWebForms.sln**.

## <a name="add-container-support"></a>Adición de compatibilidad con contenedores
 
Agregue compatibilidad con la orquestación de contenedores a un proyecto ASP.NET o de consola existente mediante las herramientas de Service Fabric Mesh de la siguiente manera:

En el explorador de soluciones de Visual Studio, haga clic con el botón derecho en el nombre del proyecto (en el ejemplo, **eShopLegacyWebForms**) y, después, elija **Agregar** > **Compatibilidad con el orquestador de contenedores**.
Se muestra el cuadro de diálogo **Agregar compatibilidad con el orquestador de contenedores**.

![Cuadro de diálogo Agregar orquestador de contenedores de Visual Studio](./media/service-fabric-mesh-howto-containerize-vs/add-container-orchestration-support.png)

Seleccione **Service Fabric Mesh** en el menú desplegable y, a continuación, haga clic en **Aceptar**.

A continuación, la herramienta comprueba que el Docker está instalado, agrega Dockerfile al proyecto y extrae una imagen Docker para el proyecto.  
Se agrega a la solución un proyecto de aplicación de Service Fabric Mesh. Contiene sus perfiles de publicación y archivos de configuración de Mesh. El nombre del proyecto es el mismo que el nombre de su proyecto, con "Application" concatenado al final, por ejemplo,  **eShopLegacyWebFormsApplication**. 

En el nuevo proyecto Mesh se pueden ver dos carpetas que debe tener en cuenta:
- **App Resources** (Recursos de aplicación) que contiene archivos YAML que describen recursos Mesh adicionales como la red.
- **Service Resources** (Recursos de servicio) que contiene un archivo service.yaml que describe cómo debe ejecutarse la aplicación cuando se implementa.

Una vez que la compatibilidad con la orquestación de contenedores se agregue a la aplicación, puede presionar **F5** para depurar su aplicación .NET en su clúster local de Service Fabric Mesh. Esta es la aplicación eShop ASP.NET que se ejecuta en un clúster de Service Fabric Mesh: 

![Aplicación eShop](./media/service-fabric-mesh-howto-containerize-vs/eshop-running.png)

Ahora puede publicar la aplicación en Azure Service Fabric Mesh.

## <a name="next-steps"></a>Pasos siguientes

Consulte cómo publicar una aplicación en Service Fabric Mesh: [Tutorial: Implementación de una aplicación de Service Fabric Mesh](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)