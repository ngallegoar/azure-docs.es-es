---
title: Creación de contenedores y bases de datos de Azure Cosmos en modo Autopilot
description: Obtenga información sobre las ventajas, los casos de uso y la manera de aprovisionar los contenedores y bases de datos de Azure Cosmos en el modo Autopilot.
author: kirillg
ms.author: kirillg
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 89af30788fe5129cddc6a3607b8c722549b610d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79225680"
---
# <a name="create-azure-cosmos-containers-and-databases-in-autopilot-mode-preview"></a>Creación de contenedores y bases de datos de Azure Cosmos en modo Autopilot (versión preliminar)

Azure Cosmos DB le permite aprovisionar el rendimiento de los contenedores en modo manual o Autopilot. En este artículo se describen las ventajas y los casos de uso del modo Autopilot.

> [!NOTE]
> El modo Autopilot está disponible actualmente en versión preliminar pública. Puede [habilitar Autopilot solo para bases de datos y contenedores nuevos](#create-a-database-or-a-container-with-autopilot-mode). No está disponible para las bases de datos y los contenedores existentes.

Además del aprovisionamiento manual de la capacidad de proceso, ahora puede configurar contenedores de Azure Cosmos en el modo Autopilot. Los contenedores y las bases de datos de configurados en el modo Autopilot **escalarán de forma automática e instantánea la capacidad de proceso aprovisionada en función de las necesidades de la aplicación sin que ello afecte a la disponibilidad, la latencia, la capacidad de proceso o el rendimiento de la carga de manera global**.

Al configurar contenedores y bases de datos en el modo Autopilot, debe especificar que la capacidad de proceso máxima de `Tmax` no se supere. Después, los contenedores pueden escalar su capacidad de proceso de manera que `0.1*Tmax < T < Tmax`. En otras palabras, los contenedores y las bases de datos se escalan de forma instantánea en función de las necesidades de la carga de trabajo, desde un mínimo del 10 % del valor máximo de capacidad de proceso configurado hasta dicho valor máximo. Puede cambiar el valor de la capacidad de proceso máxima (`Tmax`) en una base de datos o un contenedor de Autopilot en cualquier momento. Con la opción AutoPilot, ya no es aplicable la capacidad de proceso mínima de 400 RU/s por contenedor o base de datos.

Durante la versión preliminar de Autopilot, el sistema permite el funcionamiento dentro del límite de almacenamiento calculado para la capacidad máxima de proceso especificada en el contenedor o la base de datos. Si se supera el límite de almacenamiento, la capacidad máxima de proceso se ajusta automáticamente a un valor superior. Cuando se usa la capacidad de proceso de nivel de base de datos con el modo Autopilot, el número de contenedores permitidos en una base de datos se calcula de la siguiente manera: `0.001*TMax`. Por ejemplo, si aprovisiona 20 000 RU/s en modo Autopilot, la base de datos puede tener 20 contenedores.

## <a name="benefits-of-autopilot-mode"></a>Ventajas del modo Autopilot

Los contenedores de Azure Cosmos configurados en el modo Autopilot tienen las siguientes ventajas:

* **Simple:** los contenedores en el modo Autopilot quitan la complejidad de administrar el rendimiento aprovisionado (RU) y la capacidad manualmente para varios contenedores.

* **Escalable:** los contenedores del modo Autopilot escalan sin problemas la capacidad de rendimiento aprovisionada según sea necesario. No hay ninguna interrupción en las conexiones de cliente ni en las aplicaciones y no afectan a los contratos de nivel de usuario existentes.

* **Rentable:** al usar contenedores configurados en el modo Autopilot, solo paga por los recursos que las cargas de trabajo necesitan por hora.

* **Alta disponibilidad:** los contenedores en modo Autopilot usan el mismo back-end distribuido globalmente, tolerante a errores y de alta disponibilidad para garantizar la durabilidad de los datos y la alta disponibilidad.

## <a name="use-cases-of-autopilot-mode"></a>Casos de uso del modo Autopilot

Los casos de uso de los contenedores de Azure Cosmos configurados en el modo Autopilot incluyen:

* **Cargas de trabajo de variables:** cuando se ejecuta una aplicación que se usa poco con un uso máximo de una a pocas horas al día, o varias veces al año. Entre los ejemplos se incluyen las aplicaciones para recursos humanos, presupuestos e informes operativos. En estos escenarios, se pueden usar los contenedores configurados en el modo Autopilot y ya no es necesario el aprovisionamiento manual para una capacidad máxima o media.

* **Cargas de trabajo imprevisibles:** cuando se ejecutan cargas de trabajo en las que se usa la base de datos a lo largo del día, pero también picos de actividad que son difíciles de predecir. Un ejemplo incluye un sitio de tráfico que experimenta un aumento de actividad cuando el pronóstico meteorológico cambia. Los contenedores configurados en el modo Autopilot ajustan la capacidad para satisfacer las necesidades de la carga máxima de la aplicación y la reducen verticalmente cuando se supera el pico de actividad.

* **Aplicaciones nuevas:** si está implementando una nueva aplicación y no está seguro del rendimiento aprovisionado (es decir, el número de RU) que necesita. Con los contenedores configurados en el modo Autopilot, puede escalar automáticamente a las necesidades de capacidad y los requisitos de la aplicación.

* **Aplicaciones usadas con poca frecuencia:** si tiene una aplicación que solo se usa durante unas horas varias veces al día, la semana o el mes, como un sitio de aplicación, web o blog de bajo volumen.

* **Bases de datos de desarrollo y pruebas:** si tiene desarrolladores que usan contenedores durante el horario laboral, pero no los necesitan por la noche ni los fines de semana. Con los contenedores configurados en el modo Autopilot, se reducen verticalmente al mínimo cuando no están en uso.

* **Cargas de trabajo o consultas de producción programadas:** si tiene una serie de solicitudes, operaciones o consultas programadas en un solo contenedor, y si hay períodos de inactividad en los que desea ejecutar con un rendimiento absoluto bajo, ahora puede hacerlo fácilmente. Cuando se envía una consulta o una solicitud programada a un contenedor configurado en el modo Autopilot, se escalará verticalmente de forma automática en la medida necesaria y ejecutará la operación.

Las soluciones a los problemas anteriores no solo requieren una cantidad enorme de tiempo de implementación, sino que también presentan complejidad en la configuración o el código, y suelen requerir la intervención manual para abordarlos. El modo Autopilot habilita los escenarios anteriores listos para su uso, de modo que ya no es necesario preocuparse por estos problemas.

## <a name="comparison--containers-configured-in-manual-mode-vs-autopilot-mode"></a>Comparación: contenedores configurados en modo manual frente al modo Autopilot

|  | Contenedores configurados en el modo manual  | Contenedores configurados en el modo Autopilot |
|---------|---------|---------|
| **Rendimiento aprovisionado** | Aprovisionada manualmente. | Escalado automático e instantáneo en función de los patrones de uso de la carga de trabajo. |
| **Limitación de velocidad de las solicitudes o las operaciones (429)**  | Puede ocurrir si el consumo supera la capacidad aprovisionada. | No ocurrirá si la capacidad de proceso consumida se encuentra dentro de la capacidad máxima de proceso elegida para el modo Autopilot.   |
| **Planificación de capacidad** |  Debe realizar un planeamiento inicial de la capacidad y aprovisionar el rendimiento necesario. |    No tiene que preocuparse por el planeamiento de la capacidad. El sistema se encarga automáticamente del planeamiento y la administración de la capacidad. |
| **Precios** | RU/s por hora aprovisionadas manualmente. | En el caso de las cuentas de una sola región de escritura, se paga por el rendimiento que se usa cada hora, con la tarifa de RU/s por hora de Autopilot. <br/><br/>En el caso de las cuentas con varias regiones de escritura, no se aplica ningún cargo adicional por Autopilot. Paga por el rendimiento que se usa por hora con la misma tarifa de RU/s por hora de la arquitectura multimaestro. |
| **Adecuado para los tipos de carga de trabajo** |  Cargas de trabajo predecibles y estables|   Cargas de trabajo impredecibles y variables  |

## <a name="create-a-database-or-a-container-with-autopilot-mode"></a>Creación de una base de datos o un contenedor con el modo Autopilot

Puede configurar Autopilot para bases de datos o contenedores nuevos mientras los crea a través de Azure Portal. Siga los pasos que se indican a continuación para crear una base de datos o un contenedor nuevos, habilite Autopilot y especifique el rendimiento máximo (RU/s).

1. Inicie sesión en [Azure Portal](https://portal.azure.com) o en el [explorador de Azure Cosmos DB](https://cosmos.azure.com/).

1. Vaya a la cuenta de Azure Cosmos DB y abra la pestaña **Explorador de datos**.

1. Seleccione **Nuevo contenedor**. Escriba un nombre para la base de datos, el contenedor y una clave de partición. En **Rendimiento**, seleccione la opción **Autopilot** y elija la capacidad de proceso máxima (RU/s) que la base de datos o el contenedor no podrá superar cuando se use la opción Autopilot.

   ![Creación de un contenedor y configuración de la capacidad de proceso de AutoPilot](./media/provision-throughput-autopilot/create-container-autopilot-mode.png)

1. Seleccione **Aceptar**.

Para crear una base de datos de rendimiento compartida con el modo Autopilot, seleccione la opción de **Rendimiento de la base de datos de aprovisionamiento**.

## <a name="throughput-and-storage-limits-for-autopilot"></a><a id="autopilot-limits"></a> Límites de rendimiento y almacenamiento para AutoPilot

En la tabla siguiente se muestran los límites de rendimiento y almacenamiento máximos para distintas opciones en el modo AutoPilot:

|Límite de rendimiento máximo  |Límite de almacenamiento máximo  |
|---------|---------|
|4000 RU/s  |   50 GB    |
|20 000 RU/s  |  200 GB  |
|100 000 RU/s    |  1 TB   |
|500 000 RU/s    |  5 TB  |

## <a name="next-steps"></a>Pasos siguientes

* Revise las [preguntas más frecuentes sobre Autopilot](autopilot-faq.md).
* Más información sobre las [particiones lógicas](partition-data.md).
* Obtenga más información sobre el [aprovisionamiento del rendimiento de un contenedor de Azure Cosmos](how-to-provision-container-throughput.md).
* Obtenga más información sobre el [aprovisionamiento del rendimiento de una base de datos de Azure Cosmos](how-to-provision-database-throughput.md).
