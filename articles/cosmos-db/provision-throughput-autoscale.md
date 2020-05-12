---
title: Creación de contenedores y bases de datos de Azure Cosmos en el rendimiento aprovisionado de escalabilidad automática
description: Obtenga información sobre las ventajas, los casos de uso y la manera de aprovisionar los contenedores y bases de datos de Azure Cosmos en el rendimiento aprovisionado de escalabilidad automática.
author: kirillg
ms.author: kirillg
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/28/2020
ms.openlocfilehash: 81a13dcb7955a7d46f485416bf9b7e4e7be4d9ac
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/05/2020
ms.locfileid: "82791721"
---
# <a name="create-azure-cosmos-containers-and-databases-with-autoscale-provisioned-throughput"></a>Creación de contenedores y bases de datos de Azure Cosmos con el rendimiento aprovisionado de escalabilidad automática

Azure Cosmos DB le permite configurar los contenedores con rendimiento aprovisionado estándar (manual) o rendimiento aprovisionado de escalabilidad automática. En este artículo se describen las ventajas y los casos de uso de la escalabilidad automática.

> [!NOTE]
> Puede [habilitar la Escalabilidad automática solo para bases de datos y contenedores nuevos](#create-db-container-autoscale). No está disponible para las bases de datos y los contenedores existentes.

Además del aprovisionamiento estándar de rendimiento, ahora puede configurar contenedores de Azure Cosmos con el rendimiento aprovisionado de escalabilidad automática. Los contenedores y las bases de datos configurados en el rendimiento aprovisionado de escalabilidad automática **escalarán de forma automática e instantánea el rendimiento aprovisionado en función de las necesidades de la aplicación sin que ello afecte a la disponibilidad, la latencia, la capacidad de proceso o el rendimiento de la carga de manera global**.

Al configurar contenedores y bases de datos en la escalabilidad automática, debe especificar que el rendimiento máximo de `Tmax` no se supere. Después, los contenedores pueden escalar su capacidad de proceso de manera que `0.1*Tmax < T < Tmax`. En otras palabras, los contenedores y las bases de datos se escalan de forma instantánea en función de las necesidades de la carga de trabajo, desde un mínimo del 10 % del valor máximo de capacidad de proceso configurado hasta dicho valor máximo. Puede cambiar el valor del rendimiento máximo (`Tmax`) en una base de datos o un contenedor de Escalabilidad automática en cualquier momento. Con la opción Escalabilidad automática, ya no es aplicable el rendimiento mínimo de 400 RU/s por contenedor o base de datos.

El sistema permite el funcionamiento dentro del límite de almacenamiento calculado para el rendimiento máximo especificado en el contenedor o la base de datos. Si se supera el límite de almacenamiento, la capacidad máxima de proceso se ajusta automáticamente a un valor superior. Cuando se usa el rendimiento de nivel de base de datos con la escalabilidad automática, el número de contenedores permitidos en una base de datos se calcula de la siguiente manera: `0.001*TMax`. Por ejemplo, si aprovisiona 20 000 RU/s en modo Escalabilidad automática, la base de datos puede tener 20 contenedores.

## <a name="benefits-of-autoscale-provisioned-throughput"></a><a id="autoscale-benefits"></a> Ventajas del rendimiento aprovisionado de escalabilidad automática

Los contenedores de Azure Cosmos configurados con escalabilidad automática tienen las ventajas siguientes:

* **Simple:** los contenedores con escalabilidad automática quitan la complejidad de administrar el rendimiento aprovisionado (RU) y la capacidad manualmente para varios contenedores.

* **Escalable:** los contenedores con escalabilidad automática escalan sin problemas el rendimiento aprovisionado según sea necesario. No hay ninguna interrupción en las conexiones de cliente ni en las aplicaciones y no afectan a los contratos de nivel de usuario existentes.

* **Rentable:** al usar contenedores configurados con escalabilidad automática, solo paga por los recursos que las cargas de trabajo necesitan por hora.

* **Alta disponibilidad:** los contenedores con escalabilidad automática usan el mismo back-end distribuido globalmente, tolerante a errores y de alta disponibilidad para garantizar la durabilidad de los datos y la alta disponibilidad.

## <a name="use-cases-of-autoscale-provisioned-throughput"></a><a id="autoscale-usecases"></a> Casos de uso del rendimiento aprovisionado de escalabilidad automática

Los casos de uso de los contenedores de Azure Cosmos configurados con escalabilidad automática incluyen:

* **Cargas de trabajo de variables:** cuando se ejecuta una aplicación que se usa poco con un uso máximo de una a pocas horas al día, o varias veces al año. Entre los ejemplos se incluyen las aplicaciones para recursos humanos, presupuestos e informes operativos. En estos escenarios, se pueden usar los contenedores configurados con escalabilidad automática y ya no es necesario el aprovisionamiento manual para una capacidad máxima o media.

* **Cargas de trabajo imprevisibles:** cuando se ejecutan cargas de trabajo en las que se usa la base de datos a lo largo del día, pero también picos de actividad que son difíciles de predecir. Un ejemplo incluye un sitio de tráfico que experimenta un aumento de actividad cuando el pronóstico meteorológico cambia. Los contenedores configurados con escalabilidad automática ajustan la capacidad para satisfacer las necesidades de la carga máxima de la aplicación y la reducen verticalmente cuando se supera el pico de actividad.

* **Aplicaciones nuevas:** si está implementando una nueva aplicación y no está seguro del rendimiento aprovisionado (es decir, el número de RU) que necesita. Con los contenedores configurados con escalabilidad automática, puede escalar automáticamente según las necesidades de capacidad y los requisitos de la aplicación.

* **Aplicaciones usadas con poca frecuencia:** si tiene una aplicación que solo se usa durante unas horas varias veces al día, la semana o el mes, como un sitio de aplicación, web o blog de bajo volumen.

* **Bases de datos de desarrollo y pruebas:** si tiene desarrolladores que usan contenedores durante el horario laboral, pero no los necesitan por la noche ni los fines de semana. Con los contenedores configurados con escalabilidad automática, se reducen verticalmente al mínimo cuando no están en uso.

* **Cargas de trabajo o consultas de producción programadas:** si tiene una serie de solicitudes, operaciones o consultas programadas en un solo contenedor, y si hay períodos de inactividad en los que desea ejecutar con un rendimiento absoluto bajo, ahora puede hacerlo fácilmente. Cuando se envía una consulta o una solicitud programada a un contenedor configurado con escalabilidad automática, este se escalará verticalmente de forma automática en la medida necesaria y ejecutará la operación.

Las soluciones a los problemas anteriores no solo requieren una cantidad enorme de tiempo de implementación, sino que también presentan complejidad en la configuración o el código, y suelen requerir la intervención manual para abordarlos. La escalabilidad automática habilita los escenarios anteriores desde el principio, de modo que ya no es necesario preocuparse por estos problemas.

## <a name="comparison--standard-manual-vs-autoscale-provisioned-throughput"></a>Comparación: rendimiento aprovisionado estándar (manual) frente a rendimiento aprovisionado de escalabilidad automática

|  | Contenedores configurados con rendimiento aprovisionado estándar  | Contenedores configurados con rendimiento aprovisionado de escalabilidad automática |
|---------|---------|---------|
| **Rendimiento aprovisionado** | Aprovisionada manualmente. | Escalado automático e instantáneo en función de los patrones de uso de la carga de trabajo. |
| **Limitación de velocidad de las solicitudes o las operaciones (429)**  | Puede ocurrir si el consumo supera la capacidad aprovisionada. | No ocurrirá si el rendimiento consumido se encuentra dentro de la capacidad máxima de proceso elegida con escalabilidad automática.   |
| **Planificación de capacidad** |  Debe realizar un planeamiento inicial de la capacidad y aprovisionar el rendimiento necesario. |    No tiene que preocuparse por el planeamiento de la capacidad. El sistema se encarga automáticamente del planeamiento y la administración de la capacidad. |
| **Precios** | RU/s por hora aprovisionadas manualmente. | En el caso de las cuentas de una sola región de escritura, se paga por el rendimiento que se usa cada hora, con la tarifa de RU/s por hora de Escalabilidad automática. <br/><br/>En el caso de las cuentas con varias regiones de escritura, no se aplica ningún cargo adicional por Escalabilidad automática. Paga por el rendimiento que se usa por hora con la misma tarifa de RU/s por hora de la arquitectura multimaestro. |
| **Adecuado para los tipos de carga de trabajo** |  Cargas de trabajo predecibles y estables|   Cargas de trabajo impredecibles y variables  |

## <a name="create-a-database-or-a-container-with-autoscale"></a><a id="create-db-container-autoscale"></a> Creación de una base de datos o un contenedor con escalabilidad automática

Puede configurar Escalabilidad automática para bases de datos o contenedores nuevos mientras los crea a través de Azure Portal. Siga los pasos que se indican a continuación para crear una base de datos o un contenedor nuevos, habilite Escalabilidad automática y especifique el rendimiento máximo (RU/s).

1. Inicie sesión en [Azure Portal](https://portal.azure.com) o en el [explorador de Azure Cosmos DB](https://cosmos.azure.com/).

1. Vaya a la cuenta de Azure Cosmos DB y abra la pestaña **Explorador de datos**.

1. Seleccione **Nuevo contenedor**. Escriba un nombre para la base de datos, el contenedor y una clave de partición. En **Rendimiento**, seleccione la opción **Escalabilidad automática** y elija el rendimiento máximo (RU/s) que la base de datos o el contenedor no podrá superar cuando se use la opción Escalabilidad automática.

   ![Creación de un contenedor y configuración del rendimiento de Escalabilidad automática](./media/provision-throughput-autoscale/create-container-autoscale-mode.png)

1. Seleccione **Aceptar**.

Para crear una base de datos de rendimiento compartida con escalabilidad automática, seleccione la opción de **Rendimiento de la base de datos de aprovisionamiento**.

## <a name="throughput-and-storage-limits-for-autoscale"></a><a id="autoscale-limits"></a> Límites de rendimiento y almacenamiento para Escalabilidad automática

En la tabla siguiente se muestran los límites de rendimiento y almacenamiento máximos para distintas opciones en escalabilidad automática:

|Límite de rendimiento máximo  |Límite de almacenamiento máximo  |
|---------|---------|
|4000 RU/s  |   50 GB    |
|20 000 RU/s  |  200 GB  |
|100 000 RU/s    |  1 TB   |
|500 000 RU/s    |  5 TB  |

## <a name="next-steps"></a>Pasos siguientes

* Revise las [preguntas más frecuentes sobre Escalabilidad automática](autoscale-faq.md).
* Más información sobre las [particiones lógicas](partition-data.md).
* Obtenga más información sobre el [aprovisionamiento del rendimiento de un contenedor de Azure Cosmos](how-to-provision-container-throughput.md).
* Obtenga más información sobre el [aprovisionamiento del rendimiento de una base de datos de Azure Cosmos](how-to-provision-database-throughput.md).
