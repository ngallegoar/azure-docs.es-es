---
title: Adición de confirmaciones en una aplicación de comandos personalizados (versión preliminar) - Servicio de voz
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo agregar confirmaciones a los comandos de una aplicación de comandos personalizados (versión preliminar).
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: encorona
ms.openlocfilehash: 1cb0624012b22b6cae2c98bfa6ddc9495e09615d
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84310475"
---
# <a name="add-confirmations-to-a-command-in-a-custom-commands-preview-application"></a>Agregar confirmaciones a un comando en una aplicación de comandos personalizados (versión preliminar)

En este artículo, aprenderá a crear confirmaciones para los comandos de una aplicación de comandos personalizados (versión preliminar).

## <a name="prerequisites"></a>Requisitos previos

Siga los pasos descritos en estos artículos:
> [!div class="checklist"]
> * [Inicio rápido: Creación de una aplicación de comandos personalizados (versión preliminar)](./quickstart-custom-speech-commands-create-new.md)
> * [Inicio rápido: Creación de una aplicación de comandos personalizados (versión preliminar) con parámetros](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-setalarm-command"></a>Creación de un comando SetAlarm

Para demostrar las confirmaciones, cree un nuevo comando que establezca una alarma.

1. En [Speech Studio](https://speech.microsoft.com/), abra la aplicación de comandos personalizados (versión preliminar) que creó.
1. Cree un nuevo comando **SetAlarm**.
1. Agregue un parámetro **DateTime** que tenga esta configuración:

   | Configuración                           | Valor sugerido                     |  Descripción                 |
   | --------------------------------- | -----------------------------------------------------| ------------|
   | **Nombre**                              | **DateTime**                                | Nombre descriptivo del parámetro                                |
   | **Obligatorio**                          | Activado                                 | Casilla que indica si es obligatorio especificar un valor para este parámetro antes de completar el comando. |
   | **Respuesta para un parámetro obligatorio**   | **Editor sencillo -> What time? (¿Qué hora?)**                              | Pregunta para solicitar el valor de este parámetro cuando no se conoce. |
   | **Tipo**                              | **DateTime**                                | Tipo de parámetro; por ejemplo, Number, String, DateTime o Geography.   |
   | **Valores predeterminados de fecha**                     | Si falta la fecha, use la fecha actual.            | Valor predeterminado de la variable que se usará si el usuario no lo proporciona.  |  
   | **Valores predeterminados de hora**                     | Si falta la hora, use el inicio del día.     |  Valor predeterminado de la variable que se usará si el usuario no lo proporciona.|

1. Agregue algunas oraciones de ejemplo.
   
    ```
    set an alarm for {DateTime}
    set alarm {DateTime}
    alarm for {DateTime}
   ```

1. Agregue una regla de finalización para confirmar el resultado. Use la configuración siguiente:

   | Configuración    | Valor sugerido                               |Descripción                                     |
   | ---------- | ------------------------------------------------------- |-----|
   | **Nombre de la regla**  | **Establecer alarma**                                               |    Nombre que describe el propósito de la regla. |
   | **Acciones**    | **Enviar una respuesta de voz: OK, alarm set for {DateTime} (De acuerdo, alarma establecida para el {DateTime})**    |Acción que se realizará cuando la condición de la regla sea true.

## <a name="try-it-out"></a>Prueba

1. Seleccione **Entrenar** en la parte superior del panel derecho.

1. Una vez que haya finalizado el entrenamiento, seleccione **Probar** y luego pruebe estas interacciones:
    - Entrada: Establecer alarma para mañana a mediodía
    - Salida: Ok, alarm set for 2020-05-02 12:00:00 (De acuerdo, alarma establecida para el 02/05/2020 a las 12:00:00)
    - Entrada: Establecer una alarma
    - Salida: What time? (¿A qué hora?)
    - Entrada: 17:00
    - Salida: OK, alarm set for 2020-05-01 17:00:00 (De acuerdo, alarma establecida para el 01/05/2020 a las 17:00:00)

## <a name="add-interaction-rules-for-the-confirmation"></a>Agregue reglas de interacción para la confirmación.

Cree confirmaciones mediante la adición de reglas de interacción.

1. En el comando **SetAlarm**, seleccione **Agregar** en el panel central y luego seleccione **Reglas de interacción** > **Confirmar comando**.

    Esta regla solicita al usuario que confirme la fecha y la hora de la alarma. Use estos valores de configuración:

   | Configuración               | Valor sugerido                                                                  | Descripción                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | **Nombre de la regla**             | **Confirmar fecha y hora**                                                                | Nombre que describe el propósito de la regla.          |
   | **Condiciones**            | **Parámetro obligatorio -> DateTime**                                                    | Condiciones que determinan cuándo se puede ejecutar la regla.    |   
   | **Acciones**               | **Enviar respuesta de voz -> Are you sure you want to set an alarm for {DateTime}? (¿Seguro que quiere establecer una alarma para el {DateTime}?)**     | Acción que se realizará cuando la condición de la regla sea true. |
   | **Expectativas**          | **Esperar confirmación del usuario**                                                 | Expectativa para el siguiente turno                      |
   | **Estado posterior a la ejecución**  | **Esperar la entrada del usuario**                                                            | Estado del usuario después del turno                  |
  
      > [!div class="mx-imgBorder"]
      > ![Crear respuesta de parámetro requerida](media/custom-speech-commands/add-validation-set-temperature.png)

1. Agregue una regla de interacción para una confirmación aceptada (el usuario dijo "sí"). Use la configuración siguiente:

   | Configuración               | Valor sugerido                                                                  | Descripción                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | **Nombre de la regla**             | **Confirmación aceptada**                                                            | Nombre que describe el propósito de la regla.          |
   | **Condiciones**            | **Confirmación correcta y parámetro obligatorio -> DateTime**                      | Condiciones que determinan cuándo se puede ejecutar la regla.    |   
   | **Estado posterior a la ejecución** | **Ejecutar reglas de finalización**                                                          | Estado del usuario después del turno                   |

1. Agregue una regla de interacción para una confirmación denegada (el usuario dijo "no"). Use la configuración siguiente:

   | Configuración               | Valor sugerido                                                                  | Descripción                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | **Nombre de la regla**             | **Confirmación denegada**                                                                   | Nombre que describe el propósito de la regla.          |
   | **Condiciones**            | **Confirmación denegada y parámetro obligatorio -> DateTime**                               | Condiciones que determinan cuándo se puede ejecutar la regla.    |   
   | **Acciones**               | **Borrar valor de parámetro-> DateTime y enviar respuesta de voz -> No problem, what time then? (No hay problema, ¿a qué hora, entonces?)**  | Acción que se realizará cuando la condición de la regla sea true. |
   | **Estado después de la ejecución** | **Esperar la entrada de datos**                                                                   | Estado del usuario después del turno                   |
   | **Expectativas**          | **Se espera la entrada de parámetros del usuario -> DateTime**                           | Expectativa para el siguiente turno                      |

## <a name="try-out-the-changes"></a>Prueba de los cambios

1. Seleccione **Entrenar**.

1. Una vez que haya finalizado el entrenamiento, seleccione **Probar** y, después, pruebe estas interacciones:

    - Entrada: Establecer alarma para mañana a mediodía
    - Salida: Are you sure you want to set an alarm for 2020-05-02 12:00:00? (¿Seguro que quiere establecer una alarma para el 05/02/2020 a las 12:00:00?)
    - Entrada: No
    - Salida: No problem, what time then? (No hay problema, ¿a qué hora, entonces?)
    - Entrada: 17:00
    - Salida: "Are you sure you want to set an alarm for 2020-05-01 17:00:00?" (¿Seguro que quiere establecer una alarma para el 05/01/2020 a las 17:00:00?)
    - Entrada: Sí
    - Salida: OK, alarm set for 2020-05-01 17:00:00 (De acuerdo, alarma establecida para el 01/05/2020 a las 17:00:00)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Adición de una corrección en un paso a un comando en una aplicación de comandos personalizados (versión preliminar)](./how-to-custom-speech-commands-one-step-correction.md)
