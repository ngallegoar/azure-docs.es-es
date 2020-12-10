---
title: Evaluación de las máquinas virtuales de VMware para la migración a Azure VMware Solution (AVS) con Azure Migrate
description: Aprenda a evaluar las máquinas virtuales de VMware para la migración a AVS con Azure Migrate Server Assessment.
author: rashi-ms
ms.author: rajosh
ms.manager: abhemraj
ms.topic: tutorial
ms.date: 09/14/2020
ms.custom: MVC
ms.openlocfilehash: f6d3c6f77b062939a88e7277cb7f0ab6ecff9fcb
ms.sourcegitcommit: ea551dad8d870ddcc0fee4423026f51bf4532e19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2020
ms.locfileid: "96753083"
---
# <a name="tutorial-assess-vmware-vms-for-migration-to-avs"></a>Tutorial: Evaluación de máquinas virtuales de VMware para la migración a AVS

Como parte del recorrido de la migración a Azure, puede evaluar las cargas de trabajo locales para medir la preparación de la nube, identificar los riesgos y hacer una estimación de los costos y la complejidad.

En este artículo se muestra cómo evaluar las máquinas virtuales de VMware detectadas para la migración a Azure VMware Solution (AVS) mediante la herramienta Azure Migrate: Server Assessment. AVS es un servicio administrado que le permite ejecutar la plataforma de VMware en Azure.

En este tutorial, aprenderá a:
> [!div class="checklist"]
- Ejecutar una valoración basada en los metadatos y la información de configuración de la máquina.
- Ejecutar una valoración basada en los datos de rendimiento.

> [!NOTE]
> Los tutoriales muestran la manera más rápida de probar un escenario y utilizar las opciones predeterminadas siempre que sea posible. 

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.



## <a name="prerequisites"></a>Requisitos previos

Antes de seguir este tutorial y evaluar las máquinas para la migración a AVS, asegúrese de detectar las máquinas que desee evaluar:

- Para detectar las máquinas mediante el dispositivo de Azure Migrate, [siga este tutorial](tutorial-discover-vmware.md). 
- Para detectar las máquinas mediante un archivo CSV importado, [siga este tutorial](tutorial-discover-import.md).


## <a name="decide-which-assessment-to-run"></a>Decisión de qué valoración ejecutar


Decida si desea ejecutar una valoración usando criterios de ajuste de tamaño en función de los datos de configuración de la máquina o los metadatos que se recopilan tal cual en el entorno local o en los datos de rendimiento dinámicos.

**Valoración** | **Detalles** | **Recomendación**
--- | --- | ---
**Tal cual en el entorno local** | Valore en función de los datos o los metadatos de configuración de la máquina.  | El tamaño de nodo recomendado en AVS se basa en el tamaño de la máquina virtual local, junto con la configuración especificada en la evaluación para el tipo de nodo, el tipo de almacenamiento y la configuración de la tolerancia a errores.
**Basada en el rendimiento** | Valore en función de los datos de rendimiento dinámico recopilados. | El tamaño de nodo recomendado en AVS se basa en los datos de uso de CPU y memoria, junto con los valores que se especifican en la evaluación para el tipo de nodo, el tipo de almacenamiento y la configuración de la tolerancia a errores.

## <a name="run-an-assessment"></a>Ejecución de una evaluación

Las evaluaciones se realizan como se indica a continuación:

1. En la página **Servidores** > **Servidores Windows y Linux**, haga clic en **Evaluar y migrar servidores**.

   ![Ubicación del botón Evaluar y migrar servidores](./media/tutorial-assess-vmware-azure-vmware-solution/assess.png)

2. En **Azure Migrate: Server Assessment**, haga clic en **Evaluar**.

3. En **Evaluar los servidores** > **Tipo de evaluación**, seleccione **Azure VMware Solution (AVS) (versión preliminar)** .
4. En **Origen de detección**:

    - Si ha detectado máquinas que usan el dispositivo, seleccione **Máquinas detectadas desde el dispositivo de Azure Migrate**.
    - Si ha detectado máquinas que usan un archivo CSV importado, seleccione **Máquinas importadas**. 
    
5. Especifique un nombre para la valoración. 
6. Haga clic en **View all** (Ver todo) para revisar la configuración de la valoración.

    ![Página para seleccionar la configuración de la evaluación](./media/tutorial-assess-vmware-azure-vmware-solution/assess-servers.png)


