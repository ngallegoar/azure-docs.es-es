---
title: Procedimientos recomendados de evaluación en Azure Migrate Server Assessment
description: Sugerencias para crear evaluaciones con Azure Migrate Server Assessment.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: raynew
ms.openlocfilehash: de6953b6648613595bc9975b17941b3a453a6d60
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74185981"
---
# <a name="best-practices-for-creating-assessments"></a>Procedimientos recomendados para crear evaluaciones

[Azure Migrate](migrate-overview.md) proporciona un centro de herramientas que le ayuda a detecta las aplicaciones, la infraestructura y las cargas de trabajo, a evaluarlas y a migrarlas a Microsoft Azure. Este centro incluye herramientas de Azure Migrate y ofertas de fabricantes de software independientes (ISV) de terceros.

En este artículo se resumen los procedimientos recomendados para crear evaluaciones mediante la herramienta Azure Migrate Server Assessment.

## <a name="about-assessments"></a>Acerca de las evaluaciones

Las evaluaciones que crea con Azure Migrate Server Assessment son una instantánea de datos de un momento dado. En Azure Migrate, existen dos tipos de evaluaciones.

**Tipo de evaluación** | **Detalles** | **Data**
--- | --- | ---
**Basada en el rendimiento** | Evaluaciones que realizan recomendaciones basadas en datos de rendimiento recopilados | La recomendación del tamaño de VM se basa en los datos de uso de la CPU y la memoria.<br/><br/> La recomendación de tipo de disco (SSD/disco duro estándar o discos administrados prémium) se basa en el IOPS y el rendimiento de los discos locales.
**Tal cual en el entorno local** | Evaluaciones que no usan datos de rendimiento para hacer recomendaciones. | La recomendación de tamaño de la máquina virtual se basa en el tamaño de la máquina virtual local.<br/><br> El tipo de disco recomendado se basa en lo que selecciona en la configuración de tipo de almacenamiento para la evaluación.

### <a name="example"></a>Ejemplo
Como ejemplo, si tiene una máquina virtual local con cuatro núcleos con un uso del 20 % y una memoria de 8 GB con un uso del 10 %, las evaluaciones serán las siguientes:

- **Evaluación basada en el rendimiento**:
    - Identifica la memoria y los núcleos efectivos según el uso del núcleo (4 x 0,20 = 0,8) y la memoria (8 GB x 0,10 = 0,8).
    - Aplica el factor de confort especificado en las propiedades de evaluación (por ejemplo, 1,3 x) para obtener los valores que se van a usar para el ajuste de tamaño. 
    - Recomienda el tamaño de máquina virtual más cercano en Azure que puede admitir ~1,04 núcleos (0,8 x 1,3) y una memoria de ~1,04 GB (0,8 x 1,3).

- **Evaluación tal cual (como en el entorno local)** :
    -  Recomienda una máquina virtual con cuatro núcleos; 8 GB de memoria.

## <a name="best-practices-for-creating-assessments"></a>Procedimientos recomendados para crear evaluaciones

El dispositivo Azure Migrate genera perfiles continuamente de su entorno local y envía metadatos y datos de rendimiento a Azure. Siga estos procedimiento recomendado para evaluar los servidores detectados mediante un dispositivo:

- **Crear evaluaciones tal cual**: puede crear evaluaciones tal cual inmediatamente después de que se muestren las máquinas en el portal de Azure Migrate.
- **Crear una evaluación basada en el rendimiento**: después de configurar la detección, le recomendamos esperar al menos un día antes de ejecutar una evaluación basada en el rendimiento:
    - La recopilación de datos de rendimiento lleva tiempo. El hecho de esperar al menos un día garantiza que haya suficientes puntos de datos de rendimiento antes de ejecutar la evaluación.
    - Cuando ejecuta evaluaciones basadas en el rendimiento, asegúrese de generar un perfil del entorno para la duración de la evaluación. Por ejemplo, si crea una evaluación con la duración de rendimiento establecida en una semana, debe esperar al menos una semana después de iniciar la detección para que se recopilen todos los puntos de datos. Si no lo hace, la evaluación no obtendrá una clasificación de cinco estrellas.
- **Recalcular evaluaciones**: dado que las evaluaciones son instantáneas de un momento dado, no se actualizan automáticamente con los datos más recientes. Para actualizar una evaluación con los datos más recientes, debe recalcularla.

