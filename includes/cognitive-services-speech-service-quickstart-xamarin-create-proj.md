---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 09/13/2019
ms.author: erhopf
ms.openlocfilehash: 35133b64e43ca3814410d7bbf2ac1be9f760b369
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91654434"
---
Para crear un proyecto de Visual Studio para el desarrollo en .NET de aplicaciones móviles multiplataforma con Xamarin, debe:
- Configurar las opciones de desarrollo de Visual Studio.
- Crear el proyecto y seleccionar la arquitectura de destino. 
- Instalar el SDK de Voz.

### <a name="set-up-visual-studio-development-options"></a>Configuración de las opciones de desarrollo de Visual Studio

Para empezar, asegúrese de que Visual Studio está configurado correctamente para el desarrollo para dispositivos móviles multiplataforma con .NET:

1. Abra Visual Studio 2019.

1. En la barra de menús de Visual Studio, seleccione **Herramientas** > **Get Tools and Features** (Obtener herramientas y características) para abrir el Instalador de Visual Studio y ver el cuadro de diálogo **Modificar**.

   ![Captura de pantalla que muestra la pestaña Cargas de trabajo, el cuadro de diálogo Modificando, el Instalador de Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-workload.png)

1. En la pestaña **Cargas de trabajo**, en **Windows**, busque la carga de trabajo **Desarrollo para dispositivos móviles con .NET**. Si la casilla situada junto a esa carga de trabajo ya está seleccionada, cierre el cuadro de diálogo **Modificar** y vaya al paso 5.

1. Seleccione la casilla **Desarrollo para dispositivos móviles con .NET** y seleccione **Modificar**. En el cuadro de diálogo **Antes de comenzar**, seleccione **Continuar** para instalar la carga de trabajo de desarrollo para dispositivos móviles con .NET. La instalación de la nueva característica puede tardar un rato.

1. Cierre el Instalador de Visual Studio.

### <a name="create-the-project"></a>Creación del proyecto

1. En la barra de menús de Visual Studio, seleccione **Archivo** > **Nuevo** > **Proyecto** para mostrar la ventana **Crear un nuevo proyecto**.

   ![Captura de pantalla que muestra cómo crear un proyecto en Visual Studio.](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-create-new-project.png)

1. Busque y seleccione **Aplicación móvil (Xamarin.Forms)** .

1. Seleccione **Siguiente** para abrir la pantalla **Configurar el nuevo proyecto**.

   ![Captura de pantalla que muestra cómo configurar el nuevo proyecto en Visual Studio.](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-configure-your-new-project.png)

1. En **Nombre del proyecto**, escriba *helloworld*.

1. En **Ubicación**, vaya a la carpeta en la que desea guardar el proyecto y selecciónela o créela.

1. Seleccione **Crear** para ir a la ventana de **nuevo proyecto aplicación móvil de Xamarin Forms**.

   ![Captura de pantalla que muestra el cuadro de diálogo Nuevo proyecto de la Plataforma universal de Windows en Visual Studio.](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-xamarin-new-xamarin-project.png)

1. Seleccione la plantilla **En blanco**.

1. En **Plataforma**, seleccione las casillas **Android**, **iOS** y **Windows (UWP)** .

1. Seleccione **Aceptar**. Se le devolverá al IDE de Visual Studio, con el nuevo proyecto creado y visible en el panel **Explorador de soluciones**.

   ![Proyecto helloworld: Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-helloworld.png)

Ahora seleccione la arquitectura de la plataforma de destino y el proyecto de inicio. En la barra de herramientas de Visual Studio, busque el cuadro desplegable **Plataformas de solución**. (Si no lo ve, seleccione **Ver** > **Barra de herramientas** > **Estándar** para mostrar la barra de herramientas que contiene **Plataformas de solución**). Si está ejecutando Windows de 64 bits, seleccione **x64** en el cuadro desplegable. Puede seleccionar **x86** si lo desea porque Windows de 64 bits también puede ejecutar aplicaciones de 32 bits. En el cuadro desplegable **Proyectos de inicio**, establezca **helloworld.UWP (Universal Windows)** .

### <a name="install-the-speech-sdk"></a>Instalación de Speech SDK

Instale el [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget) y haga referencia al SDK de Voz del proyecto.

1. En el **Explorador de soluciones**, haga clic con el botón derecho en la solución. Seleccione **Administrar paquetes NuGet para la solución** para ir a la ventana **NuGet - Solución**.

1. Haga clic en **Examinar**.

   ![Captura de pantalla del cuadro de diálogo Administrar paquetes para la solución al instalar el SDK de Voz.](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-nuget-solution-browse.png)

1. En **Origen del paquete**, seleccione nuget.org.

1. En el cuadro de **búsqueda**, escriba *Microsoft.CognitiveServices.Speech*. A continuación, seleccione ese paquete después de que aparezca en los resultados de la búsqueda.

   ![Captura de pantalla que resalta el paquete Microsoft.CognitiveServices.Speech.](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-xamarin-nuget-install.png)

   > [!NOTE] 
   > La biblioteca de iOS dentro del paquete NuGet `Microsoft.CognitiveServices.Speech` no tiene habilitado Bitcode. Si necesita que la biblioteca Bitcode esté habilitada para la aplicación, use el paquete NuGet `Microsoft.CognitiveServices.Speech.Xamarin.iOS` para el proyecto de iOS específicamente.

1. En el panel de estado del paquete situado junto a los resultados de la búsqueda, seleccione todos los proyectos: **helloworld**, **helloworld.Android**, **helloworld.iOS** y **helloworld.UWP**.

1. Seleccione **Instalar**.

1. En el cuadro de diálogo **Vista previa de los cambios**, seleccione **Aceptar**.

1. En el cuadro de diálogo **Aceptación de licencia**, vea la licencia y, a continuación, seleccione **Acepto**. Instale la referencia de paquete del SDK de Voz en todos los proyectos. Una vez completada correctamente la instalación, es posible que vea la siguiente advertencia para helloworld.iOS. Este es un problema conocido y no debe afectar a la funcionalidad de la aplicación.

   > No se pudo resolver la referencia "C:\Users\Default\.nuget\packages\microsoft.cognitiveservices.speech\1.7.0\build\Xamarin.iOS\libMicrosoft.CognitiveServices.Speech.core.a". Si el código requiere esta referencia, pueden aparecer errores de compilación.
