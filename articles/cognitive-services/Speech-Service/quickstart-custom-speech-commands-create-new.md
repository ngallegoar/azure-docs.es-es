---
title: 'Inicio rápido: Creación de una aplicación de comando personalizado versión preliminar: servicio de voz'
titleSuffix: Azure Cognitive Services
description: En este artículo, creará y probará una aplicación de comandos personalizados versión preliminar hospedada. La aplicación procesará expresiones.
services: cognitive-services
author: nitinme
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: nitinme
ms.openlocfilehash: 5b54923bb667053ffc886b335ab9d1b0cf4cb754
ms.sourcegitcommit: 5504d5a88896c692303b9c676a7d2860f36394c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2020
ms.locfileid: "84509362"
---
# <a name="quickstart-create-a-custom-commands-preview-app"></a>Inicio rápido: Creación de una aplicación de comandos personalizados versión preliminar

En este inicio rápido, aprenderá a crear y probar una aplicación de comandos personalizados.
La aplicación procesará expresiones como "turn on the tv" (enciende el televisor) y responderá con un mensaje sencillo como "Ok, turning on the tv" (de acuerdo, encenderé el televisor).

## <a name="prerequisites"></a>Requisitos previos

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Creación de un recurso de voz de Azure <span class="docon docon-navigate-external x-hidden-focus"></span></a>

  > [!NOTE]
  > En este momento, los comandos personalizados solo admiten suscripciones de voz en las regiones westus, westus2 y neur.

## <a name="go-to-the-speech-studio-for-custom-commands"></a>Navegación a Speech Studio para los comandos personalizados

