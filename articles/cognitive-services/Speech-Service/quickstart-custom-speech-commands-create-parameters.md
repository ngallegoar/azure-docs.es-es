---
title: 'Inicio rápido: Creación de un comando personalizado con parámetros (versión preliminar): servicio de voz'
titleSuffix: Azure Cognitive Services
description: En este artículo, agregará parámetros a una aplicación de comandos personalizados.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: donkim
ms.openlocfilehash: bf77616123f9311f7384fea515f250e47b354c8c
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2020
ms.locfileid: "82853609"
---
# <a name="quickstart-create-a-custom-commands-application-with-parameters-preview"></a>Inicio rápido: Creación de una aplicación de comandos personalizados con parámetros (versión preliminar)

En el [artículo anterior](./quickstart-custom-speech-commands-create-new.md), creó una sencilla aplicación de comandos personalizados sin parámetros.

En este artículo, la ampliará para que use parámetros y pueda controlar el encendido y apagado de varios dispositivos.

## <a name="create-parameters"></a>Creación de parámetros

1. Abra el proyecto [que creó anteriormente](./quickstart-custom-speech-commands-create-new.md).
1. Vamos a editar el comando existente para activar y desactivar varios dispositivos.
1. Dado que el comando controlará ahora el encendido y apagado, cambie el nombre del comando a `TurnOnOff`.
   - En el panel izquierdo, seleccione el comando `TurnOn` y, a continuación, haga clic en el icono `...` situado junto a `+ New command` en la parte superior del panel.
   
   - Seleccione el icono `Rename`. En la ventana emergente **Rename command** (Cambiar nombre de comando), cambie **Nombre** a `TurOnOff`. Luego, seleccione **Guardar**.

1. A continuación, cree un nuevo parámetro para indicar si el usuario quiere encender o apagar el dispositivo.
   - Seleccione el icono `+ Add` presente en la parte superior del panel central. En el menú desplegable, seleccione **Parámetro**.
   - En el panel de la derecha, puede ver la sección de configuración de **Parámetros**.
   - Agregue un valor para **Nombre**.
   - Active la casilla **Obligatorio**. En la ventana **Add response for a required parameter** (Agregar respuesta para un parámetro requerido), seleccione **Simple editor** (Editor sencillo) y, en **First variation** (Primera variación), agregue
        ```
        On or Off?
        ```
   - Seleccione **Actualizar**.

       > [!div class="mx-imgBorder"]
       > ![Crear respuesta de parámetro requerida](media/custom-speech-commands/add-required-on-off-parameter-response.png)
   
   - A continuación, vamos a configurar el resto de las propiedades del parámetro como se indica a continuación y a seleccionar `Save` para guardar todas las configuraciones del parámetro.
       

       | Configuración      | Valor sugerido     | Descripción                                                      |
       | ------------------ | ----------------| ---------------------------------------------------------------------|
       | Nombre               | OnOff           | Nombre descriptivo para el parámetro                                                                           |
       | Es global          | Desactivado       | Casilla que indica si el valor de este parámetro se aplica globalmente a todos los comandos de la aplicación.|
       | Obligatorio           | Activado         | Casilla que indica si es obligatorio especificar un valor para este parámetro antes de completar el comando. |
       | Respuesta para el parámetro requerido      |Editor sencillo -> On (Activado) u Off (Desactivado)      | Pregunta para solicitar el valor de este parámetro cuando no se conoce. |
       | Tipo               | String          | Tipo de parámetro; por ejemplo, Number, String, Date Time o Geography   |
       | Configuración      | Aceptar valores de entrada predefinidos del catálogo interno | En el caso de las cadenas, esto limita las entradas a un conjunto de valores posibles. |
       | Valores de entrada predefinidos     | on, off             | Conjunto de valores posibles y sus alias         |
       
        > [!div class="mx-imgBorder"]
        > ![Crear parámetro](media/custom-speech-commands/create-on-off-parameter.png)

   - A continuación, vuelva a seleccionar el icono de `+ Add` para agregar un segundo parámetro que represente el nombre de los dispositivos con la siguiente configuración.
   

       | Configuración            | Valor sugerido       | Descripción                                                                                               |
       | ------------------ | --------------------- | --------------------------------------------------------------------------------------------------------- |
       | Nombre               | SubjectDevice         | Nombre descriptivo para el parámetro                                                                     |
       | Es global          | Desactivado             | Casilla que indica si el valor de este parámetro se aplica globalmente a todos los comandos de la aplicación. |
       | Obligatorio           | Activado               | Casilla que indica si es obligatorio especificar un valor para este parámetro antes de completar el comando.          |
       | Editor sencillo      | ¿Qué dispositivo?    | Pregunta para solicitar el valor de este parámetro cuando no se conoce.                                       |
       | Tipo               | String                | Tipo de parámetro; por ejemplo, Number, String, Date Time o Geography                                                |
       | Configuración      | Aceptar valores de entrada predefinidos del catálogo interno | En el caso de las cadenas, una lista de cadenas limita las entradas a un conjunto de valores posibles.       |
       | Valores de entrada predefinidos | TV, ventilador               | Conjunto de valores posibles y sus alias                               |
       | Alias (TV)      | televisión, tele     | Alias opcionales para cada valor posible de los valores de entrada predefinidos                                 |

