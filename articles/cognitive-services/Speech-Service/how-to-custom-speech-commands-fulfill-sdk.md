---
title: Realización de comandos desde un cliente con Speech SDK
titleSuffix: Azure Cognitive Services
description: En este artículo, se explica cómo controlar las actividades de Comandos personalizados en un cliente con Speech SDK.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: trbye
ms.openlocfilehash: f11f5f3c2ad4c9f0241d34edeb664f739f88d15c
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2020
ms.locfileid: "82871750"
---
# <a name="fulfill-commands-from-a-client-with-the-speech-sdk-preview"></a>Realización de comandos desde un cliente con Speech SDK (versión preliminar)

Para finalizar las tareas con una aplicación de comandos personalizada, puede enviar cargas personalizadas a un dispositivo cliente conectado.

En este artículo, hará lo siguiente:

- Definir y enviar una carga JSON personalizada desde la aplicación de comandos personalizados.
- Recibir y visualizar el contenido de la carga JSON personalizada desde una aplicación cliente del SDK de Voz en C# de UWP.

## <a name="prerequisites"></a>Prerrequisitos
> [!div class = "checklist"]
> * [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)
> * Una clave de suscripción de Azure para el servicio de voz: [Obtenga una gratis](get-started.md) o créela en [Azure Portal](https://portal.azure.com).
> * Una aplicación de comandos personalizados creada previamente: [Inicio rápido: Creación de un comando personalizado con parámetros (versión preliminar)](./quickstart-custom-speech-commands-create-parameters.md)
> * Una aplicación cliente habilitada para el SDK de Voz: [Inicio rápido: conexión a una aplicación de comandos personalizados con el SDK de Voz (versión preliminar)](./quickstart-custom-speech-commands-speech-sdk.md)

## <a name="optional-get-started-fast"></a>Opcional: Empiece rápidamente

En este artículo se describe, paso a paso, cómo hacer que una aplicación cliente se comunique con la aplicación de comandos personalizados. Si prefiere sumergirse de lleno, el código fuente completo y listo para compilar utilizado en este artículo está disponible en los [ejemplos del SDK de Voz](https://aka.ms/csspeech/samples).

## <a name="fulfill-with-json-payload"></a>Finalización de la carga útil de JSON

1. Abra la aplicación de comandos personalizados que creó anteriormente desde [Inicio rápido: Creación de un comando personalizado con parámetros](./quickstart-custom-speech-commands-create-parameters.md)
1. Compruebe la sección **Reglas de finalización** para asegurarse de estar usando la regla creada anteriormente que responde al usuario.
1. Para enviar una carga directamente al cliente, cree una nueva regla con una acción Enviar actividad.

   > [!div class="mx-imgBorder"]
   > ![Regla de finalización Enviar actividad](media/custom-speech-commands/fulfill-sdk-completion-rule.png)

   | Configuración | Valor sugerido | Descripción |
   | ------- | --------------- | ----------- |
   | Nombre de la regla | UpdateDeviceState | Nombre que describe el propósito de la regla. |
   | Condiciones | Parámetro obligatorio: `OnOff` y `SubjectDevice`. | Condiciones que determinan cuándo se puede ejecutar la regla. |
   | Acciones | `SendActivity` (consulte a continuación) | Acción que se realizará cuando la condición de la regla sea true. |

1. Copie el código JSON siguiente en **Activity content** (Contenido de la actividad).
   ```json
   {
     "type": "event",
     "name": "UpdateDeviceState",
     "state": "{OnOff}",
     "device": "{SubjectDevice}"
   }
   ```
   > [!div class="mx-imgBorder"]
   > ![Carga de Enviar actividad](media/custom-speech-commands/fulfill-sdk-send-activity-action.png)

## <a name="create-visuals-for-device-on-or-off-state"></a>Creación de objetos visuales para el estado de encendido o apagado del dispositivo

En [Inicio rápido: Conexión a una aplicación de comandos personalizados con el SDK de Voz](./quickstart-custom-speech-commands-speech-sdk.md) creó una aplicación cliente del SDK de voz que administraba comandos como `turn on the tv`, `turn off the fan`. Después de agregar algunos objetos visuales, puede ver el resultado de esos comandos.

Agregue cuadros etiquetados con texto que indique **Encendido** o **Apagado** mediante el siguiente XML agregado a `MainPage.xaml`

```xml
<StackPanel Orientation="Horizontal" HorizontalAlignment="Center" Margin="20">
    <Grid x:Name="Grid_TV" Margin="50, 0" Width="100" Height="100" Background="LightBlue">
        <StackPanel>
            <TextBlock Text="TV" Margin="0, 10" TextAlignment="Center"/>
            <TextBlock x:Name="State_TV" Text="Off" TextAlignment="Center"/>
        </StackPanel>
    </Grid>
    <Grid x:Name="Grid_Fan" Margin="50, 0" Width="100" Height="100" Background="LightBlue">
        <StackPanel>
            <TextBlock Text="Fan" Margin="0, 10" TextAlignment="Center"/>
            <TextBlock x:Name="State_Fan" Text="Off" TextAlignment="Center"/>
        </StackPanel>
    </Grid>
</StackPanel>
```

## <a name="handle-customizable-payload"></a>Control de la carga personalizable
### <a name="add-reference-libraries"></a>Adición de bibliotecas de referencia

Dado que ha creado una carga JSON, ahora necesita agregar una referencia a la biblioteca [JSON.NET](https://www.newtonsoft.com/json) para controlar la deserialización.
- Haga clic con el botón derecho en la solución.
- Elija **Administrar paquetes NuGet para la solución** y seleccione **Instalar**. 
- Busque **Newtonsoft.json** en la lista de actualizaciones y actualice **Microsoft.NETCore.UniversalWindowsPlatform** a la versión más reciente.

> [!div class="mx-imgBorder"]
> ![Carga de Enviar actividad](media/custom-speech-commands/fulfill-sdk-json-nuget.png)

En "MainPage.xaml.cs", agregue
- `using Newtonsoft.Json;` 
- `using Windows.ApplicationModel.Core;`

### <a name="handle-received-payload"></a>Control de la carga recibida

En `InitializeDialogServiceConnector`, reemplace el controlador de eventos `ActivityReceived` por el código siguiente. El controlador de eventos `ActivityReceived` extraerá la carga de la actividad y cambiará el estado visual del televisor o el ventilador según corresponda.

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
1. El estado visual del televisor debe cambiar a "Encendido"
   > [!div class="mx-imgBorder"]
   > ![Carga de Enviar actividad](media/custom-speech-commands/fulfill-sdk-turn-on-tv.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Cómo: adición de validaciones a los parámetros de comandos personalizados (versión preliminar)](./how-to-custom-speech-commands-validations.md)
