---
title: 'Inicio rápido: Creación de una aplicación de comandos personalizados (versión preliminar) con parámetros: Servicio de voz'
titleSuffix: Azure Cognitive Services
description: En este artículo, agregará parámetros a una aplicación de comandos personalizados para que pueda encender y apagar varios dispositivos.
services: cognitive-services
author: nitinme
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: nitinme
ms.openlocfilehash: 940b3604487fbef7736c0d8f4f9299563b0b86d5
ms.sourcegitcommit: 5504d5a88896c692303b9c676a7d2860f36394c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2020
ms.locfileid: "84509311"
---
# <a name="quickstart-create-a-custom-commands-preview-application-with-parameters"></a>Inicio rápido: Creación de una aplicación de comandos personalizados (versión preliminar) con parámetros

En el [artículo anterior](./quickstart-custom-speech-commands-create-new.md), creó una sencilla aplicación de comandos personalizados sin parámetros.

En este artículo, ampliará la aplicación con parámetros para que pueda encender y apagar varios dispositivos.

## <a name="create-parameters"></a>Creación de parámetros

1. Abra el proyecto que creó en el [artículo anterior](./quickstart-custom-speech-commands-create-new.md).

   Editaremos el comando existente para que se pueda usar para activar y desactivar varios dispositivos.
1. Dado que el comando controlará ahora la activación y desactivación, cambie el nombre del comando a **TurnOnOff**.
   1. En el panel izquierdo, seleccione el comando **TurnOn** y, después, el botón de puntos suspensivos ( **...** ) junto a **New command** (Nuevo comando) en la parte superior del panel.
   
   1. Seleccione **Rename** (Cambiar nombre). En la ventana **Rename command** (Cambiar nombre del comando), cambie el **nombre** a **TurOnOff**. Seleccione **Guardar**.

1. Cree un parámetro para indicar si el usuario quiere encender o apagar el dispositivo.
   1. Seleccione **Add** (Agregar) en la parte superior del panel. En la lista desplegable, seleccione **Parameter** (Parámetro).
   1. En el panel derecho, en la sección **Parameters** (Parámetros), agregue un valor en el cuadro **Name** (Nombre).
   1. Seleccione **Requerido**. En la ventana **Add response for a required parameter** (Agregar respuesta para un parámetro requerido), seleccione **Simple editor** (Editor sencillo). En el cuadro **First variation** (Primera variación), escriba este texto:
        ```
        On or Off?
        ```
   1. Seleccione **Actualizar**.

       > [!div class="mx-imgBorder"]
       > ![Crear respuesta de parámetro requerida](media/custom-speech-commands/add-required-on-off-parameter-response.png)
   
1. Configure el resto de las propiedades del parámetro de la manera siguiente:
       

    | Configuración      | Valor sugerido     | Descripción                                                      |
    | ------------------ | ----------------| ---------------------------------------------------------------------|
    | **Nombre**               | **OnOff**           | Nombre descriptivo del parámetro.                                                                  |
    | **Is Global** (Es global)          | Desactivado       | Casilla que indica si el valor de este parámetro se aplica globalmente a todos los comandos de la aplicación.|
    | **Obligatorio**           | Seleccionado         | Una casilla que indica si se requiere un valor para el parámetro.  |
    | **Response for required parameter** (Respuesta para el parámetro requerido)      |**Simple editor -> On or Off?** (Editor sencillo < ¿Encendido o apagado?)      | Pregunta para solicitar el valor del parámetro cuando no se conoce. |
    | **Tipo**               | **String**          | Tipo de parámetro. Por ejemplo, número, cadena, fecha y hora, geografía.   |
    | **Configuración**      | **Aceptar valores de entrada predefinidos del catálogo interno** | En el caso de las cadenas, esta configuración limita las entradas a un conjunto de valores posibles. |
    | **Predefined input values** (Valores de entrada predefinidos)     | **on**, **off** (encendido, apagado)             | Conjunto de valores posibles y sus alias.         |
       


    > [!div class="mx-imgBorder"]
    > ![Crear parámetro](media/custom-speech-commands/create-on-off-parameter.png)