7. En **Propiedades de evaluación** > **Propiedades de destino**:

    - En **Ubicación de destino**, especifique la región de Azure a la que desee migrar.
       - Las recomendaciones de tamaño y costo se basan en la ubicación que especifique.
       - Actualmente puede realizar evaluaciones en tres regiones (Este de EE. UU., Oeste de EE. UU. y Oeste de Europa)
   - En **Tipo de almacenamiento**, deje **vSAN**. Es el predeterminado para una nube privada de AVS.
   - Las **instancias reservadas** no se admiten actualmente para los nodos AVS.
8. En **Tamaño de VM**:
    - En **Tipo de nodo**, seleccione un tipo de nodo en función de las cargas de trabajo que se ejecuten en las máquinas virtuales locales.
        - Azure Migrate recomienda el tipo de nodos necesarios para migrar las máquinas virtuales a AVS.
        - El tipo de nodo predeterminado es AV36.
    - En **Configuración de FTT, nivel de RAID**, seleccione la combinación de tolerancia a errores y RAID.  La opción de FTT seleccionada junto con el requisito de disco de la máquina virtual local determinará el almacenamiento de vSAN total que se requiere en AVS.
    - En **CPU Oversubscription** (Suscripción excesiva de CPU), especifique la proporción de núcleos virtuales asociados a un núcleo físico en el nodo AVS. Tenga en cuenta que una suscripción excesiva de vCPU mayor que 4:1 puede producir una degradación del rendimiento, pero se puede usar para cargas de trabajo del tipo del servidor web.

9. En **Tamaño del nodo**: 
    - En **Criterio de tamaño**, seleccione si desea basar la evaluación en metadatos estáticos o en datos basados en el rendimiento. Si utiliza datos de rendimiento:
        - En **Historial de rendimiento**, indique la duración de los datos en los que desee basar la valoración.
        - En **Uso de percentil**, especifique el valor de percentil que desee utilizar para la muestra de rendimiento. 
    - En **Factor de confort**, indique el búfer que desee usar durante la valoración. Tiene en cuenta problemas como el uso estacional, el historial de rendimiento corto y los posibles aumentos en el uso futuro. Por ejemplo, si usa un factor de confort de dos:
    
        **Componente** | **Utilización efectiva** | **Agregar factor de confort (2,0)**
        --- | --- | ---  
        Núcleos | 2 | 4
        Memoria | 8 GB | 16 GB     

