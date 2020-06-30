---
title: Depuración de errores durante la creación de una aplicación de Comandos personalizados (versión preliminar)
titleSuffix: Azure Cognitive Services
description: En este artículo, aprenderá a depurar los errores al crear una aplicación de Comandos personalizados.
services: cognitive-services
author: xiaojul
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: xiaojul
ms.openlocfilehash: b0a26b4d3f0f59f8e05c5990bbc64ee55f12f124
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2020
ms.locfileid: "85307300"
---
# <a name="debug-errors-when-authoring-a-custom-commands-application"></a>Depuración de errores durante la creación de una aplicación de Comandos personalizados

En este artículo se describe cómo realizar una depuración cuando se ven errores durante la compilación de una aplicación de Comandos personalizados. 

## <a name="errors-when-creating-an-application"></a>Errores durante la creación de una aplicación
Comandos personalizados también crea una aplicación en [LUIS](https://www.luis.ai/) cuando se crea una aplicación de Comandos personalizados. 

[LUIS tiene un límite de 500 aplicaciones por recurso de creación](https://docs.microsoft.com/azure/cognitive-services/luis/luis-limits). Podría producirse un error en la creación de la aplicación de LUIS si usa un recurso de creación que ya tenga 500 aplicaciones. 

Asegúrese de que el recurso de creación de LUIS seleccionado tenga menos de 500. De lo contrario, puede crear un nuevo recurso de creación de LUIS, cambiar a otro o intentar quitar las aplicaciones de LUIS.  

## <a name="errors-when-deleting-an-application"></a>Errores durante la eliminación de una aplicación
### <a name="cant-delete-luis-application"></a>No se puede eliminar la aplicación de LUIS
Al eliminar una aplicación de Comandos personalizados, es posible que Comandos personalizados también intente eliminar la aplicación de LUIS asociada a la aplicación de Comandos personalizados.

Si se produjo un error al eliminar la aplicación LUIS, vaya a la cuenta de [LUIS](https://www.luis.ai/) para eliminarla manualmente.

### <a name="toomanyrequests"></a>TooManyRequests
Al tratar de eliminar una gran cantidad de aplicaciones a la vez, es probable que se muestren errores "TooManyRequests". Esto implica que Azure está limitando las solicitudes de eliminación. 

Actualice la página e intente eliminar menos aplicaciones.

## <a name="errors-when-modifying-an-application"></a>Errores durante la modificación de una aplicación

### <a name="cant-delete-a-parameter-or-a-web-endpoint"></a>No se puede eliminar un parámetro o un punto de conexión web
No está permitido eliminar un parámetro cuando está en uso. Quite cualquier referencia del parámetro en las respuestas de voz, las oraciones de ejemplo, las condiciones y las acciones e inténtelo de nuevo.

### <a name="cant-delete-a-web-endpoint"></a>No se puede eliminar un punto de conexión web
No está permitido eliminar un punto de conexión web cuando está en uso. Quite todas las acciones **Call Web Endpoint** (llamar a un punto de conexión web) que usen este punto de conexión web antes de quitarlo.

## <a name="errors-when-training-an-application"></a>Errores durante el entrenamiento de una aplicación
### <a name="build-in-intents"></a>Intenciones integradas
LUIS tiene intenciones de tipo Sí/No integradas. Al usar oraciones de ejemplo solo con "sí", "no" devolvería un error en el entrenamiento. 

| Palabra clave | Variaciones | 
| ------- | --------- | 
| Sí | Claro, aceptar |
| No | Para nada, claro que no | 

### <a name="common-sample-sentences"></a>Oraciones de ejemplo comunes
Comandos personalizados no permite las oraciones de ejemplo comunes que se comparten entre distintos comandos. Se puede producir un error en el entrenamiento de una aplicación si algunas oraciones de ejemplo de un comando ya se definieron en otro. 

Asegúrese de que no haya oraciones de ejemplo comunes que se compartan entre distintos comandos. 

Para conocer el procedimiento recomendado para equilibrar las oraciones de ejemplo en distintos comandos, consulte el [Procedimiento recomendado de LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-best-practices).

### <a name="empty-sample-sentences"></a>Oraciones de ejemplo vacías
Debe tener al menos una oración de ejemplo para cada comando.

### <a name="undefined-parameter-in-sample-sentences"></a>Parámetro sin definir en las oraciones de ejemplo
En las oraciones de ejemplo se usa al menos un parámetro, pero ninguno se deja sin definir.

### <a name="training-takes-too-long"></a>La operación de entrenamiento tarda demasiado
El entrenamiento de LUIS está pensado para aprender rápidamente con pocos ejemplos. No agregue demasiadas oraciones de ejemplo. 

Si tiene muchas oraciones de ejemplo similares, defina un parámetro, abstraiga las oraciones en un patrón y agréguelo a las oraciones de ejemplo.

Por ejemplo, puede definir un parámetro {vehículo} para las oraciones de ejemplo siguientes y agregar solo "Reservar un {vehículo}" a las oraciones de ejemplo.

| Oraciones de ejemplo | Patrón | 
| ------- | ------- | 
| Reservar un automóvil | Reservar un {vehículo} | 
| Reservar un vuelo | Reservar un {vehículo} |
| Reservar un taxi | Reservar un {vehículo} |

Para conocer el procedimiento recomendado para entrenamiento de LUIS, consulte el [procedimiento recomendado de LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-best-practices).

## <a name="cant-update-luis-key"></a>No se puede actualizar la clave de LUIS
### <a name="reassign-to-e0-authoring-resource"></a>Reasignación de un recurso de creación de E0
LUIS no admite la reasignación de la aplicación de LUIS al recurso de creación de E0.

Si necesita cambiar el recurso de creación de F0 a E0, o bien cambiarlo a otro recurso de E0, vuelva a crear la aplicación.

### <a name="save-button-is-disabled"></a>El botón Guardar está deshabilitado
Si nunca asigna un recurso de predicción de LUIS a la aplicación, el botón Guardar se deshabilitará cuando intente modificar el recurso de creación sin agregar un recurso de predicción.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Consulte los ejemplos en GitHub](https://aka.ms/speech/cc-samples)
