---
title: Realización de comandos desde un cliente con Speech SDK
titleSuffix: Azure Cognitive Services
description: En este artículo, se explica cómo controlar las actividades de Comandos personalizados en un cliente con Speech SDK.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/12/2020
ms.author: donkim
ms.openlocfilehash: e109955774722da7f55defe1417de35ff202cce8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79367756"
---
# <a name="fulfill-commands-from-a-client-with-the-speech-sdk-preview"></a>Realización de comandos desde un cliente con Speech SDK (versión preliminar)

Para finalizar las tareas con una aplicación de comandos personalizada, puede enviar cargas personalizadas a un dispositivo cliente conectado.

En este artículo, hará lo siguiente:

- Definir y enviar una carga JSON personalizada desde la aplicación de comandos personalizados.
- Recibir y visualizar el contenido de la carga JSON personalizada desde una aplicación cliente del SDK de Voz en C# de UWP.

## <a name="prerequisites"></a>Prerrequisitos

- [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)
- Una clave de suscripción de Azure para el servicio de voz.
  - [Obtenga una gratis](get-started.md) o créela en [Azure Portal](https://portal.azure.com).
- Una aplicación de comandos personalizados creada previamente.
  - [Inicio rápido: Creación de un comando personalizado con parámetros (versión preliminar)](./quickstart-custom-speech-commands-create-parameters.md)
- Una aplicación cliente habilitada para el SDK de Voz.
  - [Inicio rápido: conexión a una aplicación de comandos personalizados con el SDK de Voz (versión preliminar)](./quickstart-custom-speech-commands-speech-sdk.md)

## <a name="optional-get-started-fast"></a>Opcional: Empiece rápidamente

En este artículo se describe, paso a paso, cómo hacer que una aplicación cliente se comunique con la aplicación de comandos personalizados. Si prefiere sumergirse de lleno, el código fuente completo y listo para compilar utilizado en este artículo está disponible en los [ejemplos del SDK de Voz](https://aka.ms/csspeech/samples).

## <a name="fulfill-with-json-payload"></a>Finalización de la carga útil de JSON

1. Abra la aplicación de comandos personalizados creada anteriormente desde [Speech Studio](https://speech.microsoft.com/).
1. Compruebe la sección **Reglas de finalización** para asegurarse de estar usando la regla creada anteriormente que responde al usuario.
1. Para enviar una carga directamente al cliente, cree una nueva regla con una acción Enviar actividad.

   > [!div class="mx-imgBorder"]
   > ![Regla de finalización Enviar actividad](media/custom-speech-commands/fulfill-sdk-completion-rule.png)

   | Configuración | Valor sugerido | Descripción |
   | ------- | --------------- | ----------- |
   | Nombre de la regla | UpdateDeviceState | Nombre que describe el propósito de la regla. |
   | Condiciones | Parámetro obligatorio: `OnOff` y `SubjectDevice`. | Condiciones que determinan cuándo se puede ejecutar la regla. |
   | Acciones | `SendActivity` (consulte a continuación) | Acción que se realizará cuando la condición de la regla sea true. |

   > [!div class="mx-imgBorder"]
   > ![Carga de Enviar actividad](media/custom-speech-commands/fulfill-sdk-send-activity-action.png)

   ```json
   {
     "type": "event",
     "name": "UpdateDeviceState",
     "state": "{OnOff}",
     "device": "{SubjectDevice}"
   }
   ```

## <a name="create-visuals-for-device-on-or-off-state"></a>Creación de objetos visuales para el estado de encendido o apagado del dispositivo

En [Inicio rápido: conexión a una aplicación de comandos personalizados con el SDK de Voz (versión preliminar)](./quickstart-custom-speech-commands-speech-sdk.md) creó una aplicación cliente del SDK de voz que administraba comandos como `turn on the tv`, `turn off the fan`. Ahora, agregue algunos objetos visuales para que pueda ver el resultado de esos comandos.

Agregue cuadros etiquetados con texto que indique **Encendido** o **Apagado** mediante el siguiente XML agregado a `MainPage.xaml.cs`

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

Ahora que ha creado una carga JSON, puede agregar una referencia a la biblioteca [JSON.NET](https://www.newtonsoft.com/json) para controlar la deserialización.

> [!div class="mx-imgBorder"]
> ![Carga de Enviar actividad](media/custom-speech-commands/fulfill-sdk-json-nuget.png)

En `InitializeDialogServiceConnector`, agregue lo siguiente al controlador de eventos `ActivityReceived`. El código adicional extraerá la carga de la actividad y cambiará el estado visual del televisor o el ventilador según corresponda.

```C#
connector.ActivityReceived += async (sender, activityReceivedEventArgs) =>
{
    NotifyUser($"Activity received, hasAudio={activityReceivedEventArgs.HasAudio} activity={activityReceivedEventArgs.Activity}");

    dynamic activity = JsonConvert.DeserializeObject(activityReceivedEventArgs.Activity);

    if(activity?.name == "SetDeviceState")
    {
        var state = activity?.state;
        var device = activity?.device;
        switch(device)
        {
            case "tv":
                State_TV.Text = state;
                break;
            case "fan":
                State_Fan.Text = state;
                break;
            default:
                NotifyUser($"Received request to set unsupported device {device} to {state}");
                break;
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

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Cómo: adición de validaciones a los parámetros de comandos personalizados (versión preliminar)](./how-to-custom-speech-commands-validations.md)
