---
title: Creación de una valoración de VM de Azure con la herramienta de valoración de servidores de Azure Migrate | Microsoft Docs
description: En este artículo se describe cómo crear una valoración de VM de Azure con la herramienta de valoración de servidores de Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/15/2019
ms.author: raynew
ms.openlocfilehash: 534619ace09b4e11934062a591adf8d9ab6f77ad
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96500808"
---
# <a name="create-an-azure-vm-assessment"></a>Creación de una valoración de máquina virtual de Azure

En este artículo se describe cómo crear una valoración de VM de Azure para máquinas virtuales locales de VMware o de Hyper-V con Azure Migrate: Server Assessment.

[Azure Migrate](migrate-services-overview.md) le ayuda a migrar a Azure. Azure Migrate proporciona un centro principal para realizar el seguimiento de la detección, valoración y migración de infraestructuras, aplicaciones y datos locales a Azure. El centro proporciona herramientas de Azure para la valoración y migración, así como ofertas de proveedores de software independientes (ISV) externos. 

## <a name="before-you-start"></a>Antes de comenzar

- Asegúrese de que ha [creado](./create-manage-projects.md) un proyecto de Azure Migrate.
- Si ya ha creado un proyecto, asegúrese de que ha [agregado](how-to-assess.md) la herramienta Azure Migrate: Server Assessment.
- Para crear una evaluación, debe configurar una aplicación Azure Migrate para [VMware](how-to-set-up-appliance-vmware.md) o [Hyper-V](how-to-set-up-appliance-hyper-v.md). El dispositivo detecta máquinas locales y envía metadatos y datos de rendimiento a Azure Migrate: Server Assessment. [Más información](migrate-appliance.md).


## <a name="azure-vm-assessment-overview"></a>Introducción a las valoraciones de VM de Azure
Existen dos tipos de criterios de tamaño que puede usar para crear valoraciones de VM de Azure mediante Azure Migrate: Server Assessment.

**Valoración** | **Detalles** | **Data**
--- | --- | ---
**Basada en el rendimiento** | Evaluaciones basadas en los datos de rendimiento recopilados | **Tamaño de máquina virtual recomendado**: se basa en los datos de uso de la CPU y de la memoria.<br/><br/> **Tipo de disco recomendado (disco administrado estándar o Premium**): se basa en IOPS y en el rendimiento de los discos locales.
**Como local** | Evaluaciones que se basan en el tamaño local. | **Tamaño de máquina virtual recomendado**: se basa en el tamaño de la máquina virtual local<br/><br> **Tipo de disco recomendado**: se basa en el valor del tipo de almacenamiento que se selecciona para la evaluación.

[Más información](concepts-assessment-calculation.md) sobre las evaluaciones.

## <a name="run-an-assessment"></a>Ejecución de una evaluación

Las evaluaciones se realizan como se indica a continuación:

1. Consulte los [procedimientos recomendados](best-practices-assessment.md) para crear evaluaciones.
2. En la pestaña **Servidores**, en el icono **Azure Migrate: Server Assessment**, haga clic en **Evaluar**.

    ![Captura de pantalla que muestra los servidores de Azure Migrate con la opción Evaluar seleccionada en Herramientas de evaluación.](./media/how-to-create-assessment/assess.png)

3. En **Evaluar los servidores**, seleccione el tipo de evaluación como "Máquina virtual de Azure", seleccione el origen de detección y especifique el nombre de la evaluación.

    ![Aspectos básicos de la valoración](./media/how-to-create-assessment/assess-servers-azurevm.png)

