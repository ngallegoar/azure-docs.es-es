---
title: 'Inicio rápido: Creación de un comando personalizado (versión preliminar) - servicio de voz'
titleSuffix: Azure Cognitive Services
description: En este artículo, creará y probará una aplicación de comandos personalizados hospedada.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: donkim
ms.openlocfilehash: f31d7279b73bab7aefda4c4b6570500d05cb89d7
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2020
ms.locfileid: "82872528"
---
# <a name="quickstart-create-a-custom-commands-app-preview"></a>Inicio rápido: Creación de una aplicación de comandos personalizados (versión preliminar)

En este inicio rápido, aprenderá a crear y probar una aplicación de comandos personalizados.
La aplicación creada procesará expresiones como "turn on the tv" (enciende el televisor) y responderá con un mensaje sencillo como "Ok, turning on the tv" (De acuerdo, encenderé el televisor).

## <a name="prerequisites"></a>Prerrequisitos

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Creación de un recurso de voz de Azure <span class="docon docon-navigate-external x-hidden-focus"></span></a>

  > [!NOTE]
  > En este momento, los comandos personalizados solo admiten suscripciones de voz en las regiones westus, westus2 y neur.

## <a name="go-to-the-speech-studio-for-custom-commands"></a>Navegación a Speech Studio para los comandos personalizados