Siga estos procedimientos recomendados para evaluar los servidores importados en Azure Migrate mediante un archivo .CSV:

- **Crear evaluaciones tal cual**: puede crear evaluaciones tal cual inmediatamente después de que se muestren las máquinas en el portal de Azure Migrate.
- **Crear una evaluación basada en el rendimiento**: ayuda a obtener una mejor estimación de los costos, en especial si se ha aprovisionado en exceso la capacidad del servidor en el entorno local. Sin embargo, la precisión de la evaluación basada en el rendimiento depende de los datos de rendimiento que se especifiquen para los servidores. 
- **Recalcular evaluaciones**: dado que las evaluaciones son instantáneas de un momento dado, no se actualizan automáticamente con los datos más recientes. Para actualizar una evaluación con los datos importados más recientes, debe volver a calcularla.

## <a name="best-practices-for-confidence-ratings"></a>Procedimientos recomendados para las clasificaciones de confianza

Cuando realiza evaluaciones basadas en el rendimiento, se asigna a la evaluación una clasificación de confianza que puede oscilar entre 1 estrella (la más baja) y 5 estrellas (la más alta). Para utilizar de manera eficiente las clasificaciones de confianza:
- Azure Migrate Server Assessment necesita los datos de uso de la memoria o la CPU de la máquina virtual.
- Para cada disco asociado a la máquina virtual local, necesita los datos de rendimiento o IOPS de lectura o escritura.
- Para cada adaptador de red asociado a la máquina virtual, necesita los datos de entrada o salida de la red.

La clasificación de confianza de una evaluación se proporciona en función del porcentaje de puntos de datos disponibles para la duración seleccionada, tal y como se resume en la tabla siguiente:

   **Disponibilidad del punto de datos** | **Clasificación de confianza**
   --- | ---
   0 % - 20 % | 1 estrella
   21 % - 40 % | 2 estrellas
   41 % - 60 % | 3 estrellas
   61 % - 80 % | 4 estrellas
   81 % - 100 % | 5 estrellas


## <a name="common-assessment-issues"></a>Problemas de evaluación comunes

A continuación, se indica cómo se deben abordar algunos problemas comunes del entorno que afectan a las evaluaciones.

###  <a name="out-of-sync-assessments"></a>Evaluaciones sin sincronizar

Si agrega o quita máquinas de un grupo después de crear una evaluación, la evaluación que ha creado se marcará **Sin sincronizar**. Vuelva a ejecutar la evaluación **(Recalcular**) para reflejar los cambios en el grupo.

### <a name="outdated-assessments"></a>Evaluaciones obsoletas

Si hay cambios en el entorno local de las máquinas virtuales que se encuentran en un grupo que se ha evaluado, la evaluación se marca como **Obsoleta**. Para reflejar los cambios, vuelva a ejecutar la evaluación.

### <a name="low-confidence-rating"></a>Clasificación de confianza baja

Puede que una evaluación no tenga todos los puntos de datos debido a varios motivos:

- No generó un perfil de su entorno durante el tiempo que está creando la evaluación. Por ejemplo, si está creando una *evaluación basada en el rendimiento* con la duración de rendimiento establecida en una semana, debe esperar al menos una semana después de iniciar la detección para que se recopilen todos los puntos de datos. Puede hacer clic en **Recalcular** en cualquier momento para consultar la clasificación de confianza aplicable más reciente. Tenga en cuenta que la clasificación de confianza solo es aplicable al crear una evaluación *basada en el rendimiento*.

- Se apagaron algunas máquinas virtuales en el período durante el que se calcula la valoración. Si alguna máquina virtual se apagó por algún tiempo, Server Assessment no podrá recopilar los datos de rendimiento durante ese período.

- Algunas máquinas virtuales se crearon después de iniciar la detección en Server Assessment. Por ejemplo, si va a crear una valoración para el historial de rendimiento del último mes, pero algunas máquinas virtuales se crearon en el entorno hace solo una semana. En este caso, los datos de rendimiento de las nuevas máquinas virtuales no estarán disponibles en ningún momento y la clasificación de confianza sería baja.


## <a name="next-steps"></a>Pasos siguientes

- [Averigüe](concepts-assessment-calculation.md) cómo se calculan las evaluaciones.
- [Averigüe](how-to-modify-assessment.md) cómo puede personalizar una evaluación.
