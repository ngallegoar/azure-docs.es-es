---
title: Diseño de funciones de Azure para entradas idénticas
description: Compilación de funciones de Azure idempotentes
author: craigshoemaker
ms.author: cshoe
ms.date: 9/12/2019
ms.topic: article
ms.openlocfilehash: 15af60ac5a862e6fb20e65ba6fbb92482420b7c0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "74226870"
---
# <a name="designing-azure-functions-for-identical-input"></a>Diseño de funciones de Azure para entradas idénticas

La realidad de la arquitectura orientada a los eventos y basada en mensajes dicta la necesidad de aceptar solicitudes idénticas y de mantener al tiempo la integridad de los datos y la estabilidad del sistema.

Para ilustrarlo, pensemos en un botón de llamada al ascensor. Al presionarlo, se enciende y se envía un ascensor a su piso. Unos momentos más tarde, otra persona aparece en el vestíbulo, le sonríe y presiona el botón iluminado por segunda vez. Usted le devuelve la sonrisa y se ríe para sí mismo al recordar que el comando para llamar al ascensor es idempotente.

El hecho de presionar el botón de llamada al ascensor dos, tres o cuatro veces no afecta al resultado final. Al presionar el botón, independientemente del número de veces que se haga, se envía un ascensor al piso. Los sistemas idempotentes, como el ascensor, responden con el mismo resultado, independientemente del número de veces que se emitan comandos idénticos.

En lo que respecta a la compilación de aplicaciones, tenga en cuenta los siguientes escenarios:

- ¿Qué ocurre si su aplicación de control de inventario intenta eliminar el mismo producto más de una vez?
- ¿Cómo se comporta la aplicación de recursos humanos si hay más de una solicitud de creación de un registro de empleado para la misma persona?
- ¿Dónde va el dinero si la aplicación bancaria obtiene 100 solicitudes de ejecución de la misma retirada?

Hay muchos contextos en los que las solicitudes a una función pueden recibir comandos idénticos. Estas son algunas de esas situaciones:

- Directivas de reintento que envían la misma solicitud muchas veces
- Comandos almacenados en caché reproducidos en la aplicación
- Errores de aplicación que envían varias solicitudes idénticas

Para proteger la integridad de los datos y el estado del sistema, una aplicación idempotente contiene lógica que puede incluir los siguientes comportamientos:

- Comprobación de la existencia de datos antes de intentar ejecutar una eliminación
- Comprobación de si ya existen los datos antes de intentar ejecutar una acción de creación
- Conciliación de lógica que crea coherencia final en los datos
- Controles de simultaneidad
- Detección de duplicaciones
- Validación de la actualización de datos
- Lógica de protección para comprobar los datos de entrada

En última instancia, la idempotencia se logra al garantizar que una acción determinada es posible y solo se ejecuta una vez.