1. En un explorador web, vaya a [Speech Studio](https://speech.microsoft.com/).
1. Introduzca sus credenciales para iniciar sesión en el portal.

   La vista predeterminada es la lista de suscripciones de Voz.
    > [!NOTE]
    > Si no ve la página para seleccionar una suscripción, puede llegar a ella al seleccionar **Recursos de voz** en el menú de configuración de la parte superior de la pantalla.

1. Seleccione su suscripción a voz y, a continuación, seleccione **Ir a Studio**.
1. Seleccione **Comandos personalizados**.

     La vista predeterminada es una lista de las aplicaciones de comandos personalizados de las que dispone en la suscripción seleccionada.

## <a name="create-a-custom-commands-project"></a>Creación de un proyecto de comandos personalizados

1. Seleccione **Nuevo proyecto** para crear un proyecto.

   > [!div class="mx-imgBorder"]
   > ![Creación de un proyecto](media/custom-speech-commands/create-new-project.png)

1. En el cuadro **Nombre**, escriba un nombre de proyecto.
1. En la lista **Lenguaje**, seleccione uno.
1. En la lista **Recurso de creación de LUIS**, seleccione un recurso de creación. Si no hay ninguno válido, créelo. Para ello, seleccione **Create new LUIS authoring resource** (Crear nuevo recurso de creación LUIS).

   > [!div class="mx-imgBorder"]
   > ![Creación de un recurso](media/custom-speech-commands/create-new-resource.png)

   1. En el cuadro **Nombre de recurso**, escriba el nombre del recurso.
   1. En la lista **Grupo de recursos**, seleccione uno.
   1. En la lista desplegable **Ubicación**, seleccione una.
   1. En la lista **Plan de tarifa**, seleccione un nivel.

      > [!NOTE]
      > Para crear un grupo de recursos, escriba el nombre del grupo de recursos en el cuadro **Grupo de recursos**. El grupo de recursos se creará cuando seleccione **Crear**.

1. Seleccione **Crear**.
1. Después de crear el proyecto, selecciónelo.

    Ahora debe mostrar una visión general de la nueva aplicación de comandos personalizados.

## <a name="update-luis-resources-optional"></a>Actualización de recursos de LUIS (opcional)

Puede actualizar el recurso de creación que seleccionó en la ventana **Nuevo proyecto** y establecer un recurso de predicción. El recurso de predicción se usa para el reconocimiento cuando se publica la aplicación de comandos personalizados. No necesita un recurso de predicción durante las fases de desarrollo y pruebas.

1. Seleccione **Configuración** en el panel izquierdo y, a continuación, seleccione **LUIS resources** (Recursos de LUIS) en el panel central.
1. Seleccione un recurso de predicción o cree uno seleccionando **Crear nuevo recurso**.
1. Seleccione **Guardar**.
    
    > [!div class="mx-imgBorder"]
    > ![Configuración de recursos de LUIS](media/custom-speech-commands/set-luis-resources.png)


> [!NOTE]
> Dado que el recurso de creación solo admite 1000 solicitudes de punto de conexión de predicción al mes, debe establecer un recurso de predicción LUIS antes de publicar la aplicación de comandos personalizados.


## <a name="create-a-command"></a>Creación de un comando

Vamos a usar un comando sencillo que usará una sola expresión (`turn on the tv`) y responderá con el mensaje `Ok, turning on the tv`.

1. Seleccione **Nuevo comando** en la parte superior del panel izquierdo para crear un comando. Se abre la ventana **Nuevo comando**.
1. En el cuadro **Nombre**, escriba **TurnOn**.
1. Seleccione **Crear**.

En el panel central se muestran las propiedades del comando:


| Configuración            | Descripción                                                                                                                 |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **Oraciones de ejemplo** | Ejemplos de expresiones que el usuario puede decir para desencadenar el comando.                                                                 |
| **Parámetros**       | Información necesaria para completar el comando.                                                                                |
| **Reglas de finalización** | Acciones que se realizarán para finalizar el comando. Por ejemplo, para responder al usuario o comunicarse con otro servicio web. |
| **Reglas de interacción**   | Reglas adicionales para controlar situaciones más específicas o complejas.                                                              |


> [!div class="mx-imgBorder"]
> ![Creación de un comando](media/custom-speech-commands/create-add-command.png)


### <a name="add-example-sentences"></a>Adición de oraciones de ejemplo

Comencemos con la sección **Example sentences** (Oraciones de ejemplo). Proporcionaremos un ejemplo de lo que puede decir el usuario.

1. Seleccione **Example sentences** (Oraciones de ejemplo) en el panel central. 
1. En el panel derecho, agregue los ejemplos:

    ```
    turn on the tv
    ```

1. En la parte superior del panel, seleccione **Guardar**.

Por ahora, no hay ningún parámetro, así que podemos continuar con la sección **Completion rules** (Reglas de finalización).

### <a name="add-a-completion-rule"></a>Adición de una regla de finalización

Ahora, agregue una regla de finalización que tenga la configuración siguiente. Esta regla informa al usuario que se está llevando a cabo una acción de cumplimiento.


| Configuración    | Valor sugerido                          | Descripción                                        |
| ---------- | ---------------------------------------- | -------------------------------------------------- |
| **Nombre**  | **ConfirmationResponse**                  | Nombre que describe el propósito de la regla.          |
| **Condiciones** | None                                     | Condiciones que determinan cuándo se puede ejecutar la regla.    |
| **Acciones**    | **Enviar una respuesta de voz: "- Ok, turning on the TV" (de acuerdo, encenderé el televisor)** | Acción que se realizará cuando la condición de la regla sea true. |

1. Cree una regla de finalización. Para hacerlo, seleccione **Agregar** en la parte superior del panel central.
1. En el cuadro **Name** (Nombre), escriba un nombre.
1. Agregue una acción.
   1. Cree una acción. Para hacerlo, seleccione **Agregar una acción** en la sección **Acciones**.
   1. En la ventana **Editar acción**, en la lista **Tipo**, seleccione **Send speech response** (Enviar respuesta de voz).
   1. En **Respuesta**, seleccione **Editor simple**. En el cuadro **First variation** (Primera variación), escriba **Ok, turning on the tv** (de acuerdo, encenderé el televisor).

   > [!div class="mx-imgBorder"]
   > ![Creación de una respuesta](media/custom-speech-commands/create-speech-response-action.png)

1. Seleccione **Guardar** para guardar la regla.
1. De nuevo en la sección **Completion rules** (reglas de finalización), seleccione **Guardar** para guardar todos los cambios. 

> [!div class="mx-imgBorder"]
> ![Creación de una regla de finalización](media/custom-speech-commands/create-basic-completion-response-rule.png)



## <a name="try-it-out"></a>Prueba

Use el panel de conversación de prueba para probar el comportamiento.
1. Seleccione **Entrenar** en la parte superior del panel derecho.
1. Después de realizar el entrenamiento, seleccione **Probar**. Aparecerá una nueva ventana **Test your application** (Probar la aplicación).
    - Escriba **turn on the tv** (enciende el televisor)
    - La respuesta esperada: **Ok, turning on the TV** (de acuerdo, encenderé el televisor)


> [!div class="mx-imgBorder"]
> ![Probar el comportamiento](media/custom-speech-commands/create-basic-test-chat.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Inicio rápido: Creación de una aplicación de comandos personalizados versión preliminar con parámetros](./quickstart-custom-speech-commands-create-parameters.md)
