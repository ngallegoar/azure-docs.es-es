---
title: 'Conceptos y definiciones de los Comandos personalizados: servicio de Voz'
titleSuffix: Azure Cognitive Services
description: En este artículo, obtendrá información sobre los conceptos y las definiciones de las aplicaciones de Comandos personalizados.
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: sausin
ms.openlocfilehash: 5a5e3590e0e184633760f080dfd3402ed75bd48e
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2020
ms.locfileid: "85307271"
---
# <a name="custom-commands-concepts-and-definitions"></a>Conceptos y definiciones de Comandos personalizados

Este artículo funcionará como referencia sobre los conceptos y las definiciones de las aplicaciones de Comandos personalizados.

## <a name="commands-configuration"></a>Configuración de comandos
Los comandos son los bloques de creación fundamentales de una aplicación de Comandos personalizados. Un comando es un conjunto de configuraciones necesarias para completar una tarea específica que ha definido un usuario.

### <a name="example-sentences"></a>Oraciones de ejemplo
Las expresiones de ejemplo son el conjunto de ejemplos que el usuario puede decir para desencadenar un comando determinado. Tenga en cuenta que solo tiene que proporcionar ejemplos de expresiones, no una lista exhaustiva. 

### <a name="parameters"></a>Parámetros
Los parámetros son información que los comandos requieren para completar una tarea. En los escenarios complejos, los parámetros también se pueden usar para definir las condiciones que desencadenan acciones personalizadas.

### <a name="completion-rules"></a>Reglas de finalización
Serie de reglas que se ejecutarán una vez que el comando esté listo para realizarse; es decir, cuando se cumplan todas las condiciones de las reglas.

### <a name="interaction-rules"></a>Reglas de interacción
Reglas adicionales para controlar situaciones más específicas o complejas. Puede agregar validaciones adicionales o configurar características avanzadas, tales como confirmaciones o correcciones de un paso. También puede crear sus propias reglas de interacción personalizadas.

## <a name="parameters-configuration"></a>Configuración de parámetros

Los parámetros son la información que los comandos requieren para completar una tarea. En los escenarios complejos, los parámetros también se pueden usar para definir las condiciones que desencadenan acciones personalizadas.

### <a name="name"></a>Nombre
Un parámetro se identifica mediante la propiedad de nombre. Siempre debe asignar un nombre descriptivo a los parámetros. Se puede hacer referencia a un parámetro en distintas secciones, como durante la construcción de condiciones, respuestas de voz u otras acciones.
 
### <a name="isglobal"></a>IsGlobal
Casilla que indica si el ámbito de este parámetro lo comparten todos los comandos de la aplicación. Si un parámetro es global, es posible que su valor se proporcione desde cualquier ámbito de comando y, una vez que se asigna un valor, se puede hacer referencia a él desde cualquiera de los comandos. 

### <a name="required"></a>Obligatorio
Casilla que indica si es obligatorio especificar un valor para este parámetro para completar el comando. Debe configurar las respuestas que pedirán al usuario que proporcione un valor si el parámetro se marca como obligatorio.

### <a name="type"></a>Tipo
Comandos personalizados admite los siguientes tipos de parámetros:


* DateTime
* Geography
* Number
* String

Todos estos tipos de parámetros son compatibles con la configuración de valores predeterminados, que puede configurar desde el portal.

### <a name="configuration"></a>Configuración
La configuración es una propiedad de parámetro que solo está definida para el tipo: String. A continuación se muestran los valores compatibles.
* None
* Accept full input (aceptar la entrada completa): al habilitar esta opción, el parámetro acepta cualquier expresión de entrada, lo que resulta útil cuando el usuario necesita un parámetro con toda la expresión. Por ejemplo, direcciones postales.
* Accept predefined input values from external catalog (aceptar valores de entrada predefinidos del catálogo externo): se usa para configurar un parámetro que puede tener una gran variedad de valores. Por ejemplo, catálogo de ventas. En este caso, el catálogo se hospeda en un punto de conexión web externo y se puede configurar de forma independiente.
* Accept predefined input values from internal catalog (aceptar valores de entrada predefinidos del catálogo interno): se usa para configurar un parámetro que puede tener pocos valores. En este caso, los valores se deben configurar en Speech Studio.


