---
title: 'Instrucciones: adición de validaciones a los parámetros de comandos personalizados (versión preliminar)'
titleSuffix: Azure Cognitive Services
description: En este artículo, se explica cómo agregar validaciones a un parámetro en comandos personalizados.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: donkim
ms.openlocfilehash: cf6e4e4f0bfab43fb738f8415022e55fcbcbd05a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "76156461"
---
# <a name="how-to-add-validations-to-custom-command-parameters-preview"></a>Instrucciones: adición de validaciones a los parámetros de comandos personalizados (versión preliminar)

En este artículo, aprenderá a agregar validaciones a los parámetros y a solicitar correcciones.

## <a name="prerequisites"></a>Prerrequisitos

Debe haber completado los pasos descritos en los siguientes artículos:

- [Inicio rápido: Creación de un comando personalizado (versión preliminar)](./quickstart-custom-speech-commands-create-new.md)
- [Inicio rápido: Creación de un comando personalizado con parámetros (versión preliminar)](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-settemperature-command"></a>Creación de un comando SetTemperature

Para demostrar las validaciones, vamos a crear un nuevo comando que permita al usuario establecer la temperatura.

1. Abra la aplicación de comandos personalizados creada anteriormente en [Speech Studio](https://speech.microsoft.com/)
1. Cree un nuevo comando **SetTemperature**.
1. Agregue un parámetro para la temperatura de destino.
1. Agregue una validación para el parámetro temperature.
   > [!div class="mx-imgBorder"]
   > ![Agregue una validación de intervalo](media/custom-speech-commands/validations-add-temperature.png).

   | Configuración           | Valor sugerido                                          | Descripción                                                                                      |
   | ----------------- | -------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
   | Nombre              | Temperatura                                              | Nombre descriptivo para el parámetro de comando.                                                    |
   | Obligatorio          | true                                                     | Casilla que indica si es obligatorio especificar un valor para este parámetro antes de completar el comando. |
   | Plantilla de respuesta | "- ¿Qué temperatura le gustaría?"                     | Pregunta para solicitar el valor de este parámetro cuando no se conoce.                              |
   | Tipo              | Number                                                   | Tipo de parámetro, como número, cadena o fecha y hora.                                      |
   | Validación        | Valor mínimo: 60, Valor máximo: 80                             | En el caso de los parámetros de número, el intervalo de valores permitido para el parámetro.                             |
   | Plantilla de respuesta | "- Lo sentimos, el valor solo puede establecerse entre 60 y 80 grados"      | Pregunta para solicitar un valor actualizado si se produce un error en la validación.                                       |

1. Agregue algunas oraciones de ejemplo.

   ```
   set the temperature to {Temperature} degrees
   change the temperature to {Temperature}
   set the temperature
   change the temperature
   ```

1. Agregue una regla de finalización para confirmar el resultado.

   | Configuración    | Valor sugerido                                           | Descripción                                        |
   | ---------- | --------------------------------------------------------- | -------------------------------------------------- |
   | Nombre de la regla  | Mensaje de confirmación                                      | Nombre que describe el propósito de la regla.          |
   | Condiciones | Parámetro obligatorio: temperature                          | Condiciones que determinan cuándo se puede ejecutar la regla.    |
   | Acciones    | SpeechResponse: "- De acuerdo, se establecerá en {temperature} grados" | Acción que se realizará cuando la condición de la regla sea true. |

> [!TIP]
> En este ejemplo se utiliza una respuesta de voz para confirmar el resultado. Para obtener ejemplos sobre cómo completar el comando con una acción de cliente, consulte: [Cómo: Fulfill Commands on the client with the Speech SDK (Preview)](./how-to-custom-speech-commands-fulfill-sdk.md) (Instrucciones: realización de comandos en el cliente con el SDK de Voz).

## <a name="try-it-out"></a>Prueba

Seleccione el panel de prueba y pruebe algunas interacciones.

- Entrada: establezca la temperatura en 72 grados.
- Salida: "de acuerdo, se establecerá en 72 grados".

- Entrada: establezca la temperatura en 45 grados.
- Salida: "Lo sentimos, el valor solo puede establecerse entre 60 y 80 grados"
- Entrada: cámbiala a 72 grados en su lugar.
- Salida: "de acuerdo, se establecerá en 72 grados".

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Cómo: Adición de una confirmación a un comando personalizado (versión preliminar)](./how-to-custom-speech-commands-confirmations.md)
