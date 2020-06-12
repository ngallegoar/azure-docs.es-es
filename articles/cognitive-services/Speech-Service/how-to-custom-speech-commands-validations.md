---
title: 'Adición de validaciones en Comandos personalizados (versión preliminar): servicio Voz'
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo agregar validaciones a un parámetro de comando en una aplicación de versión preliminar de Comandos personalizados.
services: cognitive-services
author: nitinme
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: nitinme
ms.openlocfilehash: eb011510a9f636aea9910a4be445cd094acf0c21
ms.sourcegitcommit: 5504d5a88896c692303b9c676a7d2860f36394c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2020
ms.locfileid: "84509583"
---
# <a name="add-validations-to-a-command-parameter-in-a-custom-commands-preview-application"></a>Adición de validaciones a un parámetro de comando en una aplicación de versión preliminar de Comandos personalizados

En este artículo, aprenderá a agregar validaciones a los parámetros y a solicitar correcciones.

## <a name="prerequisites"></a>Requisitos previos

Siga los pasos descritos en estos artículos:

> [!div class="checklist"]
 
> * [Inicio rápido: Creación de una aplicación de comandos personalizados (versión preliminar)](./quickstart-custom-speech-commands-create-new.md)
> * [Inicio rápido: Creación de una aplicación de Comandos personalizados (versión preliminar) con parámetros](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-settemperature-command"></a>Creación de un comando SetTemperature

Para demostrar las validaciones, vamos a crear un nuevo comando que permita a los usuarios establecer la temperatura.

1. En [Speech Studio](https://speech.microsoft.com/), abra la aplicación de Comandos personalizados (versión preliminar) que creó.
1. Creación de un comando **SetTemperature**.
1. Agregue un parámetro de temperatura que tenga la siguiente configuración:

   | Configuración de parámetros           | Valor sugerido    |Descripción                 |                                    
   | ----------------- | ----------------------------------| -------------|
   | **Nombre**              | **Temperatura**                       | Nombre descriptivo para el parámetro                                |
   | **Obligatorio**          | Activado                           | Casilla que indica si es obligatorio especificar un valor para este parámetro antes de completar el comando. |
   | **Respuesta para el parámetro requerido**     | **Editor sencillo: ¿Qué temperatura le gustaría?**  | Pregunta para solicitar el valor de este parámetro cuando no se conoce. |
   | **Tipo**              | **Number**                            | Tipo de parámetro; por ejemplo, Number, String, DateTime o Geography.   |

1. Agregue una validación para el parámetro de temperatura.

    1. En la página de configuración **Parámetros** para el parámetro de temperatura, seleccione **Agregar validación** en la sección **Validaciones**.

    1. En la ventana emergente de **nueva validación**, configure la validación de la siguiente manera:
  
       | Configuración de parámetros         | Valor sugerido                                          | Descripción                                                                        |
       | ----------------- | -------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
       | **Valor mínimo**        | **60**               | En el caso de los parámetros numéricos, el valor mínimo que puede asumir este parámetro |
       | **Valor máximo**        | **80**               | En el caso de los parámetros numéricos, el valor máximo que puede asumir este parámetro |
       | **Respuesta de error: Editor sencillo**| **Primera variación: Sorry, I can only set between 60 and 80 degrees (El valor solo puede establecerse entre 60 y 80 grados)**      | Mensaje para solicitar un valor nuevo si se produce un error en la validación                                       |

       > [!div class="mx-imgBorder"]
       > ![Agregue una validación de intervalo](media/custom-speech-commands/validations-add-temperature.png).

1. Seleccione **Crear**.

1. Agregue algunas oraciones de ejemplo.

   ```
   set the temperature to {Temperature} degrees
   change the temperature to {Temperature}
   set the temperature
   change the temperature
   ```

1. Agregue una regla de finalización con la configuración siguiente. Esta regla confirma el resultado.

   | Configuración    | Valor sugerido                                           |Descripción                                     |
   | ---------- | --------------------------------------------------------- |-----|
   | Nombre       | Mensaje de confirmación                                      |Nombre que describe el propósito de la regla. |
   | **Condiciones** | **Parámetros obligatorios: Temperatura**                       |Condiciones que determinan cuándo se puede ejecutar la regla.    |   
   | **Acciones**    | **Enviar respuesta de voz: Ok, setting temperature to {Temperature} degrees** (De acuerdo, definiendo la temperatura en {temperatura} grados) | Acción que se realizará cuando la condición de la regla sea true. |

> [!TIP]
> En este ejemplo se utiliza una respuesta de voz para confirmar el resultado. Para obtener ejemplos sobre cómo completar el comando con una acción de cliente, consulte [ Ejecución de comandos en el cliente con el SDK de Voz](./how-to-custom-speech-commands-fulfill-sdk.md)

## <a name="try-it-out"></a>Prueba

1. Seleccione **Entrenar**.

1. Una vez que ha finalizado el entrenamiento, seleccione **Probar** y, después, pruebe estas interacciones:

    - Entrada: establezca la temperatura en 72 grados.
    - Salida: Ok, setting temperature to 72 degrees (De acuerdo, definiendo la temperatura en 72 grados)
    - Entrada: establezca la temperatura en 45 grados.
    - Salida: Sorry, I can only set between 60 and 80 degrees (El valor solo puede establecerse entre 60 y 80 grados)
    - Entrada: cámbiala a 72 grados en su lugar.
    - Salida: Ok, setting temperature to 72 degrees (De acuerdo, definiendo la temperatura en 72 grados)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Agregar confirmaciones a un comando en una aplicación de Comandos personalizados (versión preliminar)](./how-to-custom-speech-commands-confirmations.md)