## <a name="add-example-sentences"></a>Adición de oraciones de ejemplo

Con los comandos con parámetros, resulta útil agregar oraciones de ejemplo que cubran todas las combinaciones posibles. Por ejemplo:

1. Información del parámetro completa: `turn {OnOff} the {SubjectDevice}`
1. Información del parámetro parcial: `turn it {OnOff}`.
1. Información del parámetro faltante: `turn something`.

Las oraciones de ejemplo con diferentes niveles de información permiten que la aplicación de comandos personalizados resuelva las resoluciones de una sola captura y las resoluciones multigiro con información parcial.

Teniendo esto en cuenta, edite las oraciones de ejemplo para usar los parámetros como se sugiere a continuación.

```
turn {OnOff} the {SubjectDevice}
{SubjectDevice} {OnOff}
turn it {OnOff}
turn something {OnOff}
turn something
```
> [!TIP]
> En el editor de oraciones de ejemplo, use llaves para hacer referencia a los parámetros. - `turn {OnOff} the {SubjectDevice}` Use la tecla TAB para la finalización automática respaldada por parámetros creados previamente.

## <a name="add-parameters-to-completion-rules"></a>Adición de parámetros a las reglas de finalización

Modifique la regla de finalización que creamos en [la guía de inicio rápido anterior](./quickstart-custom-speech-commands-create-new.md).

1. En la sección **Condiciones**, seleccione **+ Agregar una condición.** para agregar una nueva.
1. En la ventana emergente **Nueva condición**, seleccione `Required parameters` en la lista desplegable **Tipo**. En la lista de comprobación siguiente, active `OnOff` y `SubjectDevice`.
1. Haga clic en **Crear**.
1. En la sección **Acciones**, edite la acción de envío de respuesta de voz existente. Para ello, mantenga el puntero sobre la acción y haga clic en el icono de edición. Esta vez, vamos a usar los parámetros `SubjectDevice` y `OnOff` recién creados

    ```
    Ok, turning {OnOff} the {SubjectDevice}
    ```

## <a name="try-it-out"></a>Prueba
1. Seleccione el icono `Train` presente en la parte superior del panel derecho.

1. Una vez completado el entrenamiento, seleccione `Test`.
    - Aparecerá una nueva ventana **Test your application** (Prueba de la aplicación).
    - Pruebe algunas interacciones.

        - Entrada: Turn off the tv (apagar el televisor)
        - Salida: Ok, turning on the TV (de acuerdo, encenderé el televisor)        
        - Entrada: turn off the television (apaga el televisor)
        - Salida: Ok, turning on the TV (de acuerdo, encenderé el televisor)
        - Entrada: turn it off (apágalo)
        - Salida: ¿Qué dispositivo?
        - Entrada: the tv (el televisor)
        - Salida: Ok, turning on the TV (de acuerdo, encenderé el televisor)

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Inicio rápido: Uso de comandos personalizados con Voz personalizada (versión preliminar)](./quickstart-custom-speech-commands-select-custom-voice.md)