1. Haga clic en **Save** (Guardar) para guardar la configuración.

 1. Seleccione **Add** (Agregar) de nuevo para agregar un segundo parámetro. Este parámetro representa el nombre del dispositivo. Use estos valores de configuración:
   

       | Configuración            | Valor sugerido       | Descripción                                                                                               |
       | ------------------ | --------------------- | --------------------------------------------------------------------------------------------------------- |
       | **Nombre**               | **SubjectDevice**         | Nombre descriptivo para el parámetro.                                                                     |
       | **Is Global** (Es global)          | Desactivado             | Casilla que indica si el valor de este parámetro se aplica globalmente a todos los comandos de la aplicación. |
       | **Obligatorio**           | Seleccionado               | Una casilla que indica si se requiere un valor para el parámetro.          |
       | **Simple editor** (Editor sencillo)      | **Which device?** (¿Qué dispositivo?)    | Pregunta para solicitar el valor del parámetro cuando no se conoce.                                       |
       | **Tipo**               | **String**                | Tipo de parámetro. Por ejemplo, número, cadena, fecha y hora, geografía.                                                |
       | **Configuración**      | **Aceptar valores de entrada predefinidos del catálogo interno** | En el caso de las cadenas, esta configuración limita las entradas a un conjunto de valores posibles.       |
       | **Predefined input values** (Valores de entrada predefinidos) | **tv**, **fan**               | Conjunto de valores posibles y sus alias.                               |
       | **Aliases** (tv) (Alias [tv])      | **television**, **telly**     | Alias opcionales para cada uno de los valores de entrada predefinidos.                                 |

## <a name="add-example-sentences"></a>Adición de oraciones de ejemplo

Para los comandos que tienen parámetros, resulta útil agregar oraciones de ejemplo que cubran todas las combinaciones posibles. Por ejemplo:

- Información completa del parámetro: `turn {OnOff} the {SubjectDevice}`
- Información parcial del parámetro: `turn it {OnOff}`.
- Sin información del parámetro: `turn something`.

Las oraciones de ejemplo que tienen diferentes cantidades de información permiten que la aplicación de comandos personalizados resuelva las resoluciones de una sola captura y las resoluciones multigiro con información parcial.

Teniendo esto en cuenta, edite las oraciones de ejemplo para usar los parámetros como se sugiere aquí.

```
turn {OnOff} the {SubjectDevice}
{SubjectDevice} {OnOff}
turn it {OnOff}
turn something {OnOff}
turn something
```
> [!TIP]
> En el editor de oraciones de ejemplo, use llaves para hacer referencia a los parámetros: `turn {OnOff} the {SubjectDevice}`
>
> Use la tecla TAB para la finalización automática definida por los parámetros creados previamente.

## <a name="add-parameters-to-completion-rules"></a>Adición de parámetros a las reglas de finalización

Modifique la regla de finalización que creó en el [inicio rápido anterior](./quickstart-custom-speech-commands-create-new.md).

1. En la sección **Conditions** (Condiciones), seleccione **Add a condition** (Agregar una condición).
1. En la ventana **New Condition** (Nueva condición), en la lista **Type** (Tipo), seleccione **Required parameters** (Parámetros requeridos). En la lista, seleccione **OnOff** y **SubjectDevice**.
1. Seleccione **Crear**.
1. En la sección **Actions** (Acciones), edite la acción de **envío de respuesta de voz** existente. Para ello, mantenga el puntero sobre la acción y seleccione el botón de edición. Esta vez, use los nuevos parámetros `OnOff` y `SubjectDevice`:

    ```
    Ok, turning {OnOff} the {SubjectDevice}
    ```

## <a name="try-it-out"></a>Prueba
1. Seleccione **Train** (Entrenar) en la parte superior del panel derecho.

1. Después de realizar el entrenamiento, seleccione **Test** (Probar).
    
    Aparecerá una nueva ventana **Test your application** (Prueba de la aplicación).

1. Pruebe algunas interacciones.

        - Input: turn off the tv
        - Output: Ok, turning off the tv        
        - Input: turn off the television
        - Output: Ok, turning off the tv
        - Input: turn it off
        - Output: Which device?
        - Input: the tv
        - Output: Ok, turning off the tv

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Inicio rápido: Uso de comandos personalizados con Voz personalizada (versión preliminar)](./quickstart-custom-speech-commands-select-custom-voice.md)
