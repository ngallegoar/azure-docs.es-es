---
title: Implementación continua para Azure Functions
description: Use las características de implementación continua de Azure App Service para publicar las funciones.
ms.assetid: 361daf37-598c-4703-8d78-c77dbef91643
ms.topic: conceptual
ms.date: 09/25/2019
ms.openlocfilehash: cc1e100a0c2e652ab081869409fd24dbf88017a3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79235024"
---
# <a name="continuous-deployment-for-azure-functions"></a>Implementación continua para Azure Functions

Puede usar Azure Functions para implementar el código de forma continua mediante la [integración del control de código fuente](functions-deployment-technologies.md#source-control). La integración del control de código fuente habilita un flujo de trabajo en el que una actualización de código desencadena la implementación en Azure. Si no está familiarizado con Azure Functions, consulte [Introducción a Azure Functions](functions-overview.md) para comenzar.

La implementación continua es una buena opción para los proyectos donde se integran contribuciones diversas y frecuentes. Al usar la implementación continua, se mantiene un único origen de confianza para el código, lo que permite a los equipos colaborar fácilmente. La implementación continua en Azure Functions se puede configurar desde las siguientes ubicaciones de código fuente:

* [Azure Repos](https://azure.microsoft.com/services/devops/repos/)
* [GitHub](https://github.com)
* [Bitbucket](https://bitbucket.org/)

La unidad de implementación de las funciones en Azure es la aplicación de funciones. Todas las funciones de una aplicación de funciones se implementan a la vez. Tras habilitar la implementación continua, el acceso al código de función en Azure Portal se configura como de *solo lectura*, ya que el origen de confianza está establecido en otro lugar.

## <a name="requirements-for-continuous-deployment"></a>Requisitos de implementación continua

Para que la implementación continua se realice correctamente, la estructura de directorios debe ser compatible con la estructura básica de carpetas que espera Azure Functions.

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

>[!NOTE]  
> Todavía no se admite la implementación continua para las aplicaciones de Linux que se ejecutan en un plan de consumo. 

## <a name="set-up-continuous-deployment"></a><a name="credentials"></a>Configuración de la implementación continua

Para configurar la implementación continua de una aplicación de funciones existente, siga estos pasos. Estos pasos muestran la integración con un repositorio de GitHub, aunque estos mismos pasos son válidos en Azure Repos o en otros repositorios de código fuente.

1. En la aplicación de funciones, en [Azure Portal](https://portal.azure.com), seleccione **Características de la plataforma** > **Centro de implementación**.

    ![Abrir el centro de implementación](./media/functions-continuous-deployment/platform-features.png)

2. En el **Centro de implementación**, seleccione **GitHub** y, después, **Autorizar** o, si ya ha autorizado GitHub, seleccione **Continuar**. 

    ![Centro de implementación de Azure App Service](./media/functions-continuous-deployment/github.png)

3. En GitHub, seleccione el botón **Autorizar AzureAppService**. 

    ![Autorizar Azure App Service](./media/functions-continuous-deployment/authorize.png)
    
    En el **Centro de implementación** de Azure Portal, seleccione **Continuar**.

4. Seleccione uno de los siguientes proveedores de compilación:

    * **Servicio de compilación de App Service**: opción ideal si no se necesita una compilación o se necesita una compilación genérica.
    * **Azure Pipelines (versión preliminar)** : opción ideal si se necesita más control sobre la compilación. Este proveedor está actualmente en versión preliminar.

    ![Selección de un proveedor de compilación](./media/functions-continuous-deployment/build.png)

5. Configure información específica de la opción de control de origen que haya indicado. En GitHub, debe escribir o seleccionar valores de **Organización**, **Repositorio** y **Rama**. Los valores se basan en la ubicación del código. Después, seleccione **Continuar**.

    ![Configuración de GitHub](./media/functions-continuous-deployment/github-specifics.png)

6. Revise todos los detalles y seleccione **Finalizar** para completar la configuración de la implementación.

    ![Resumen](./media/functions-continuous-deployment/summary.png)

Cuando finalice el proceso, todo el código del origen especificado se implementará en la aplicación. Hecho esto, los cambios en el origen de implementación desencadenarán una implementación de dichos cambios en la aplicación de funciones en Azure.

## <a name="deployment-scenarios"></a>Escenarios de implementación

<a name="existing"></a>

### <a name="move-existing-functions-to-continuous-deployment"></a>Movimiento de funciones existentes para la implementación continua

Si ya ha escrito funciones en [Azure Portal](https://portal.azure.com) y quiere descargar el contenido de la aplicación antes de cambiar a la implementación continua, vaya a la pestaña **Introducción** de la aplicación de funciones. Seleccione el botón **Descargar contenido de aplicación**.

![Descargar contenido de aplicación](./media/functions-continuous-deployment/download.png)

> [!NOTE]
> Después de configurar la integración continua, ya no podrá modificar los archivos de origen en el portal de Functions.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Procedimientos recomendados para Azure Functions](functions-best-practices.md)