4. Haga clic en **View all** (Ver todo) para revisar la configuración de la valoración.

    ![Propiedades de la evaluación](./media/how-to-create-assessment//view-all.png)

5. Haga clic en **Siguiente** para **seleccionar las máquinas que se van a evaluar**. En **Seleccionar o crear un grupo**, seleccione **Crear nuevo** y especifique un nombre de grupo. Un grupo recopila una o varias máquinas virtuales para su evaluación.
6. En **Agregar máquinas al grupo**, seleccione las máquinas virtuales que se van a agregar al grupo.
7. Haga clic en **Siguiente** para **Revisar y crear evaluación** para revisar los detalles de la evaluación.
8. Haga clic en **Crear evaluación** para crear el grupo y realizar la evaluación.

    ![Crear una evaluación](./media/how-to-create-assessment//assessment-create.png)

9. Una vez creada la evaluación, se puede ver en **Servidores** > **Azure Migrate: Server Assessment** > **Evaluaciones**.
10. Haga clic en **Exportar la evaluación** para descargarla como un archivo de Excel.



## <a name="review-an-azure-vm-assessment"></a>Revisión de una valoración de VM de Azure

Una valoración de VM de Azure describe:

- **Preparación para Azure**: si las máquinas virtuales son adecuadas para la migración a Azure.
- **Estimación del costo mensual**: los costos mensuales estimados de proceso y almacenamiento por ejecutar las máquinas virtuales en Azure.
- **Estimación del costo mensual de almacenamiento**: costos estimados del almacenamiento en disco después de la migración.

### <a name="view-an-azure-vm-assessment"></a>Visualización de una valoración de VM de Azure

1. En **Objetivos de migración** >  **Servidores**, haga clic en **Evaluaciones** en **Azure Migrate: Server Assessment**.
2. En **Evaluaciones**, haga clic en una evaluación para abrirla.

    ![Resumen de evaluaciones](./media/how-to-create-assessment/assessment-summary.png)

### <a name="review-azure-readiness"></a>Revisión de la preparación para Azure

1. En **Preparación para Azure**, compruebe si las máquinas virtuales están listas para su migración a Azure.
2. Revise el estado de la máquina virtual:
    - **Preparada para Azure**: Azure Migrate recomienda unas estimaciones de tamaño y costo de las máquinas virtuales en la evaluación.
    - **Preparado con condiciones**: muestra los problemas y las soluciones que se sugieren.
    - **No está preparado para Azure**: muestra los problemas y las soluciones que se sugieren.
    - **Preparación desconocida**: se usa cuando Azure Migrate no puede evaluar la preparación debido a problemas de disponibilidad de datos.

3. Haga clic en cualquiera de los estados de **Preparación para Azure**. Puede ver los detalles de la preparación de la máquina virtual y explorar en profundidad los detalles de esta, entre los que se incluye la configuración de proceso, almacenamiento y red.



### <a name="review-cost-details"></a>Revisión de los datos de costo

Esta vista muestra el costo estimado de almacenamiento y proceso que conlleva ejecutar máquinas virtuales en Azure.

1. Revise los costos mensuales de proceso y almacenamiento. Los costos se agregan para todas las máquinas virtuales del grupo evaluado.

    - Las estimaciones de costo se basan en las recomendaciones de tamaño de una máquina, así como en sus discos y propiedades.
    - Se muestran los costos mensuales estimados de proceso y almacenamiento.
    - La estimación de costos es para ejecutar las máquinas virtuales locales como máquinas virtuales IaaS. Azure Migrate Server Assessment no tiene en cuenta los costos de PaaS o SaaS.

2. Puede revisar las estimaciones del costo de almacenamiento mensual. Esta vista muestra los costos de almacenamiento agregados del grupo evaluado, divididos por los diferentes tipos de discos de almacenamiento.
3. Puede explorar en profundidad para ver los detalles de cada una de las máquinas virtuales.


### <a name="review-confidence-rating"></a>Examen de la clasificación de confianza

Cuando se realizan valoraciones basadas en el rendimiento, se asigna una clasificación de confianza a la evaluación.

![Clasificación de confianza](./media/how-to-create-assessment/confidence-rating.png)

- Se concede una clasificación que oscila entre 1 estrella (la más baja) y 5 estrellas (la más alta).
- La clasificación de confianza sirve de ayuda para calcular la confiabilidad de las recomendaciones de tamaño que proporciona la evaluación.
- La clasificación de confianza se basa en la disponibilidad de los puntos de datos necesarios para calcular tal evaluación.

Estas son las posibles clasificaciones de confianza de una evaluación.

**Disponibilidad del punto de datos** | **Clasificación de confianza**
--- | ---
0 % - 20 % | 1 estrella
21 % - 40 % | 2 estrellas
41 % - 60 % | 3 estrellas
61 % - 80 % | 4 estrellas
81 % - 100 % | 5 estrellas




## <a name="next-steps"></a>Pasos siguientes

- Aprenda a usar la [asignación de dependencias de máquina](how-to-create-group-machine-dependencies.md) para crear grupos de confianza alta.
- [Obtenga más información](concepts-assessment-calculation.md) sobre cómo se calculan las evaluaciones.