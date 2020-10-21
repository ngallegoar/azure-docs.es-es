---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 10/15/2020
ms.author: erhopf
ms.openlocfilehash: 3f1a24db459cabd65d9ce17b89105c3b9ab8abb7
ms.sourcegitcommit: 93329b2fcdb9b4091dbd632ee031801f74beb05b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92097191"
---
1. Inicie Visual Studio 2019.

1. Asegúrese de que la carga de trabajo de **Desarrollo multiplataforma de .NET Core** esté disponible. Elija **Herramientas** > **Get Tools and Features (Obtener herramientas y características)** desde la barra de menús de Visual Studio para abrir el instalador de Visual Studio. Si esta carga de trabajo ya está habilitada, cierre el cuadro de diálogo.

   ![Captura de pantalla del instalador de Visual Studio, con la pestaña Workloads (Cargas de trabajo) resaltada](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-net-core-workload.png)

   En caso contrario, active la casilla junto a **Desarrollo multiplataforma de .NET Core** y elija **Modificar** en la esquina inferior derecha del cuadro de diálogo. La instalación de la nueva característica tardará un momento.

1. Cree una nueva aplicación de consola de .NET Core en Visual C#. En el cuadro de diálogo **Nuevo proyecto**, en el panel izquierdo, expanda **Instalado** > **Visual C#**  >  **.NET Core**. Luego, seleccione **Aplicación de consola (.NET Framework)** . Para el nombre del proyecto, escriba *helloworld*.

   ![Captura de pantalla del cuadro de diálogo Nuevo proyecto](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-01-new-console-app.png "Creación de una aplicación de consola de Visual C# (.NET Core)")

1. Instale y haga referencia al [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget). En el Explorador de soluciones, haga clic con el botón derecho en la solución y seleccione **Manage NuGet Packages for Solution** (Administrar paquetes de NuGet para la solución).

   ![Captura de pantalla del Explorador de soluciones, con la opción Administrar paquetes de NuGet para la solución resaltada](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-02-manage-nuget-packages.png "Administrar paquetes de NuGet para la solución")

1. En la esquina superior derecha, en el campo **Origen del paquete**, seleccione **nuget.org**. Busque el paquete `Microsoft.CognitiveServices.Speech` e instálelo en el proyecto **helloworld**.

   ![Captura de pantalla del cuadro de diálogo "Administrar paquetes para la solución".](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-03-nuget-install-1.0.0.png "Instalación de un paquete NuGet")

1. Acepte la licencia que aparece para comenzar la instalación del paquete de NuGet.

   ![Captura de pantalla del cuadro de diálogo Aceptación de licencia](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-04-nuget-license.png "Aceptación de la licencia")

Después de instalar el paquete, aparecerá una confirmación en la consola del Administrador de paquetes.
