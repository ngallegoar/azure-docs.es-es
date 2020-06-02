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
ms.openlocfilehash: bf1b79c1b5d7b9dfd93b354c6b6ff5a512bb74a5
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858242"
---
# <a name="how-to-add-a-confirmation-to-a-custom-command-preview"></a>Instrucciones: Adición de una confirmación a un comando personalizado (versión preliminar)

En este artículo, aprenderá a agregar una confirmación a un comando.

## <a name="prerequisites"></a>Prerrequisitos

Debe haber completado los pasos descritos en los siguientes artículos:
> [!div class="checklist"]
> *  [Inicio rápido: Creación de un comando personalizado (versión preliminar)](./quickstart-custom-speech-commands-create-new.md)
> * [Inicio rápido: Creación de un comando personalizado con parámetros (versión preliminar)](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-setalarm-command"></a>Creación de un comando SetAlarm

Para demostrar las confirmaciones, vamos a crear un nuevo comando que permita al usuario establecer una alarma.

1. Abra la aplicación de comandos personalizados creada anteriormente en [Speech Studio](https://speech.microsoft.com/).
1. Cree un comando nuevo `SetAlarm`.
1. Agregue un parámetro denominado `DateTime` con la siguiente configuración.

   | Configuración                           | Valor sugerido                     |  Descripción                 |
   | --------------------------------- | -----------------------------------------------------| ------------|
   | Nombre                              | DateTime                                | Nombre descriptivo para el parámetro                                |
   | Obligatorio                          | Activado                                 | Casilla que indica si es obligatorio especificar un valor para este parámetro antes de completar el comando. |
   | Respuesta para el parámetro requerido   | Editor sencillo -> What time? (¿Qué hora?)                              | Pregunta para solicitar el valor de este parámetro cuando no se conoce. |
   | Tipo                              | DateTime                                | Tipo de parámetro; por ejemplo, Number, String, Date Time o Geography   |
   | Valores predeterminados de fecha                     | Si falta la fecha, use hoy            | Valor predeterminado de la variable que se usará si el usuario no lo proporciona.  |  
   | Valores predeterminados de hora                     | Si falta la hora, use inicio del día     |  Valor predeterminado de la variable que se usará si el usuario no lo proporciona.|

1. Agregue algunas oraciones de ejemplo.
   
    ```
    set an alarm for {DateTime}
    set alarm {DateTime}
    alarm for {DateTime}
   ```

1. Agregue una regla de finalización para confirmar el resultado.

   | Configuración    | Valor sugerido                               |Descripción                                     |
   | ---------- | ------------------------------------------------------- |-----|
   | Nombre de la regla  | Establecer alarma                                               |    Nombre que describe el propósito de la regla. |
   | Acciones    | Enviar una respuesta de voz: Ok, alarm set for {DateTime} (De acuerdo, alarma establecida para el {FechaHora})"    |Acción que se realizará cuando la condición de la regla sea true.

## <a name="try-it-out"></a>Prueba

1. Seleccione el icono `Train` presente en la parte superior del panel derecho.

1. Una vez completado el entrenamiento, seleccione `Test`.
    - Entrada: Establecer alarma para mañana a mediodía
    - Salida: Ok, alarm set for 2020-05-02 12:00:00 (De acuerdo, alarma establecida para el 05/02/2020 a las 12:00:00)
    - Entrada: Establecer una alarma
    - Salida: What time? (¿A qué hora?)
    - Entrada: 17:00
    - Salida: Ok, alarm set for 2020-05-01 17:00:00 (De acuerdo, alarma establecida para el 05/01/2020 a las 17:00:00)

## <a name="add-the-advanced-rules-for-confirmation"></a>Agregar las reglas avanzadas para la confirmación

Para proporcionar confirmaciones, es necesario agregar reglas de interacciones.

1. En el comando `SetAlarm` existente, agregue una **regla de interacción**. Para hacerlo, seleccione el icono `+Add` en el panel central y, a continuación, **Interaction rules** (Reglas de interacción)  -> **Confirm command** (Confirmar comando).

    Esta regla pedirá al usuario que confirme la fecha y la hora de la alarma y espera una confirmación (sí/no) para el siguiente turno.

   | Configuración               | Valor sugerido                                                                  | Descripción                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Nombre de la regla             | Confirmar fecha y hora                                                                | Nombre que describe el propósito de la regla.          |
   | Condiciones            | Parámetro obligatorio -> DateTime (FechaHora)                                                    | Condiciones que determinan cuándo se puede ejecutar la regla.    |   
   | Acciones               | Enviar respuesta de voz -> Are you sure you want to set an alarm for {DateTime}? (¿Seguro que quiere establecer una alarma para {FechaHora}?)     | Acción que se realizará cuando la condición de la regla sea true. |
   | Expectativas          | Esperar confirmación del usuario                                                 | Expectativa para el siguiente turno                      |
   | Estado posterior a la ejecución  | Esperar la entrada del usuario                                                            | Estado del usuario después del turno                  |
  
      > [!div class="mx-imgBorder"]
      > ![Crear respuesta de parámetro requerida](media/custom-speech-commands/add-validation-set-temperature.png)

1. Agregue otra regla de interacción para controlar una confirmación correcta (el usuario dijo sí).

   | Configuración               | Valor sugerido                                                                  | Descripción                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Nombre de la regla             | Confirmación aceptada                                                            | Nombre que describe el propósito de la regla.          |
   | Condiciones            | Confirmación correcta y parámetro obligatorio -> DateTime (FechaHora)                      | Condiciones que determinan cuándo se puede ejecutar la regla.    |   
   | Estado posterior a la ejecución | Ejecutar reglas de finalización                                                          | Estado del usuario después del turno                   |

1. Agregar una regla avanzada para controlar una confirmación denegada (el usuario dijo no)

   | Configuración               | Valor sugerido                                                                  | Descripción                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Nombre de la regla             | Confirmación denegada                                                                   | Nombre que describe el propósito de la regla.          |
   | Condiciones            | Confirmación denegada y parámetro obligatorio -> DateTime (FechaHora)                               | Condiciones que determinan cuándo se puede ejecutar la regla.    |   
   | Acciones               | Borrar valor de parámetro-> DateTime (FechaHora) y enviar respuesta de voz -> No problem, what time then? (No hay problema, ¿a qué hora, entonces?)  | Acción que se realizará cuando la condición de la regla sea true. |
   | Estado después de la ejecución | Espera de la entrada de datos                                                                   | Estado del usuario después del turno                   |
   | Expectativas          | Se espera la entrada de parámetros del usuario -> DateTime (FechaHora)                           | Expectativa para el siguiente turno                      |

## <a name="try-out-the-changes"></a>Prueba de los cambios

Seleccione `Train`, espere a que se complete el entrenamiento y elija `Test`.

- Entrada: Establecer alarma para mañana a mediodía
- Salida: Are you sure you want to set an alarm for 2020-05-02 12:00:00? (¿Seguro que quiere establecer una alarma para el 05/02/2020 a las 12:00:00?)
- Entrada: No
- Salida: No problem, what time then? (No hay problema, ¿a qué hora, entonces?)
- Entrada: 17:00
- Salida: "Are you sure you want to set an alarm for 2020-05-01 17:00:00?" (¿Seguro que quiere establecer una alarma para el 05/01/2020 a las 17:00:00?)
- Entrada: Sí
- Salida: Ok, alarm set for 2020-05-01 17:00:00 (De acuerdo, alarma establecida para el 05/01/2020 a las 17:00:00)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Cómo: Agregar una corrección de un paso a un comando personalizado (versión preliminar)](./how-to-custom-speech-commands-one-step-correction.md)