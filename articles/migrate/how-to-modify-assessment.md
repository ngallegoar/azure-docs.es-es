---
title: Personalización de evaluaciones para la evaluación de servidores de Azure Migrate | Microsoft Docs
description: En este artículo se describe cómo personalizar las evaluaciones creadas con la evaluación de servidores de Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/15/2019
ms.author: raynew
ms.openlocfilehash: 2cfac978b0a5af20e9e2fa1e32a7361488f20fbe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "68234268"
---
# <a name="customize-an-assessment"></a>Personalización de una evaluación

En este artículo se describe cómo personalizar las evaluaciones creadas con la evaluación de servidores de Azure Migrate.

[Azure Migrate](migrate-services-overview.md) proporciona un centro de conectividad para realizar el seguimiento de la detección, evaluación y migración a Azure de las aplicaciones y cargas de trabajo locales, así como de máquinas virtuales en la nube privadas o públicas. El centro proporciona herramientas de Azure Migrate para la evaluación y la migración, así como ofertas de proveedores de software independientes (ISV).

Puede usar la herramienta de evaluación de servidores de Azure Migrate para crear evaluaciones para máquinas virtuales de VMware locales y máquinas virtuales de Hyper-V, como preparación para la migración a Azure.

## <a name="about-assessments"></a>Acerca de las evaluaciones

Se pueden ejecutar dos tipos de evaluaciones mediante Azure Migrate Server Assessment.

**Valoración** | **Detalles** | **Data**
--- | --- | ---
**Basada en el rendimiento** | Evaluaciones basadas en los datos de rendimiento recopilados | **Tamaño de máquina virtual recomendado**: se basa en los datos de uso de la CPU y de la memoria.<br/><br/> **Tipo de disco recomendado (disco administrado estándar o Premium**): se basa en IOPS y en el rendimiento de los discos locales.
**Como local** | Evaluaciones que se basan en el tamaño local. | **Tamaño de máquina virtual recomendado**: se basa en el tamaño de la máquina virtual local<br/><br> **Tipo de disco recomendado**: se basa en el valor del tipo de almacenamiento que se selecciona para la evaluación.


## <a name="how-is-an-assessment-done"></a>¿Cómo se realiza una evaluación?

Una evaluación realizada en la herramienta de evaluación de servidores de Azure Migrate tiene tres fases. La valoración comienza con un análisis de idoneidad, seguido de las estimaciones de ajuste de tamaño y, por último, la estimación del costo mensual. Una máquina solo se mueve a una fase posterior si aprueba la anterior. Por ejemplo, si no supera la comprobación de idoneidad de Azure, se marca como no adecuada para Azure, y el ajuste de tamaño y los costos no se calculan. [Más información.](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation)

## <a name="whats-in-an-assessment"></a>¿Qué es una evaluación?

