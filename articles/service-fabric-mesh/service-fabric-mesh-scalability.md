---
title: Escalabilidad de aplicaciones en Azure Service Fabric Mesh
description: Una de las ventajas de implementar aplicaciones en Service Fabric Mesh es la capacidad de escalar fácilmente los servicios, ya sea manualmente o con directivas de escalado automático.
author: georgewallace
ms.author: gwallace
ms.date: 10/26/2018
ms.topic: conceptual
ms.openlocfilehash: 67b4b27a5d1faab3e6c156704d67a30b220ac54c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91840190"
---
# <a name="scaling-service-fabric-mesh-applications"></a>Escalado de aplicaciones en Service Fabric Mesh

Una de las principales ventajas de implementar aplicaciones en Service Fabric Mesh es la capacidad de escalar vertical u horizontalmente los servicios de un modo sencillo. Esto se debe usar para controlar diferentes cantidades de carga en los servicios o bien para mejorar la disponibilidad. Puede escalar o reducir los servicios horizontalmente de forma manual o configurar directivas de escalado automático.

## <a name="manual-scaling-instances"></a>Instancias de escalado manual

En la plantilla de implementación para el recurso de aplicación, cada servicio tiene una propiedad *replicaCount* que puede usarse para establecer el número de veces que desea que ese servicio se implemente. Una aplicación puede constar de varios servicios, cada uno con un único número de *replicaCount*, que se implementan y administran de forma conjunta. Para escalar el número de réplicas de servicio, modifique el valor de *replicaCount* para cada servicio que desee escalar en el archivo de parámetros o la plantilla de implementación. Después, actualice la aplicación.

Para obtener ejemplos de cómo escalar manualmente las instancias de servicios, consulte [Escalar o reducir los servicios horizontalmente de forma manual](service-fabric-mesh-tutorial-template-scale-services.md).

## <a name="autoscaling-service-instances"></a>Escalado automático de instancias de servicio
El escalado automático es una funcionalidad adicional de Service Fabric para escalar de forma dinámica una serie de instancias de servicio (escalado horizontal). El escalado automático proporciona una gran elasticidad y permite el aprovisionamiento o la eliminación de instancias de servicio en función del uso de CPU o memoria.  El escalado automático permite ejecutar el número correcto de instancias de servicio para la carga de trabajo y para optimizar el costo.

La directiva de escalado automático se define por servicio en el archivo de recursos del servicio. Cada directiva de escalado consta de dos partes:

- Un desencadenador de escalado, que describe cuándo se escalará el servicio. Hay tres factores que determinan cuándo se escalará el servicio. El *umbral inferior de carga* es un valor que determina si el servicio se reducirá horizontalmente. Si la carga media de todas las instancias de las particiones es inferior a este valor, el servicio se reducirá horizontalmente. El *umbral superior de carga* es un valor que determina si el servicio se escalará horizontalmente. Si la carga media de todas las instancias de la partición es inferior a este valor, el servicio se escalará horizontalmente. El *intervalo de escalado* determina la frecuencia (en segundos) con la que se comprobará el desencadenador. Una vez comprobado el desencadenador, se aplicará el mecanismo si es necesario escalar. Si no es necesario escalar, no se realizará ninguna acción. En ambos casos, no se volverá a comprobar el desencadenador antes de que expire el intervalo de escalado.

- Un mecanismo de escalado, que describe cómo se escalará cuando se desencadene. El *incremento de escala* determina cuántas instancias se agregarán o quitarán cuando se desencadene el mecanismo. El *recuento máximo de instancias* define el límite superior para el escalado. Si el número de instancias alcanza este límite, el servicio no se escalará horizontalmente, independientemente de la carga. El *recuento mínimo de instancias* define el límite inferior para el escalado. Si el número de instancias de la partición alcanza este límite, el servicio no se reducirá horizontalmente, independientemente de la carga.

Para obtener información sobre cómo establecer una directiva de escalado automático para el servicio, vea [Escalado automático de servicios](service-fabric-mesh-howto-auto-scale-services.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre el modelo de aplicación, consulte [Recursos de Service Fabric](service-fabric-mesh-service-fabric-resources.md).