### <a name="validation"></a>Validación
Las validaciones son construcciones que se aplican a determinados tipos de parámetro y permiten configurar restricciones para el valor de un parámetro. Actualmente, Comandos personalizados admite validaciones para los siguientes tipos de parámetros:
* DateTime
* Number

## <a name="rules-configuration"></a>Configuración de reglas
Una regla de Comandos personalizados se define mediante un conjunto de **condiciones** que, cuando se cumplen, ejecutan un conjunto de **acciones**. Una regla también permite configurar su **estado posterior a la ejecución** y las **expectativas** para el siguiente turno.

### <a name="types"></a>Tipos
Comandos personalizados admite las siguientes categorías de reglas:
* Reglas de finalización

    Lista de reglas que se ejecutarán cuando se cumpla un comando. Se ejecutarán todas las reglas configuradas en esta sección cuyas condiciones se cumplan.
    
* Reglas de interacción

    Las reglas de interacción se pueden usar para configurar validaciones personalizadas, confirmaciones, correcciones de un paso adicionales o, incluso, para realizar cualquier otra lógica de diálogo personalizada. Estas reglas se evalúan durante el procesamiento de cada turno y se pueden usar para desencadenar reglas de finalización.

Las distintas acciones configuradas como parte de una regla se ejecutan en el orden en el que aparecen en el portal de creación.
    
### <a name="conditions"></a>Condiciones
Conjunto de condiciones que se deben cumplir para ejecutar una regla. Las condiciones de una regla pueden ser de uno de los tipos siguientes:
* Parameter value equals (el valor del parámetro es igual a): el valor del parámetro configurado es igual a un valor específico.
* No parameter value (sin valor de parámetro): los parámetros configurados no deben tener ningún valor.
* Parámetros obligatorios: el parámetro configurado tiene un valor.
* All required parameters (todos los parámetros obligatorios): todos los parámetros que se hayan marcado como obligatorios tienen un valor.
* Updated parameters (parámetros actualizados): uno o varios valores de parámetro se actualizaron como resultado de procesar la entrada actual (expresión/actividad).
* Confirmation was successful (confirmación correcta): la expresión/actividad de entrada tuvo una confirmación correcta (sí).
* Confirmation was denied (confirmación denegada): la expresión/actividad de entrada tuvo una confirmación correcta (no).
* Previous command needs to be updated (el comando anterior debe actualizarse): esta condición se usa en las instancias en las que quiere detectar una confirmación negada junto con una actualización. En segundo plano, se configura para cuando el motor de diálogo detecte una confirmación negativa en la que la intención sea la misma que la del turno anterior y el usuario haya respondido con una actualización.

### <a name="actions"></a>Acciones
* Send speech response (enviar respuesta de voz): envía una respuesta de voz al cliente.
* Actualizar valor de parámetro: actualiza el valor de un parámetro de comando al valor especificado.
* Clear parameter value (borrar el valor del parámetro): borra el valor del parámetro de comando.
* Call web endpoint (llamar a punto de conexión web): realiza una llamada a un punto de conexión web.
* Send activity to client (enviar una actividad al cliente): envía una actividad personalizada al cliente.

### <a name="expectations"></a>Expectativas
Las expectativas se usan para configurar sugerencias para el procesamiento de la siguiente entrada de usuario. Se admiten los tipos siguientes:
* Expecting confirmation from user (a la espera de confirmación del usuario): especifica que la aplicación espera una confirmación (sí/no) para la siguiente entrada del usuario.
* Expecting parameter(s) input from user (a la espera de la entrada de parámetros del usuario): especifica uno o varios parámetros de comando que la aplicación espera en la entrada del usuario.

### <a name="post-execution-state"></a>Estado posterior a la ejecución
Estado del diálogo después de procesar la entrada actual (expresión/actividad). Es uno de los siguientes tipos:
* Comando finalizado: finaliza el comando y no se procesará ninguna regla adicional del comando.
* Execute completion rules (ejecutar reglas de finalización): ejecuta todas las reglas de finalización válidas.
* Wait for user's input (esperar la entrada del usuario): espera a la siguiente entrada del usuario.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Consulte los ejemplos en GitHub](https://aka.ms/speech/cc-samples)
