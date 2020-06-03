---
title: Implementación de un modelo en Azure Analysis Services mediante Visual Studio | Microsoft Docs
description: Aprenda a implementar un modelo tabular en un servidor de Azure Analysis Services mediante Visual Studio.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: d6b737985bc19aa09b26bb0d4a65696a364a903a
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83697594"
---
# <a name="deploy-a-model-from-visual-studio"></a>Implementación de un modelo de Visual Studio

Una vez que ha creado un servidor en su suscripción de Azure, está listo para implementar una base de datos de modelo tabular en él. Puede usar Visual Studio con proyectos de Analysis Services para compilar e implementar un proyecto de modelo tabular en el que está trabajando. 

## <a name="prerequisites"></a>Prerrequisitos

Para empezar, necesitará lo siguiente:

* **Servidor de Analysis Services** en Azure. Para más información, consulte [Creación de un servidor de Azure Analysis Services en Azure Portal](analysis-services-create-server.md).
* **Proyecto de modelo tabular** en Visual Studio o un modelo tabular existente en el nivel de compatibilidad 1200 o superior. ¿Nunca ha creado ninguno? Pruebe el [tutorial de modelado tabular de ventas en Internet de Adventure Works](https://docs.microsoft.com/analysis-services/tutorial-tabular-1400/as-adventure-works-tutorial).
* **Puerta de enlace local**: si uno o varios orígenes de datos son locales en la red de su organización, debe instalar una [puerta de enlace de datos local](analysis-services-gateway.md). La puerta de enlace es necesaria para que el servidor en la nube se conecte a sus orígenes de datos locales para procesar y actualizar los datos en el modelo.

> [!TIP]
> Antes de implementarlo, asegúrese de que puede procesar los datos de las tablas. En Visual Studio, haga clic en **Modelo** > **Proceso** > **Procesar todo**. Si se produce un error de procesamiento, no se implementará correctamente.
> 
> 

## <a name="get-the-server-name"></a>Obtención del nombre del servidor

En **Azure Portal** > servidor > **Información general** > **Nombre de servidor**, copie el nombre del servidor.
   
![Obtención del nombre del servidor en Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

## <a name="to-deploy-from-visual-studio"></a>Implementación desde Visual Studio

1. En Visual Studio > **Explorador de soluciones**, haga clic con el botón derecho en el proyecto > **Propiedades**. A continuación, en **Implementación** > **Servidor**, pegue el nombre del servidor.   
   
    ![Pegar el nombre del servidor en la propiedad del servidor de implementación](./media/analysis-services-deploy/aas-deploy-deployment-server-property.png)
2. En el **Explorador de soluciones**, haga clic con el botón derecho en **Propiedades** y haga clic en **Implementar**. Es posible que se le pida que inicie sesión en Azure.
   
    ![Implementación en el servidor](./media/analysis-services-deploy/aas-deploy-deploy.png)
   
    El estado de la implementación aparece en la ventana Salida y en Implementar.
   
    ![Estado de implementación](./media/analysis-services-deploy/aas-deploy-status.png)

Eso es todo.


## <a name="troubleshooting"></a>Solución de problemas

Si se produce un error al implementar los metadatos, es probable que sea porque Visual Studio no se pudo conectar al servidor. Asegúrese de que puede conectarse al servidor mediante SQL Server Management Studio (SSMS). A continuación, asegúrese de que la propiedad de Servidor de implementación del proyecto es correcta.

Si se produce un error al realizar una implementación en una tabla, es probable que sea porque el servidor no pudo conectarse a un origen de datos. Si el origen de datos es local en la red de su organización, asegúrese de instalar una [puerta de enlace de datos local](analysis-services-gateway.md).

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene el modelo tabular implementado en el servidor, está listo para conectarse a él. Puede [conectarse a él con SQL Server Management Studio (SSMS)](analysis-services-manage.md) para administrarlo. Además, puede [conectarse a él mediante una herramienta cliente](analysis-services-connect.md) como Power BI, Power BI Desktop o Excel y empezar a crear informes.   

Para obtener información sobre métodos de implementación avanzada, consulte [Implementación de la solución de modelo tabular](https://docs.microsoft.com/analysis-services/deployment/tabular-model-solution-deployment?view=azure-analysis-services-current).



