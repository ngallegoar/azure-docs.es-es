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
ms.openlocfilehash: f43c28d314cb8a0211496664cd20d2c380e4d5b0
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858278"
---
# <a name="how-to-add-a-one-step-correction-to-a-custom-command-preview"></a>Instrucciones: Agregar una corrección de un paso a un comando personalizado (versión preliminar)

En este artículo, aprenderá a agregar una confirmación en un paso a un comando.

La corrección en un paso se usa para actualizar un comando que se acaba de completar. Es decir, si acaba de configurar una alarma, puede cambiar de opinión y actualizar la hora establecida. A continuación se incluye un ejemplo de este caso.

- Entrada: Establecer alarma para mañana a mediodía
- Salida: Ok, alarm set for 2020-05-02 12:00:00 (De acuerdo, alarma establecida para el 05/02/2020 a las 12:00:00)
- Entrada: No, mañana a las 13:00
- Salida: Aceptar

Un escenario real, que suele ir acompañado de la ejecución de una acción por parte del cliente como resultado de la aplicación de un comando. En este artículo se da por sentado que, como desarrollador, tiene un mecanismo para actualizar la alarma en la aplicación del back-end.

## <a name="prerequisites"></a>Prerrequisitos

Debe haber completado los pasos descritos en los siguientes artículos:
> [!div class="checklist"]

> * [Inicio rápido: Creación de un comando personalizado (versión preliminar)](./quickstart-custom-speech-commands-create-new.md)
> * [Inicio rápido: Creación de un comando personalizado con parámetros (versión preliminar)](./quickstart-custom-speech-commands-create-parameters.md)
> * [Cómo: Adición de una confirmación a un comando personalizado (versión preliminar)](./how-to-custom-speech-commands-confirmations.md)


## <a name="add-interaction-rules-for-one-step-correction"></a>Incorporación de reglas de interacción para la corrección en un paso 

Para demostrar la corrección en un paso, vamos a ampliar el comando **SetAlarm** creado en el [Procedimiento: Adición de una confirmación a un comando personalizado (versión preliminar)](./how-to-custom-speech-commands-confirmations.md).
1. Agregue una regla de interacción para actualizar la alarma establecida previamente. 

    Esta regla pedirá al usuario que confirme la fecha y la hora de la alarma y espera una confirmación (sí/no) para el siguiente turno.

   | Configuración               | Valor sugerido                                                  | Descripción                                        |
   | --------------------- | ---------------------------------------------------------------- | -------------------------------------------------- |
   | Nombre de la regla             | Actualizar alarma anterior                                            | Nombre que describe el propósito de la regla.          |
   | Condiciones            | Previous command needs to be updated & Required Parameter -> DateTime (Es necesario actualizar el comando anterior y se requiere el parámetro DateTime)                | Condiciones que determinan cuándo se puede ejecutar la regla.    |   
   | Acciones               | Send speech response -> Simple editor -> Updating previous alarm to {DateTime} (Enviar respuesta de voz -> Editor sencillo -> Actualizando la alarma anterior a {DateTime})      | Acción que se realizará si las condiciones de la regla tienen el valor true |
   | Estado posterior a la ejecución | Command completed (Comando completado)        | Estado del usuario después del turno                   |

1. Mueva la regla que acaba de crear a la parte superior de las reglas de interacción (seleccione la regla en el panel y haga clic en la flecha arriba que aparece bajo el icono `...` en la parte superior del panel central).
   > [!div class="mx-imgBorder"]
   > ![Agregue una validación de intervalo](media/custom-speech-commands/one-step-correction-rules.png)
    > .[!NOTE]
    > En una aplicación real, en la sección Acciones de esta regla también enviará una actividad al cliente o llamará a un punto de conexión HTTP para actualizar la alarma en el sistema.

## <a name="try-it-out"></a>Prueba

Seleccione `Train`, espere a que se complete el entrenamiento y elija `Test`.

- Entrada: Establecer alarma para mañana a mediodía
- Salida: Are you sure you want to set an alarm for 2020-05-02 12:00:00 (¿Seguro que desea establecer una alarma para el 05/02/2020 a las 12:00:00?)
- Entrada: Sí
- Salida: Ok, alarm set for 2020-05-02 12:00:00 (De acuerdo, alarma establecida para el 05/02/2020 a las 12:00:00)
- Entrada: No, tomorrow at 2pm (No, mañana a las 14:00)
- Salida: Updating previous alarm to 2020-05-02 14:00:00 (Actualizando la alarma anterior al 05/02/2020 a las 14:00:00)
