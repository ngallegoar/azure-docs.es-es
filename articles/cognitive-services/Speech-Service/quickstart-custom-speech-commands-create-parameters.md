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
ms.date: 12/09/2019
ms.author: donkim
ms.openlocfilehash: 96312bac369cfa5fe3cb8a00fd63ecfbec624918
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80348527"
---
# <a name="quickstart-create-a-custom-command-with-parameters-preview"></a>Inicio rápido: Creación de un comando personalizado con parámetros (versión preliminar)

En el [artículo anterior](./quickstart-custom-speech-commands-create-new.md), creamos un nuevo proyecto de comandos personalizados para responder a comandos sin parámetros.

En este artículo, extenderemos esta aplicación con parámetros para que pueda controlar el encendido y apagado de varios dispositivos.

## <a name="create-parameters"></a>Creación de parámetros

1. Abra el proyecto [creado anteriormente](./quickstart-custom-speech-commands-create-new.md).
1. Dado que el comando controlará ahora el encendido y apagado, cambie el nombre del comando a "TurnOnOff".
   - Mantenga el mouse sobre el nombre del comando y seleccione el icono de edición para cambiar el nombre.
1. Cree un nuevo parámetro para indicar si el usuario quiere encender o apagar el dispositivo.
   - Seleccione el icono de `+` junto a la sección de parámetros.

   > [!div class="mx-imgBorder"]
   > ![Crear parámetro](media/custom-speech-commands/create-on-off-parameter.png)

   | Configuración            | Valor sugerido     | Descripción                                                                                               |
   | ------------------ | ------------------- | --------------------------------------------------------------------------------------------------------- |
   | Nombre               | OnOff               | Nombre descriptivo para el parámetro.                                                                     |
   | Es global          | Desactivado           | Casilla que indica si el valor de este parámetro se aplica globalmente a todos los comandos del proyecto. |
   | Obligatorio           | Activado             | Casilla que indica si es obligatorio especificar un valor para este parámetro antes de completar el comando.          |
   | Plantilla de respuesta  | On (Encendido) u Off (Apagado)      | Pregunta para solicitar el valor de este parámetro cuando no se conoce.                                       |
   | Tipo               | String              | Tipo de parámetro, como número, cadena o fecha y hora.                                               |
   | Configuración      | Lista de cadenas         | En el caso de las cadenas, una lista de cadenas limita las entradas a un conjunto de valores posibles.                                      |
   | Valores de la lista de cadenas. | on, off             | Para un parámetro de lista de cadenas, el conjunto de valores posibles y sus sinónimos.                                |

   - A continuación, vuelva a seleccionar el icono de `+` para agregar un segundo parámetro que represente el nombre de los dispositivos. En este ejemplo, un televisor y un ventilador.

   | Configuración            | Valor sugerido       | Descripción                                                                                               |
   | ------------------ | --------------------- | --------------------------------------------------------------------------------------------------------- |
   | Nombre               | SubjectDevice         | Nombre descriptivo para el parámetro.                                                                     |
   | Es global          | Desactivado             | Casilla que indica si el valor de este parámetro se aplica globalmente a todos los comandos del proyecto. |
   | Obligatorio           | Activado               | Casilla que indica si es obligatorio especificar un valor para este parámetro antes de completar el comando.          |
   | Plantilla de respuesta  | ¿Qué dispositivo?     | Pregunta para solicitar el valor de este parámetro cuando no se conoce.                                       |
   | Tipo               | String                | Tipo de parámetro, como número, cadena o fecha y hora.                                               |
   | Configuración      | Lista de cadenas           | En el caso de las cadenas, una lista de cadenas limita las entradas a un conjunto de valores posibles.                                      |
   | Valores de la lista de cadenas. | TV, ventilador               | Para un parámetro de lista de cadenas, el conjunto de valores posibles y sus sinónimos.                                |
   | Sinónimos (TV)      | televisión, tele     | Sinónimos opcionales para cada valor posible de un parámetro de lista de cadenas.                                      |

## <a name="add-sample-sentences"></a>Adición de oraciones de ejemplo

Con los parámetros, resulta útil agregar oraciones de ejemplo que cubran todas las combinaciones posibles. Por ejemplo:

1. Información del parámetro completa: `"turn {OnOff} the {SubjectDevice}"`.
1. Información del parámetro parcial: `"turn it {OnOff}"`.
1. Información del parámetro faltante: `"turn something"`.

Las oraciones de ejemplo con diferentes cantidades de información permiten que la aplicación de comandos personalizados resuelva las resoluciones de una sola captura y las resoluciones multigiro con información parcial.

Teniendo esto en cuenta, edite las oraciones de ejemplo para usar los parámetros como se sugiere a continuación.

> [!TIP]
> En el editor de oraciones de ejemplo, use llaves para hacer referencia a los parámetros. - `turn {OnOff} the {SubjectDevice}` Use la finalización mediante tabulación para hacer referencia a los parámetros creados previamente.

> [!div class="mx-imgBorder"]
> ![Oraciones de ejemplo con parámetros](media/custom-speech-commands/create-parameter-sentences.png)

```
turn {OnOff} the {SubjectDevice}
{SubjectDevice} {OnOff}
turn it {OnOff}
turn something {OnOff}
turn something
```

## <a name="add-parameters-to-completion-rule"></a>Adición de parámetros a la regla de finalización

Modifique la regla de finalización que creó en [la guía de inicio rápido anterior](./quickstart-custom-speech-commands-create-new.md):

1. Agregue una nueva condición y seleccione el parámetro obligatorio. Seleccione `OnOff` y `SubjectDevice`.
1. Edite la acción de respuesta de voz para que use `OnOff` y `SubjectDevice`:

   ```
   - Ok, turning {OnOff} the {SubjectDevice}
   ```

## <a name="try-it-out"></a>Prueba

Abra el panel de chat de prueba y pruebe algunas interacciones.

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