10. En **Precios**:
    - En **Oferta**, la [oferta de Azure](https://azure.microsoft.com/support/legal/offer-details/) en la que se ha inscrito muestra el costo que Server Assessment calcula para esa oferta.
    - En **Moneda**, seleccione la moneda de facturación para la cuenta.
    - En **Descuento (%)** , agregue cualquier descuento específico de la suscripción que reciba a partir de la oferta de Azure. La configuración predeterminada es 0 %.

11. Haga clic en **Guardar** si realiza cambios.

    ![Propiedades de la evaluación](./media/tutorial-assess-vmware-azure-vmware-solution/view-all.png)

12. En **Evaluar los servidores**, haga clic en **Siguiente**.
13. En **Evaluar los servidores** > **Seleccione las máquinas que quiera evaluar**, para crear un nuevo grupo de servidores para la evaluación, seleccione **Crear nuevo** y especifique un nombre de grupo. 
14. Seleccione el dispositivo y las máquinas virtuales que desee agregar al grupo. A continuación, haga clic en **Siguiente**.
15. En **Revisar y crear valoración**, revise los detalles de la valoración y haga clic en **Crear evaluación** para crear el grupo y ejecutar la evaluación.

    > [!NOTE]
    > En el caso de las valoraciones basadas en el rendimiento, se recomienda esperar al menos un día después de iniciar la detección antes de crear una. De este modo, gana tiempo para recopilar los datos de rendimiento con mayor confianza. Lo mejor es que, después de iniciar la detección, espere el tiempo que especifique (día/semana/mes) para que la confianza sea alta.

## <a name="review-an-assessment"></a>Revisión de una evaluación

Una evaluación de AVS describe:

- La preparación de AVS: para ver si las máquinas virtuales locales son adecuadas para la migración a Azure VMware Solution (AVS).
- Número de nodos de AVS: número estimado de nodos de AVS necesarios para ejecutar las máquinas virtuales.
- Uso en los nodos de AVS: uso previsto de CPU, memoria y almacenamiento en todos los nodos.
- Estimación del costo mensual: los costos mensuales estimados de todos los nodos de Azure VMware Solution (AVS) que ejecutan las máquinas virtuales locales.

## <a name="view-an-assessment"></a>Visualización de una evaluación

Para ver una evaluación:

1. En **Servidores** > **Azure Migrate: Server Assessment**, haga clic en el número junto a **Valoraciones**.
2. En **Evaluaciones**, seleccione una evaluación para abrirla. 
3. Revise el resumen de valoraciones. También puede editar las propiedades de la valoración o volver a calcularla.
 

### <a name="review-readiness"></a>Revisión de la preparación

1. Haga clic en **Preparación para Azure**.
2. En **Preparación para Azure**, revise el estado de la máquina virtual.

    - **Listo para AVS**: la máquina se puede migrar como está a Azure AVS, sin realizar ningún cambio. Se iniciará en AVS, con compatibilidad completa con AVS.
    - **Preparado con condiciones**: es posible que la máquina tenga problemas de compatibilidad con la versión actual de vSphere. Puede que deba tener instaladas las herramientas de VMware u otra configuración, para disfrutar de la funcionalidad completa en AVS.
    - **No está listo para AVS**: la máquina virtual no se iniciará en AVS. Por ejemplo, si una máquina virtual de VMware local tiene un dispositivo externo (como un CD-ROM) conectado y va a usar VMware VMotion, se producirá un error en la operación de VMotion.
 - **Preparación desconocida**: Azure Migrate no ha podido determinar la preparación de la máquina debido a una recopilación insuficiente de metadatos en el entorno local.

3. Revise la herramienta sugerida.

    - VMware HCX o Enterprise: En el caso de las máquinas de VMware, la solución Hybrid Cloud Extension (HCX) de VMWare es la herramienta de migración sugerida para migrar su carga de trabajo en el entorno local a la nube privada de Azure VMware Solution (AVS). Obtenga más información en.
    - Desconocido: En el caso de las máquinas importadas mediante un archivo CSV, se desconoce la herramienta de migración predeterminada. Sin embargo, para las máquinas de VMware, se recomienda usar la solución Hybrid Cloud Extension (HCX) de VMware.
4. Haga clic en un estado de Preparación para AVS. Puede ver los detalles de la preparación de la máquina virtual y explorar en profundidad los detalles de esta, entre los que se incluye la configuración de proceso, almacenamiento y red.

### <a name="review-cost-estimates"></a>Revisar estimaciones de costos

El resumen de evaluación muestra el costo estimado de almacenamiento y proceso que conlleva ejecutar máquinas virtuales en Azure. 

1. Revise los costos totales mensuales. Los costos se agregan para todas las máquinas virtuales del grupo evaluado.

    - Las estimaciones de los costos se basan en el número de nodos de AVS necesarios teniendo en cuenta los requisitos de recursos de todas las máquinas virtuales en total.
    - Como los precios de AVS son por nodo, el costo total no tiene costo de proceso ni distribución de costos de almacenamiento.
    - La estimación de costos es para ejecutar las máquinas virtuales locales en AVS. Azure Migrate Server Assessment no tiene en cuenta los costos de PaaS o SaaS.

2. Revise las estimaciones de almacenamiento mensuales. Esta vista muestra los costos de almacenamiento agregados del grupo evaluado, divididos por los diferentes tipos de discos de almacenamiento. 
3. Puede explorar en profundidad para ver los detalles de costo de máquinas virtuales específicas.

### <a name="review-confidence-rating"></a>Examen de la clasificación de confianza

Server Assessment asigna una clasificación de confianza a las valoraciones basadas en el rendimiento. La clasificación es de una estrella (más baja) a cinco estrellas (más alta).

![Clasificación de confianza](./media/tutorial-assess-vmware-azure-vmware-solution/confidence-rating.png)

La clasificación de confianza sirve de ayuda para calcular la confiabilidad de las recomendaciones de tamaño de la evaluación. Esta clasificación se basa en la disponibilidad de los puntos de datos necesarios para calcular tal valoración.

> [!NOTE]
> Las clasificaciones de confianza no se asignan si crea una valoración basada en un archivo CSV.

Las clasificaciones de confianza son las siguientes.

**Disponibilidad del punto de datos** | **Clasificación de confianza**
--- | ---
0 % - 20 % | 1 estrella
21 % - 40 % | 2 estrellas
41 % - 60 % | 3 estrellas
61 % - 80 % | 4 estrellas
81 % - 100 % | 5 estrellas

[Obtenga más información](concepts-assessment-calculation.md#confidence-ratings-performance-based) sobre la clasificación de confianza.

## <a name="next-steps"></a>Pasos siguientes

- Busque dependencias de máquina mediante la [asignación de dependencias](concepts-dependency-visualization.md).
- Configure la asignación de dependencias [sin agente](how-to-create-group-machine-dependencies-agentless.md) o [basada en agente](how-to-create-group-machine-dependencies.md).
