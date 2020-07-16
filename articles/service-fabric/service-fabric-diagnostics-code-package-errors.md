---
title: Diagnóstico de errores comunes de los paquetes de código con Service Fabric
description: Aprenda a solucionar errores comunes del paquete de código con Azure Service Fabric
author: grzuber
ms.topic: article
ms.date: 05/09/2019
ms.author: grzuber
ms.openlocfilehash: 983d45a7a240701fa6441d2d9edeeda16f1ed18b
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86256499"
---
# <a name="diagnose-common-code-package-errors-by-using-service-fabric"></a>Diagnóstico de errores comunes de los paquetes de código con Service Fabric

En este artículo se describe lo que conlleva que un paquete de código finalice de forma inesperada. Además, se proporciona información sobre las posibles causas de los códigos de error comunes, junto con pasos para solucionar los problemas.

## <a name="when-does-a-process-or-container-terminate-unexpectedly"></a>¿Cuándo finaliza inesperadamente un proceso o contenedor?

Cuando Azure Service Fabric recibe una solicitud para iniciar un paquete de código, empieza a preparar el entorno en el sistema local en función de las opciones establecidas en los manifiestos de aplicación y de servicio. Estas tareas de preparación pueden incluir la reserva de recursos o puntos de conexión de red, la configuración de reglas de firewall o la configuración de restricciones de regulación de recursos. 

Una vez que el entorno se haya configurado correctamente, Service Fabric intentará abrir el paquete de código. Este paso se considera exitoso si el sistema operativo o el tiempo de ejecución del contenedor notifica que el proceso o contenedor se activó correctamente. Si la activación no se realiza correctamente, debería ver un mensaje de mantenimiento en SFX parecido al siguiente:

```
There was an error during CodePackage activation. Service host failed to activate. Error: 0xXXXXXXXX
```

Una vez que se haya activado correctamente el paquete de código, Service Fabric empezará a supervisar su vigencia. Llegado a este punto, un proceso o contenedor puede finalizar en cualquier momento por varias razones. Por ejemplo, tal vez no pudo inicializar un archivo DLL o el sistema operativo se quedó sin espacio en el montón del escritorio. Si el paquete de código finalizó, debería ver el siguiente mensaje de mantenimiento en SFX:

```
The process/container terminated with exit code: XXXXXXXX. Please look at your application logs/dump or debug your code package for more details. For information about common termination errors, please visit https://aka.ms/service-fabric-termination-errors
```

El código de salida de este mensaje de mantenimiento es la única pista que proporciona el proceso o el contenedor sobre el motivo por el que finalizó. Puede haberlo generado cualquier nivel de la pila. Por ejemplo, este código de salida podría estar relacionado con un error del sistema operativo o con un problema de .NET, o bien podría haberlo generado el código. Use este artículo como punto de partida para diagnosticar el origen de los códigos de salida de finalización y las posibles soluciones. Aun así, tenga en cuenta que son soluciones generales para escenarios comunes y quizás no se aplican a los errores que está experimentando.

## <a name="how-can-i-tell-if-service-fabric-terminated-my-code-package"></a>¿Cómo puedo saber si Service Fabric finalizó mi paquete de código?

Service Fabric puede ser responsable de finalizar su paquete de código por una variedad de motivos. Por ejemplo, puede decidir colocar el paquete de código en otro nodo para fines de equilibrio de carga. Puede comprobar si Service Fabric finalizó su paquete de código si ve cualquiera de los códigos de salida de la tabla siguiente.

>[!NOTE]
> Si el proceso o contenedor finaliza con un código de salida distinto de los que se muestran en la tabla siguiente, Service Fabric no es responsable de su finalización.

Código de salida | Descripción
--------- | -----------
7147 | Indica que Service Fabric cerró correctamente el proceso o contenedor al enviar una señal CTRL+C.
7148 | Indica que Service Fabric finalizó el proceso o contenedor. En ocasiones, este código de error puede indicar que el proceso o contenedor no respondió de forma oportuna después de enviar una señal CTRL+C, por lo que hubo que finalizarlo.


## <a name="other-common-error-codes-and-their-potential-fixes"></a>Otros códigos de error comunes y sus posibles correcciones

Código de salida | Valor hexadecimal | Descripción breve | Causa principal | Corrección posible
--------- | --------- | ----------------- | ---------- | -------------
3221225794 | 0xc0000142 | STATUS_DLL_INIT_FAILED | Este error en ocasiones significa que el equipo se quedó sin espacio en el montón del escritorio. Esta causa es especialmente probable si tiene numerosos procesos que pertenecen a la aplicación en ejecución en el nodo. | Si el programa no se creó para responder a las señales CTRL+C, puede habilitar el valor **EnableActivateNoWindow** en el manifiesto del clúster. Al habilitar esta configuración, el paquete de código se ejecutará sin una ventana de GUI y no recibirá las señales CTRL+C. Esta acción también reduce la cantidad de espacio en el montón del escritorio que consume cada proceso. Si el paquete de código tiene que recibir señales CTRL+C, puede aumentar el tamaño del montón del escritorio del nodo.
3762504530 | 0xe0434352 | N/D | Este valor representa el código de error para una excepción no controlada desde código administrado (es decir, .NET). | Este código de salida indica que la aplicación lanzó una excepción que todavía no se ha controlado y que finalizó el proceso. Como primer paso para determinar qué ha desencadenado este error, depure los registros y los archivos de volcado de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre [cómo diagnosticar otros escenarios comunes](service-fabric-diagnostics-common-scenarios.md).
* Consulte [Introducción a Azure Monitor](../azure-monitor/overview.md) para obtener información más detallada sobre los registros de Azure Monitor y lo que ofrecen.
* Obtenga más información sobre las [alertas](../azure-monitor/platform/alerts-overview.md) de los registros de Azure Monitor como ayuda para la detección y el diagnóstico.
* Familiarícese con las características de [consultas y búsqueda de registros](../azure-monitor/log-query/log-query-overview.md) que se ofrecen como parte de los registros de Azure Monitor.
