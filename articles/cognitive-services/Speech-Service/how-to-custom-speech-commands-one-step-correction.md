---
title: 'Adición de una corrección en un paso a Comandos personalizados (versión preliminar): servicio Voz'
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo agregar una corrección en un paso para una aplicación de Comandos personalizados (versión preliminar).
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: encorona
ms.openlocfilehash: 05f63ba4e70f649df33905f1e92fb1fab866a86c
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84310434"
---
# <a name="add-a-one-step-correction-to-a-custom-command-in-a-custom-commands-preview-application"></a>Adición de una corrección en un paso a un comando personalizado en una aplicación de Comandos personalizados (versión preliminar)

En este artículo, aprenderá a agregar una confirmación en un paso a un comando en una aplicación de Comandos personalizados (versión preliminar).

La corrección en un paso se usa para actualizar un comando que se acaba de completar. Al agregar una corrección en un paso a un comando de alarma, puede cambiar de opinión y actualizar la hora de la alarma. Por ejemplo:

- Entrada: Establecer alarma para mañana a mediodía
- Salida: Ok, alarm set for 2020-05-02 12:00:00 (De acuerdo, alarma establecida para el 05/02/2020 a las 12:00:00)
- Entrada: No, mañana a las 13:00
- Salida: Aceptar

> [!NOTE]
> En un escenario real, el cliente ejecuta una acción como resultado de la finalización del comando. En este artículo se supone que tiene un mecanismo para actualizar la alarma en la aplicación de back-end.

## <a name="prerequisites"></a>Requisitos previos

Complete los pasos descritos en los artículos siguientes:
> [!div class="checklist"]

> * [Inicio rápido: Creación de una aplicación de Comandos personalizados (versión preliminar)](./quickstart-custom-speech-commands-create-new.md)
> * [Inicio rápido: Creación de una aplicación de Comandos personalizados (versión preliminar) con parámetros](./quickstart-custom-speech-commands-create-parameters.md)
> * [Cómo: agregar confirmaciones a un comando en una aplicación de Comandos personalizados (versión preliminar)](./how-to-custom-speech-commands-confirmations.md)

## <a name="add-interaction-rules-for-one-step-correction"></a>Incorporación de reglas de interacción para la corrección en un paso

Para demostrar la corrección en un paso, amplíe el comando **SetAlarm** creado en el [Procedimiento para agregar una confirmación a un comando en Comandos personalizados (versión preliminar)](./how-to-custom-speech-commands-confirmations.md).

1. Agregue una regla de interacción para actualizar el comando **SetAlarm**.

    Esta regla le pide al usuario que confirme la fecha y la hora de la alarma, y espera una confirmación (sí/no) para el turno siguiente.

   | Configuración               | Valor sugerido                                                  | Descripción                                        |
   | --------------------- | ---------------------------------------------------------------- | -------------------------------------------------- |
   | **Nombre de la regla**             | **Actualizar alarma anterior**                                            | Nombre que describe el propósito de la regla.          |
   | **Condiciones**            | **Es necesario actualizar el comando anterior y se requiere el parámetro DateTime**                | Condiciones que determinan cuándo se puede ejecutar la regla.    |   
   | **Acciones**               | **Enviar respuesta de voz -> Editor sencillo -> Actualizar la alarma anterior a {DateTime}**      | Acción que se realizará si las condiciones de la regla tienen el valor true |
   | **Estado posterior a la ejecución** | **Comando completado**        | Estado del usuario después del turno                   |

1. En el panel, seleccione la regla de interacción que acaba de crear. Seleccione el botón de puntos suspensivos ( **...** ) de la esquina superior izquierda del panel. Después, use la flecha **Subir** para desplazar la regla a la parte superior de la lista **Reglas de interacción**.
   > [!div class="mx-imgBorder"]
   > ![Agregue una validación de intervalo](media/custom-speech-commands/one-step-correction-rules.png).

    > [!NOTE]
    > En una aplicación del mundo real, use la sección **Acciones** para devolver una actividad al cliente o llamar a un punto de conexión HTTP para actualizar la alarma en el sistema.

## <a name="try-it-out"></a>Prueba

1. Seleccione **Entrenar**.

1. Una vez que haya finalizado el entrenamiento, seleccione **Probar** y, después, pruebe estas interacciones:

   - Entrada: Establecer alarma para mañana a mediodía
   - Salida: Are you sure you want to set an alarm for 2020-05-02 12:00:00 (¿Seguro que desea establecer una alarma para el 05/02/2020 a las 12:00:00?)
   - Entrada: Sí
   - Salida: Ok, alarm set for 2020-05-02 12:00:00 (De acuerdo, alarma establecida para el 05/02/2020 a las 12:00:00)
   - Entrada: No, tomorrow at 2pm (No, mañana a las 14:00)
   - Salida: Updating previous alarm to 2020-05-02 14:00:00 (Actualizando la alarma anterior al 05/02/2020 a las 14:00:00)
