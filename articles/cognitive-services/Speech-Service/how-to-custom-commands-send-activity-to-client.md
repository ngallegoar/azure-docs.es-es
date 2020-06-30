---
title: Envío de actividad de Comandos personalizados a una aplicación cliente
titleSuffix: Azure Cognitive Services
description: En este artículo, aprenderá a enviar actividades desde una aplicación de Comandos personalizados a una aplicación cliente que ejecute el SDK de Voz.
services: cognitive-services
author: xiaojul
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: xiaojul
ms.openlocfilehash: 0a3e3455615006c0e93cf32eebcdaedac9960a79
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2020
ms.locfileid: "85307299"
---
# <a name="send-custom-commands-activity-to-client-application"></a>Envío de actividad de Comandos personalizados a una aplicación cliente

En este artículo, aprenderá a enviar actividades desde una aplicación de Comandos personalizados a una aplicación cliente que ejecute el SDK de Voz.

Debe realizar las siguientes tareas:

- Definir y enviar una carga JSON personalizada desde la aplicación de comandos personalizados.
- Recibir y visualizar el contenido de la carga JSON personalizada desde una aplicación cliente del SDK de Voz en C# de UWP.

## <a name="prerequisites"></a>Requisitos previos
> [!div class = "checklist"]
> * [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)
> * Una clave de suscripción de Azure para el servicio de voz: [Obtenga una gratis](get-started.md) o créela en [Azure Portal](https://portal.azure.com).
> * Una [aplicación de Comandos personalizados creada](quickstart-custom-commands-application.md) previamente
> * Una aplicación cliente habilitada para el SDK de Voz: [Procedimiento: Integración con una aplicación cliente mediante el SDK de Voz](./how-to-custom-commands-setup-speech-sdk.md)

## <a name="setup-send-activity-to-client"></a>Configuración del envío de actividad a un cliente 
1. Abra la aplicación de Comandos personalizados que creó anteriormente.
1. Seleccione el comando **TurnOnOff**, seleccione **ConfirmationResponse** en regla de finalización y, a continuación, seleccione **Agregar una acción.**
1. En **Nuevo tipo de acción**, seleccione **Send activity to client** (enviar una actividad al cliente).
1. Copie el código JSON siguiente en **Activity content** (Contenido de la actividad).
   ```json
   {
     "type": "event",
     "name": "UpdateDeviceState",
     "state": "{OnOff}",
     "device": "{SubjectDevice}"
   }
   ```
1. Haga clic en **Guardar** para crear una nueva regla con una acción Enviar actividad

   > [!div class="mx-imgBorder"]
   > ![Regla de finalización Enviar actividad](media/custom-commands/send-activity-to-client-completion-rules.png)

## <a name="integrate-with-client-application"></a>Integración con la aplicación cliente

En [Procedimiento: Configuración de una aplicación de comandos personalizados con el SDK de Voz (versión preliminar)](./how-to-custom-commands-setup-speech-sdk.md) creó una aplicación cliente de UWP con el SDK de Voz que administraba comandos como `turn on the tv`, `turn off the fan`. Después de agregar algunos objetos visuales, puede ver el resultado de esos comandos.

Agregue cuadros etiquetados con texto que indique **Encendido** o **Apagado** mediante el siguiente XML agregado a `MainPage.xaml`

```xml
<StackPanel Orientation="Horizontal" HorizontalAlignment="Center" Margin="20">
    <Grid x:Name="Grid_TV" Margin="50, 0" Width="100" Height="100" Background="LightBlue">
        <StackPanel>
            <TextBlock Text="TV" Margin="0, 10" TextAlignment="Center"/>
            <TextBlock x:Name="State_TV" Text="off" TextAlignment="Center"/>
        </StackPanel>
    </Grid>
    <Grid x:Name="Grid_Fan" Margin="50, 0" Width="100" Height="100" Background="LightBlue">
        <StackPanel>
            <TextBlock Text="Fan" Margin="0, 10" TextAlignment="Center"/>
            <TextBlock x:Name="State_Fan" Text="off" TextAlignment="Center"/>
        </StackPanel>
    </Grid>
</StackPanel>
```

### <a name="add-reference-libraries"></a>Adición de bibliotecas de referencia

Dado que ha creado una carga JSON, ahora necesita agregar una referencia a la biblioteca [JSON.NET](https://www.newtonsoft.com/json) para controlar la deserialización.

1. Haga clic con el botón derecho en la solución.
1. Elija **Administrar paquetes NuGet para la solución** y seleccione **Instalar**. 
1. Busque **Newtonsoft.json** en la lista de actualizaciones y actualice **Microsoft.NETCore.UniversalWindowsPlatform** a la versión más reciente.

> [!div class="mx-imgBorder"]
> ![Carga de Enviar actividad](media/custom-commands/send-activity-to-client-json-nuget.png)

En "MainPage.xaml.cs", agregue
- `using Newtonsoft.Json;` 
- `using Windows.ApplicationModel.Core;`

### <a name="handle-the-received-payload"></a>Control de la carga recibida

En `InitializeDialogServiceConnector`, reemplace el controlador de eventos `ActivityReceived` por el código siguiente. El controlador de eventos `ActivityReceived` modificado extraerá la carga de la actividad y cambiará el estado visual del televisor o el ventilador, respectivamente.

```C#
connector.ActivityReceived += async (sender, activityReceivedEventArgs) =>
{
    NotifyUser($"Activity received, hasAudio={activityReceivedEventArgs.HasAudio} activity={activityReceivedEventArgs.Activity}");

    dynamic activity = JsonConvert.DeserializeObject(activityReceivedEventArgs.Activity);
    var name = activity?.name != null ? activity.name.ToString() : string.Empty;

    if (name.Equals("UpdateDeviceState"))
    {
        Debug.WriteLine("Here");
        var state = activity?.device != null ? activity.state.ToString() : string.Empty;
        var device = activity?.device != null ? activity.device.ToString() : string.Empty;

        if (state.Equals("on") || state.Equals("off"))
        {
            switch (device)
            {
                case "tv":
                    await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
                        CoreDispatcherPriority.Normal, () => { State_TV.Text = state; });
                    break;
                case "fan":
                    await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
                        CoreDispatcherPriority.Normal, () => { State_Fan.Text = state; });
                    break;
                default:
                    NotifyUser($"Received request to set unsupported device {device} to {state}");
                    break;
            }
        }
        else { 
            NotifyUser($"Received request to set unsupported state {state}");
        }
    }

    if (activityReceivedEventArgs.HasAudio)
    {
        SynchronouslyPlayActivityAudio(activityReceivedEventArgs.Audio);
    }
};
```

## <a name="try-it-out"></a>Prueba

1. Inicio de la aplicación
1. Seleccione Habilitar micrófono.
1. Seleccionar el botón Hablar.
1. Diga `turn on the tv`.
1. El estado visual del televisor debe cambiar a "Encendido".
   > [!div class="mx-imgBorder"]
   > ![Carga de Enviar actividad](media/custom-commands/send-activity-to-client-turn-on-tv.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Procedimiento: configuración de puntos de conexión web (versión preliminar)](./how-to-custom-commands-setup-web-endpoints.md)