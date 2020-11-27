---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/30/2020
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: ef617b9ed6c35d33350b173efe6189b5b79b6ae6
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96008244"
---
1. En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Publicar**.

1. En **Destino**, seleccione **Azure** :::image type="content" source="media/functions-vstools-publish/functions-visual-studio-publish-profile-step-1.png" alt-text="Seleccionar destino de Azure":::

1. En **Destino específico**, seleccione **Azure Function App (Windows)** .

    :::image type="content" source="media/functions-vstools-publish/functions-visual-studio-publish-profile-step-2.png" alt-text="Seleccionar Azure Function App":::

1. En **Instancia de Functions**, seleccione **Crear una instancia de Azure Functions...** y, después, use los valores especificados en la tabla siguiente:

    | Configuración      | Value  | Descripción                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nombre** | Nombre único globalmente | Nombre que identifica de forma única la nueva aplicación de función. Acepte este nombre o escriba uno nuevo. Los caracteres válidos son `a-z`, `0-9` y `-`. |
    | **Suscripción** | Su suscripción | La suscripción de Azure que se va a usar. Acepte esta suscripción o seleccione una nueva en la lista desplegable. |
    | **[Grupo de recursos](../articles/azure-resource-manager/management/overview.md)** | Nombre del grupo de recursos |  Nombre del grupo de recursos en el que se va a crear la aplicación de función. Seleccione un grupo de recursos existente en la lista desplegable o elija la opción **Nuevo** para crear un nuevo grupo de recursos.|
    | **[Tipo de plan](../articles/azure-functions/functions-scale.md)** | Consumo | Cuando publique el proyecto en una aplicación de funciones que se ejecute en un [plan Consumo](../articles/azure-functions/functions-scale.md#consumption-plan), solo pagará por las ejecuciones de la aplicación. Otros planes de hospedaje suponen costos más elevados. |
    | **Ubicación** | Ubicación de la instancia de App Service | Elija una **ubicación** en una [región](https://azure.microsoft.com/regions/) próxima a usted o a otros servicios a los que las funciones accedan. |
    | **[Azure Storage](../articles/storage/common/storage-account-create.md)** | Cuenta de almacenamiento de uso general | El entorno de ejecución de Functions necesita una cuenta de Azure Storage. Seleccione **Nueva** para configurar una cuenta de almacenamiento de uso general. También puede elegir una cuenta existente que cumpla los [requisitos de la cuenta de almacenamiento](../articles/azure-functions/functions-scale.md#storage-account-requirements).  |

    ![Cuadro de diálogo Crear servicio de aplicaciones](./media/functions-vstools-publish/functions-visual-studio-publish.png)

1. Seleccione **Crear** para crear una aplicación de funciones y sus recursos relacionados en Azure. 
1. En la **instancia de Functions**, asegúrese de que **Ejecutar desde archivo de paquete** está activada. La aplicación de funciones se implementa con la [implementación de un archivo zip](../articles/azure-functions/functions-deployment-technologies.md#zip-deploy) y con el modo de [ejecución desde el paquete](../articles/azure-functions/run-functions-from-deployment-package.md) habilitado. Este es el método de implementación recomendado para el proyecto de Functions, ya que se obtiene un mejor rendimiento. 

    :::image type="content" source="media/functions-vstools-publish/functions-visual-studio-publish-profile-step-4.png" alt-text="Finalizar la creación del perfil":::

1. Seleccione **Finalizar** y, en la página Publicar, seleccione **Publicar** para implementar el paquete que contiene los archivos de proyecto en la nueva aplicación de funciones en Azure. 

    Una vez finalizada la implementación, la dirección URL raíz de la aplicación de funciones en Azure se muestra en la pestaña **Publicar**. 
    
1.  En la pestaña Publicar, elija **Administrar en Cloud Explorer**. El nuevo recurso de Azure de la aplicación de funciones se abrirá en Cloud Explorer. 
    
    :::image type="content" source="media/functions-vstools-publish/functions-visual-studio-publish-complete.png" alt-text="Publicación de un mensaje de operación correcta":::
    
    Cloud Explorer le permite usar Visual Studio para ver el contenido del sitio, iniciar y detener la aplicación de funciones, y desplazarse directamente a los recursos de la aplicación de funciones en Azure y en Azure Portal. 