1. Abra el explorador web y navegue a [Speech Studio](https://speech.microsoft.com/).
1. Introduzca sus credenciales para iniciar sesión en el portal.

   - La vista predeterminada es la lista de suscripciones de Voz.
     > [!NOTE]
     > Si no ve la página para seleccionar una suscripción, puede navegar a ella al seleccionar "Recursos de voz" en el menú de configuración de la barra superior.

1. Seleccione su suscripción a voz y, a continuación, seleccione **Ir a Studio**.
1. Seleccione **Comandos personalizados**.

     - La vista predeterminada es una lista de las aplicaciones de comandos personalizados de la suscripción seleccionada.

## <a name="create-a-custom-commands-project"></a>Creación de un proyecto de comandos personalizados

1. Seleccione **Nuevo proyecto** para crear un nuevo proyecto.

   > [!div class="mx-imgBorder"]
   > ![Creación de un proyecto](media/custom-speech-commands/create-new-project.png)

1. Escriba el nombre del proyecto.
1. Seleccione un idioma en la lista desplegable.
1. Seleccione un recurso de creación en el menú desplegable. Si no hay ninguno válido, créelo. Para ello, debe seleccionar **Create new LUIS authoring resource** (Crear nuevo recurso de creación LUIS).

   > [!div class="mx-imgBorder"]
   > ![Creación de un recurso](media/custom-speech-commands/create-new-resource.png)

   - Escriba el nombre del recurso y el grupo de recursos.
   - Elija el valor de ubicación y plan de tarifa en el menú desplegable.

      > [!NOTE]
      > Para crear grupos de recursos, escriba el nombre del grupo de recursos deseado en el campo "Grupo de recursos". El grupo de recursos se creará cuando se seleccione **Crear**.

1. A continuación, seleccione **Crear** para crear el proyecto.
1. Una vez que cree el proyecto, selecciónelo.

    - Ahora, la vista debe mostrar una visión general de la aplicación de comandos personalizados que acaba de crear.

## <a name="update-luis-resources-optional"></a>Actualización de recursos de LUIS (opcional)

Puede actualizar el recurso de creación que se definió en la ventana Nuevo proyecto y establecer un recurso de predicción. El recurso de predicción se usa para el reconocimiento una vez que se publica la aplicación de comandos personalizados. No necesita un recurso de predicción para las fases de desarrollo y pruebas.

1. Seleccione **Configuración** en el panel izquierdo y desplácese hasta la sección **recursos LUIS** del panel central.
1. Seleccione un recurso de predicción o cree uno seleccionando **Crear nuevo recurso**.
1. Seleccione **Guardar**.
    
    > [!div class="mx-imgBorder"]
    > ![Establecimiento de recursos de LUIS](media/custom-speech-commands/set-luis-resources.png)


> [!NOTE]
> Dado que el recurso de creación solo admite 1000 solicitudes de punto de conexión de predicción al mes, deberá establecer obligatoriamente un recurso de predicción LUIS antes de publicar la aplicación de comandos personalizados.


## <a name="create-a-new-command"></a>Creación de un nuevo comando

Vamos a usar un comando sencillo que usará una sola expresión (`turn on the tv`) y responderá con el mensaje `Ok, turning on the tv`.

1. Seleccione el icono `+ New command` de la parte superior del panel de la izquierda para crear un comando nuevo. Aparecerá la nueva ventana emergente **Nuevo comando**.
1. Proporcione el valor del campo **Nombre** como `TurnOn`.
1. Seleccione **Crear**.

El panel central inscribe las distintas propiedades de un comando:


| Configuración            | Descripción                                                                                                                 |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| Oraciones de ejemplo | Expresiones de ejemplo que el usuario puede decir para desencadenar este comando.                                                                 |
| Parámetros       | Información necesaria para completar el comando.                                                                                |
| Reglas de finalización | Acciones que se van a realizar para finalizar el comando. Por ejemplo, para responder al usuario o comunicarse con otro servicio web. |
| Reglas de interacción   | Reglas adicionales para controlar situaciones más específicas o complejas.                                                              |


> [!div class="mx-imgBorder"]
> ![Creación de un comando](media/custom-speech-commands/create-add-command.png)


### <a name="add-example-sentences"></a>Adición de oraciones de ejemplo

Empecemos por la sección Oraciones de ejemplo y proporcionemos un ejemplo de lo que el usuario puede decir.

1. Seleccione la sección **Oraciones de ejemplo** del panel central y, en el panel situado más a la derecha, agregue ejemplos:

    ```
    turn on the tv
    ```

1. Seleccione el icono `Save` presente en la parte superior del panel.

Por ahora, no hay ningún parámetro, así que podemos continuar con la sección **Reglas de finalización**.

### <a name="add-a-completion-rule"></a>Adición de una regla de finalización

Ahora, agregue una regla de finalización con la configuración siguiente. Esta regla indica al usuario que se está llevando a cabo una acción de cumplimiento.


| Configuración    | Valor sugerido                          | Descripción                                        |
| ---------- | ---------------------------------------- | -------------------------------------------------- |
| Nombre de la regla  | ConfirmationResponse                  | Nombre que describe el propósito de la regla.          |
| Condiciones | None                                     | Condiciones que determinan cuándo se puede ejecutar la regla.    |
| Acciones    | SpeechResponse: "- Ok, turning on the TV" (De acuerdo, encendiendo el televisor) | Acción que se realizará cuando la condición de la regla sea true. |

1. Cree una regla de finalización. Para hacerlo, seleccione el icono `+Add` en la parte superior del panel central.
1. Proporcione un valor en la sección **Nombre**.
1. Agregar una acción
   1. Cree una acción nueva. Para hacerlo, seleccione **+ Add an action** (Agregar una acción) en la sección **Acciones**.
   1. En el menú emergente **Nueva acción**, seleccione `Send speech response` en las opciones desplegables de **Tipo**.
   1. Elija `Simple editor` para el campo **Respuesta**.
       - En el campo **First variation** (Primera variación), proporcione el valor de respuesta como `Ok, turning on the tv`.

   > [!div class="mx-imgBorder"]
   > ![Creación de una respuesta de voz](media/custom-speech-commands/create-speech-response-action.png)

1. Haga clic en **Guardar** para guardar la regla.
1. De nuevo en la sección **Completion rules** (reglas de finalización), seleccione **Guardar** para guardar todos los cambios. 

> [!div class="mx-imgBorder"]
> ![Creación de una regla de finalización](media/custom-speech-commands/create-basic-completion-response-rule.png)



## <a name="try-it-out"></a>Prueba

Use el panel de conversación de prueba para comprobar el comportamiento.
1. Seleccione el icono `Train` presente en la parte superior del panel derecho.
1. Una vez completado el entrenamiento, seleccione `Test`. Aparecerá una nueva ventana **Test your application** (Prueba de la aplicación).
    - Si escribe: turn on the tv (enciende el televisor)
    - La respuesta esperada: Ok, turning on the TV (de acuerdo, encendiendo el televisor)


> [!div class="mx-imgBorder"]
> ![Prueba con chat en web](media/custom-speech-commands/create-basic-test-chat.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Inicio rápido: Creación de un comando personalizado con parámetros (versión preliminar)](./quickstart-custom-speech-commands-create-parameters.md)
