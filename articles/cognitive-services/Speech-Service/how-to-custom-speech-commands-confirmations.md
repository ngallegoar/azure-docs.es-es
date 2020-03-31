---
title: 'Procedimientos: Adición de una confirmación a un comando personalizado (versión preliminar)'
titleSuffix: Azure Cognitive Services
description: En este artículo, se explica cómo implementar confirmaciones para un comando en Comandos personalizados.
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: encorona
ms.openlocfilehash: afa197c83b4f66f12863de4185ef7763447f3ed9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "75453367"
---
# <a name="how-to-add-a-confirmation-to-a-custom-command-preview"></a>Instrucciones: Adición de una confirmación a un comando personalizado (versión preliminar)

En este artículo, aprenderá a agregar una confirmación a un comando.

## <a name="prerequisites"></a>Prerrequisitos

Debe haber completado los pasos descritos en los siguientes artículos:

- [Inicio rápido: Creación de un comando personalizado (versión preliminar)](./quickstart-custom-speech-commands-create-new.md)
- [Inicio rápido: Creación de un comando personalizado con parámetros (versión preliminar)](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-setalarm-command"></a>Creación de un comando SetAlarm

Para demostrar las validaciones, vamos a crear un nuevo comando que permita al usuario establecer una alarma.

1. Abra la aplicación de comandos personalizados creada anteriormente en [Speech Studio](https://speech.microsoft.com/)
1. Creación de un nuevo comando **SetAlarm**
1. Adición de un parámetro llamado DateTime

   | Configuración           | Valor sugerido                                          | Descripción                                                                                      |
   | ----------------- | ---------------------------------------------------------| ------------------------------------------------------------------------------------------------ |
   | Nombre              | DateTime                                                 | Nombre descriptivo para el parámetro de comando.                                                    |
   | Obligatorio          | true                                                     | Casilla que indica si es obligatorio especificar un valor para este parámetro antes de completar el comando. |
   | Plantilla de respuesta | "- ¿A qué hora?"                                           | Pregunta para solicitar el valor de este parámetro cuando no se conoce.                              |
   | Tipo              | DateTime                                                 | Tipo de parámetro, como número, cadena o fecha y hora.                                      |
   | Valores predeterminados de fecha     | Si falta la fecha, use hoy                             |                                                                                                  |
   | Valores predeterminados de hora     | Si falta la hora, use inicio del día                      |                                                                                                  | 

1. Agregue algunas oraciones de ejemplo.
   
   ```
    set an alarm for {DateTime}
    set alarm {DateTime}
    alarm for {DateTime}
   ```

1. Agregue una regla de finalización para confirmar el resultado.

   | Configuración    | Valor sugerido                                         | Descripción                                        |
   | ---------- | ------------------------------------------------------- | -------------------------------------------------- |
   | Nombre de la regla  | Establecer alarma                                               | Nombre que describe el propósito de la regla.          |
   | Acciones    | SpeechResponse: "- De acuerdo, alarma establecida para el {FechaHora}"       | Acción que se realizará cuando la condición de la regla sea true. |

## <a name="try-it-out"></a>Prueba

Seleccione el panel de prueba y pruebe algunas interacciones.

- Entrada: Establecer alarma para mañana a mediodía
- Salida: "De acuerdo, alarma establecida para el 06/12/2019 a las 12:00:00"

- Entrada: Establecer una alarma
- Salida: "- ¿A qué hora?"
- Entrada: 17:00
- Salida: "De acuerdo, alarma establecida para el 05/12/2019 a las 17:00:00"

## <a name="add-the-advanced-rules-for-confirmation"></a>Agregar las reglas avanzadas para la confirmación

1. Agregue una regla avanzada para la confirmación. 

    Esta regla pedirá al usuario que confirme la fecha y la hora de la alarma y espera una confirmación (sí/no) para el siguiente turno.

   | Configuración               | Valor sugerido                                                                  | Descripción                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Nombre de la regla             | Confirmar fecha y hora                                                                | Nombre que describe el propósito de la regla.          |
   | Condiciones            | Parámetro obligatorio: DateTime                                                    | Condiciones que determinan cuándo se puede ejecutar la regla.    |   
   | Acciones               | SpeechResponse: "- ¿Seguro que desea establecer una alarma para el {FechaHora}?"       | Acción que se realizará cuando la condición de la regla sea true. |
   | Estado después de la ejecución | Espera de la entrada de datos                                                                   | Estado del usuario después del turno                  |
   | Expectativas          | Confirmación                                                                     | Expectativa para el siguiente turno                      |

1. Agregar una regla avanzada para controlar una confirmación correcta (el usuario dijo sí)

   | Configuración               | Valor sugerido                                                                  | Descripción                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Nombre de la regla             | Confirmación aceptada                                                            | Nombre que describe el propósito de la regla.          |
   | Condiciones            | SuccessfulConfirmation y parámetro obligatorio: DateTime                           | Condiciones que determinan cuándo se puede ejecutar la regla.    |   
   | Estado después de la ejecución | Lista para la finalización                                                             | Estado del usuario después del turno                   |

1. Agregar una regla avanzada para controlar una confirmación denegada (el usuario dijo no)

   | Configuración               | Valor sugerido                                                                  | Descripción                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Nombre de la regla             | Confirmación denegada                                                                   | Nombre que describe el propósito de la regla.          |
   | Condiciones            | DeniedConfirmation y parámetro obligatorio: DateTime                               | Condiciones que determinan cuándo se puede ejecutar la regla.    |   
   | Acciones               | ClearParameter: DateTime y SpeechResponse: "- Ningún problema, ¿a qué hora entonces?"     | Acción que se realizará cuando la condición de la regla sea true. |
   | Estado después de la ejecución | Espera de la entrada de datos                                                                   | Estado del usuario después del turno                   |
   | Expectativas          | ElicitParameters: DateTime                                                      | Expectativa para el siguiente turno                      |

## <a name="try-it-out"></a>Prueba

Seleccione el panel de prueba y pruebe algunas interacciones.

- Entrada: Establecer alarma para mañana a mediodía
- Salida: "¿Seguro que desea establecer una alarma para el 07/12/2019 a las 12:00:00?"
- Entrada: No
- Salida: "Ningún problema, ¿a qué hora entonces?"
- Entrada: 17:00
- Salida: "Seguro que desea establecer una alarma para el 06/12/2019 a las 17:00:00?"
- Entrada: Sí
- Salida: "De acuerdo, alarma establecida para el 06/12/2019 a las 17:00:00"

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Cómo: Agregar una corrección de un paso a un comando personalizado (versión preliminar)](./how-to-custom-speech-commands-one-step-correction.md)