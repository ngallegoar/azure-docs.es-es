---
title: Instalación o actualización manual de extensiones de enlace de Azure Functions
description: Obtenga información sobre cómo instalar o actualizar extensiones de enlace de Azure Functions para aplicaciones de función implementada.
ms.topic: reference
ms.date: 09/26/2018
ms.openlocfilehash: e8716f691a5d19ddac7fece47c423e1f7787b9db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75768867"
---
# <a name="manually-install-or-update-azure-functions-binding-extensions-from-the-portal"></a>Instalación o actualización manual de extensiones de enlace de Azure Functions desde el portal

A partir de la versión 2.x, el entorno en tiempo de ejecución de Azure Functions usa extensiones de enlace para implementar código para desencadenadores y enlaces. Las extensiones de enlace se proporcionan en paquetes NuGet. Para registrar una extensión, básicamente se instala un paquete. Al desarrollar funciones, la manera de instalar extensiones de enlace depende del entorno de desarrollo. Para más información, consulte [Registro de extensiones de enlace](./functions-bindings-register.md) en el artículo de enlaces y desencadenadores.

A veces, es necesario instalar o actualizar las extensiones de enlace en Azure Portal manualmente. Por ejemplo, es posible que necesite actualizar un enlace registrado a una versión más reciente. También es posible que deba registrar un enlace compatible que no se puede instalar en la pestaña **Integrar** del portal.

## <a name="install-a-binding-extension"></a>Instalación de una extensión de enlace

Siga estos pasos para instalar o actualizar manualmente extensiones desde el portal.

1. En [Azure Portal](https://portal.azure.com), busque la aplicación de función y selecciónela. Elija la pestaña **Información general** y seleccione **Detener**.  Al detener la aplicación de función se desbloquean los archivos, lo que permite realizar cambios.

1. Elija la pestaña **Características de la plataforma** y, en **Herramientas de desarrollo**, seleccione **Herramientas avanzadas (Kudu)** . El punto de conexión de Kudu (`https://<APP_NAME>.scm.azurewebsites.net/`) se abrirá en una nueva ventana.

1. En la ventana de Kudu, seleccione **Consola de depuración** > **CMD**.  

1. En la ventana de comandos, vaya a `D:\home\site\wwwroot` y seleccione el icono de eliminación junto a `bin` para eliminar la carpeta. Haga clic en **Aceptar** para confirmar la eliminación.

1. Seleccione el icono de edición junto al archivo `extensions.csproj`, que define las extensiones de enlace de la aplicación de función. El archivo de proyecto se abrirá en el editor en línea.

1. Realice las adiciones y actualizaciones necesarias de elementos de **PackageReference** en **ItemGroup**; a continuación, seleccione **Guardar**. La lista actual de versiones de paquete compatibles se puede consultar en el artículo de la wiki sobre [qué paquetes se necesitan](https://github.com/Azure/azure-functions-host/wiki/Updating-your-function-app-extensions#what-nuget-packages-do-i-need). Los tres enlaces de Azure Storage requieren el paquete Microsoft.Azure.WebJobs.Extensions.Storage.

1. Desde la carpeta `wwwroot`, ejecute el comando siguiente para volver a generar los ensamblados de referencia en la carpeta `bin`.

    ```cmd
    dotnet build extensions.csproj -o bin --no-incremental --packages D:\home\.nuget
    ```

1. Vuelva a la pestaña **Información general** del portal y seleccione **Iniciar** para reiniciar la aplicación de función.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre desencadenadores y enlaces de Azure Functions](functions-triggers-bindings.md)