**Propiedad** | **Detalles**
--- | ---
**Ubicación de destino** | La ubicación de Azure a la que desea realizar la migración.<br/> La herramienta de evaluación de servidores admite actualmente estas regiones de destino: Este de Australia, Sudeste de Australia, Sur de Brasil, Centro de Canadá, Este de Canadá, Centro de la India, Centro de EE. UU., Este de China, Norte de China, Este de Asia, Este de EE. UU., Este de EE. UU. 2, Сentro de Alemania, Nordeste de Alemania, Este de Japón, Oeste de Japón, Centro de Corea del Sur, Corea del Sur, Centro y norte de EE. UU., Norte de Europa, Centro y sur de EE. UU., Sudeste de Asia, Sur de la India, Sur de Reino Unido, Oeste del Reino Unido, US Gov Arizona, US Gov Texas, US Gov Virginia, Centro-oeste de EE. UU., Oeste de Europa, Oeste de la India, Oeste de EE. UU. y Oeste de EE. UU. 2.
**Tipo de almacenamiento** | Puede usar esta propiedad para especificar el tipo de discos a los que desea migrar en Azure.<br/><br/> Para los tamaños locales, puede especificar el tipo de almacenamiento de destino como discos administrados prémium, discos administrados SSD estándar o discos administrados HDD estándar. Para tamaños según el rendimiento, puede especificar el tipo de disco de destino como Automático, discos administrados premium, discos administrados HDD estándar o discos administrados SSD estándar.<br/><br/> Cuando se especifica el tipo de almacenamiento como automático, la recomendación de disco se realiza basándose en los datos de rendimiento de los discos (IOPS y rendimiento). Si especifica el tipo de almacenamiento como prémium o estándar, la evaluación recomendará una SKU de disco dentro del tipo de almacenamiento seleccionado. Si desea conseguir un Acuerdo de Nivel de Servicio de máquina virtual de instancia única del 99,9 %, es posible que desee especificar el tipo de almacenamiento como discos administrados premium. Esto garantiza que todos los discos de la evaluación se recomienden como discos administrados prémium. Azure
**Instancias reservadas (RI)** | Esta propiedad le ayuda a especificar si tiene [instancias reservadas](https://azure.microsoft.com/pricing/reserved-vm-instances/) en Azure, entonces, las estimaciones de costos en la evaluación se realizan teniendo en cuenta los descuentos de instancias reservadas. Las instancias reservadas solo se admiten actualmente para la oferta de pago por uso en Azure Migrate.
**Criterio de ajuste de tamaño** | El criterio que se debe utilizar para ajustar el tamaño de las máquinas virtuales para Azure. Puede hacer un ajuste de tamaño *según el rendimiento* o puede ajustar el tamaño de las máquinas virtuales *como en el entorno local*, sin tener en cuenta el historial de rendimiento.
**Historial de rendimiento** | La duración que se debe tener en cuenta para evaluar los datos de rendimiento de las máquinas. Esta propiedad solo es aplicable cuando el criterio de ajuste de tamaño es *según el rendimiento*.
**Uso de percentil** | El valor de percentil del ejemplo de rendimiento establecido para determinar el tamaño adecuado. Esta propiedad solo es aplicable cuando el ajuste de tamaño es *según el rendimiento*.
**Series de VM** |     Puede especificar la series de VM que quiera tener en cuenta para determinar el tamaño adecuado. Por ejemplo, si tiene un entorno de producción que no vaya a migrar a la serie A de las máquinas virtuales de Azure, puede excluir la serie A de la lista o serie, y el ajuste de tamaño correcto se realizará solo en la serie seleccionada.
**Factor de confort** | Azure Migrate Server Assessment tiene en cuenta un búfer (factor de confort) durante la evaluación. Dicho búfer se aplica además de los datos de uso de la máquina en las máquinas virtuales (CPU, memoria, disco y red). El factor de confort se tiene en cuenta en problemas como el uso estacional, un historial de rendimiento corto y los posibles aumentos en el uso futuro.<br/><br/> Por ejemplo, una máquina virtual de 10 núcleos con un uso del 20 % normalmente genera una máquina virtual de 2 núcleos. Sin embargo, con un factor de confort de 2.0 x, el resultado es una máquina virtual de 4 núcleos.
**Oferta** | La [oferta de Azure](https://azure.microsoft.com/support/legal/offer-details/) en la que está inscrito. Azure Migrate calcula el costo en consecuencia.
**Moneda** | Divisa de facturación.
**Descuento (%)** | Cualquier descuento específico de la suscripción que reciba además de la oferta de Azure.<br/> La configuración predeterminada es 0 %.
**Tiempo de actividad de VM** | Si las máquinas virtuales no se van a ejecutar todas las horas en Azure, puede especificar la duración (número de días al mes y número de horas al día) del período en el que se estarán ejecutando y las estimaciones de costo se realizarán en consecuencia.<br/> El valor predeterminado es 31 días al mes y 24 horas al día.
**Ventaja híbrida de Azure** | Especifique si dispone de Software Assurance y tiene derecho a [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Si se establece en Sí, los precios de Azure que no son de Windows se toman en cuenta para las máquinas virtuales de Windows. El valor predeterminado es Yes.


## <a name="edit-assessment-properties"></a>Editar propiedades de evaluación

Para editar las propiedades de evaluación después de crear una evaluación, haga lo siguiente:

1. En el proyecto de Azure Migrate, haga clic en **Servidores**.
2. En **Azure Migrate: evaluación de servidores**, haga clic en la cuenta de evaluaciones.
3. En **Evaluación**, haga clic en la evaluación pertinente > **Editar propiedades**.
5. Personalice las propiedades de la evaluación de acuerdo con la tabla anterior.
6. Haga clic en **Guardar** para actualizar la evaluación.


También puede editar las propiedades de evaluación al crear una evaluación.


## <a name="next-steps"></a>Pasos siguientes

[Obtenga más información](concepts-assessment-calculation.md) sobre cómo se calculan las evaluaciones.
