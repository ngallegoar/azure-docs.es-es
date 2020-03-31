---
title: 'Procedimientos: Agregar una corrección de un paso a un comando personalizado (versión preliminar) - Servicio de voz'
titleSuffix: Azure Cognitive Services
description: En este artículo, se explica cómo implementar correcciones de un paso para un comando en Comandos personalizados.
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: encorona
ms.openlocfilehash: 86a12bd1dccc2b6ac15010546d7e990b768ebc02
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "75453351"
---
# <a name="how-to-add-a-one-step-correction-to-a-custom-command-preview"></a>Instrucciones: Agregar una corrección de un paso a un comando personalizado (versión preliminar)

En este artículo, aprenderá a agregar una confirmación en un paso a un comando.

La corrección en un paso se usa para actualizar un comando que se acaba de completar.

Por ejemplo, si acaba de configurar una alarma, puede cambiar de opinión y actualizar la hora de esta.

- Entrada: Establecer alarma para mañana a mediodía
- Salida: "De acuerdo, alarma establecida para el 06/12/2019 a las 12:00:00"
- Entrada: No, mañana a las 13:00
- Salida: "Vale"

Tenga en cuenta que esto implica que usted, como desarrollador, tiene un mecanismo para actualizar la alarma en la aplicación de back-end.

## <a name="prerequisites"></a>Prerrequisitos

Debe haber completado los pasos descritos en los siguientes artículos:

- [Inicio rápido: Creación de un comando personalizado (versión preliminar)](./quickstart-custom-speech-commands-create-new.md)
- [Inicio rápido: Creación de un comando personalizado con parámetros (versión preliminar)](./quickstart-custom-speech-commands-create-parameters.md)
- [Cómo: Adición de una confirmación a un comando personalizado (versión preliminar)](./how-to-custom-speech-commands-confirmations.md)

## <a name="add-the-advanced-rules-for-one-step-correction"></a>Incorporación de reglas avanzadas para la corrección en un paso 

Para demostrar la corrección en un paso, vamos a ampliar el comando **SetAlarm** creado en el [procedimiento de confirmación](./how-to-custom-speech-commands-confirmations.md).
 
1. Agregue una regla avanzada para actualizar la alarma anterior. 

    Esta regla pedirá al usuario que confirme la fecha y la hora de la alarma y espera una confirmación (sí/no) para el siguiente turno.

   | Configuración               | Valor sugerido                                                  | Descripción                                        |
   | --------------------- | ---------------------------------------------------------------- | -------------------------------------------------- |
   | Nombre de la regla             | Actualizar alarma anterior                                            | Nombre que describe el propósito de la regla.          |
   | Condiciones            | UpdateLastCommand y parámetro obligatorio: DateTime                | Condiciones que determinan cuándo se puede ejecutar la regla.    |   
   | Acciones               | SpeechResponse: "actualizando la alarma anterior a {DateTime}"       | Acción que se realizará cuando la condición de la regla sea true. |
   | Estado después de la ejecución | Comando Completar                                                 | Estado del usuario después del turno                   |

1. Mueva la regla que acaba de crear a la parte superior de las reglas avanzadas (desplácese por la regla en el panel y haga clic en la flecha Arriba).
   > [!div class="mx-imgBorder"]
   > ![Agregue una validación de intervalo](media/custom-speech-commands/one-step-correction-rules.png).

> [!NOTE]
> En una aplicación real, en la sección Acciones de esta regla también enviará una actividad al cliente o llamará a un punto de conexión HTTP para actualizar la alarma en el sistema.

## <a name="try-it-out"></a>Prueba

Seleccione el panel de prueba y pruebe algunas interacciones.

- Entrada: Establecer alarma para mañana a mediodía
- Salida: "¿Seguro que desea establecer una alarma para el 07/12/2019 a las 12:00:00?"
- Entrada: Sí
- Salida: "De acuerdo, alarma establecida para el 07/12/2019 a las 12:00:00"
- Entrada: No, mañana a las 13:00
- Salida: "Actualizando la alarma anterior al 12/07/2019 a las 13:00:00"
