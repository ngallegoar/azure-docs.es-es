---
title: 'Instrucciones: Adición de validaciones a los parámetros de Comando personalizado'
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
ms.openlocfilehash: 2b7fd608156ab269cfc0c85c6c508fa9d5eebc83
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2020
ms.locfileid: "82857198"
---
# <a name="how-to-add-validations-to-custom-command-parameters-preview"></a>Instrucciones: adición de validaciones a los parámetros de comandos personalizados (versión preliminar)

En este artículo, agregará validaciones a los parámetros y solicitudes de corrección.

## <a name="prerequisites"></a>Prerrequisitos

Debe haber completado los pasos descritos en los siguientes artículos:

> [!div class="checklist"]
> * [Inicio rápido: Creación de un comando personalizado](./quickstart-custom-speech-commands-create-new.md)
> * [Inicio rápido: Creación de un comando personalizado con parámetros](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-settemperature-command"></a>Creación de un comando SetTemperature

Para demostrar las validaciones, vamos a crear un nuevo comando que permita a los usuarios establecer la temperatura.

1. Abra la aplicación de comandos personalizados creada anteriormente en [Speech Studio](https://speech.microsoft.com/)
1. Creación de un nuevo comando `SetTemperature`
1. Agregue un parámetro para la temperatura de destino.

   | Configuración de parámetros           | Valor sugerido    |Descripción                 |                                    
   | ----------------- | ----------------------------------| -------------|
   | Nombre              | Temperatura                       | Nombre descriptivo para el parámetro                                |
   | Obligatorio          | Activado                           | Casilla que indica si es obligatorio especificar un valor para este parámetro antes de completar el comando. |
   | Respuesta para el parámetro requerido     | Editor sencillo: What temperature would you like? (¿Qué temperatura le gustaría?)  | Pregunta para solicitar el valor de este parámetro cuando no se conoce. |
   | Tipo              | Number                            | Tipo de parámetro; por ejemplo, Number, String, Date Time o Geography.   |

1. Agregue una validación para el parámetro de temperatura.

    - En la página de configuración de **Parámetros** para el parámetro `Temperature`, seleccione `Add a validation` en la sección Validaciones.
    - Rellene los valores de la ventana emergente **New validation** (Nueva validación) como se indica a continuación y seleccione **Crear**.

  
       | Configuración de parámetros         | Valor sugerido                                          | Descripción                                                                        |
       | ----------------- | -------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
       | Valor mínimo        | 60               | En el caso de los parámetros numéricos, el valor mínimo que puede asumir este parámetro |
       | Valor máximo        | 80               | En el caso de los parámetros numéricos, el valor máximo que puede asumir este parámetro |
       | Respuesta de error: Editor sencillo| Primera variación: El valor solo puede establecerse entre 60 y 80 grados      | Mensaje para solicitar un valor nuevo si se produce un error en la validación                                       |

       > [!div class="mx-imgBorder"]
       > ![Agregue una validación de intervalo](media/custom-speech-commands/validations-add-temperature.png).

1. Agregue algunas oraciones de ejemplo.

   ```
   set the temperature to {Temperature} degrees
   change the temperature to {Temperature}
   set the temperature
   change the temperature
   ```

1. Agregue una regla de finalización para confirmar el resultado.

   | Configuración    | Valor sugerido                                           |Descripción                                     |
   | ---------- | --------------------------------------------------------- |-----|
   | Nombre       | Mensaje de confirmación                                      |Nombre que describe el propósito de la regla. |
   | Condiciones | Parámetros obligatorios: `Temperature`                       |Condiciones que determinan cuándo se puede ejecutar la regla.    |   
   | Acciones    | Enviar respuesta de voz: `Ok, setting temperature to {Temperature} degrees` | Acción que se realizará cuando la condición de la regla sea true. |

> [!TIP]
> En este ejemplo se utiliza una respuesta de voz para confirmar el resultado. Para obtener ejemplos sobre cómo completar el comando con una acción de cliente, consulte: [Cómo: Ejecución de comandos en el cliente con el SDK de Voz](./how-to-custom-speech-commands-fulfill-sdk.md)


## <a name="try-it-out"></a>Prueba
1. Seleccione el icono `Train` presente en la parte superior del panel derecho.

1. Una vez que se complete el entrenamiento, seleccione `Test` y pruebe algunas interacciones.

    - Entrada: establezca la temperatura en 72 grados.
    - Salida: Ok, setting temperature to 72 degrees (De acuerdo, definiendo la temperatura en 72 grados)
    - Entrada: establezca la temperatura en 45 grados.
    - Salida: Sorry, I can only set between 60 and 80 degrees (El valor solo puede establecerse entre 60 y 80 grados)
    - Entrada: cámbiala a 72 grados en su lugar.
    - Salida: Ok, setting temperature to 72 degrees (De acuerdo, definiendo la temperatura en 72 grados)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Cómo: Adición de una confirmación a un comando personalizado (versión preliminar)](./how-to-custom-speech-commands-confirmations.md)
