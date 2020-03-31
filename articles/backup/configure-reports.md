---
title: Configuración de informes de Azure Backup
description: Configuración y visualización de informes para Azure Backup con Log Analytics y Azure Workbooks
ms.topic: conceptual
ms.date: 02/10/2020
ms.openlocfilehash: 651d1383f0f292895ed95c91bafd5206d4f04c2c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78161208"
---
# <a name="configure-azure-backup-reports"></a>Configuración de informes de Azure Backup

Un requisito común para los administradores de copias de seguridad es obtener información sobre las copias de seguridad, en función de los datos que abarcan un período largo de tiempo. Podría haber varios casos de uso para esta solución: asignación y previsión del almacenamiento en la nube consumido, auditoría de copias de seguridad y restauraciones e identificación de tendencias clave en diferentes niveles de granularidad.

En la actualidad, Azure Backup proporciona una solución de informes que aprovecha las ventajas de los [registros de Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) y [Azure Workbooks](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks), lo que ayuda a obtener información detallada sobre el conjunto de las copias de seguridad. En este artículo se explica cómo configurar y ver informes de copia de seguridad.

## <a name="supported-scenarios"></a>Escenarios admitidos

* Los informes de copia de seguridad son compatibles con las máquinas virtuales de Azure, SQL en máquinas virtuales de Azure, SAP HANA/ASE en máquinas virtuales de Azure, el agente de Azure Backup (MARS), Azure Backup Server (MABS) y System Center DPM.
* En las cargas de trabajo de DPM, se admiten los informes de copia de seguridad para la versión 5.1.363.0 de DPM y versiones posteriores y la versión 2.0.9127.0 del agente y posteriores.
* En las cargas de trabajo de MABS, se admiten los informes de copia de seguridad para la versión 13.0.415.0 de MABS y versiones posteriores y la versión 2.0.9170.0 del agente y posteriores.
* Los informes de copia de seguridad se pueden ver en todos los elementos de copia de seguridad, almacenes, suscripciones y regiones, siempre y cuando sus datos se envíen a un área de trabajo Log Analytics (LA) a la que el usuario tenga acceso. 
* Si es usuario de [Azure Lighthouse](https://docs.microsoft.com/azure/lighthouse/) con acceso delegado a las suscripciones de los clientes, puede usar estos informes con Azure Lighthouse para ver los informes de todos los inquilinos.
* Los datos de los trabajos de copia de seguridad de registros no se muestran actualmente en los informes.

## <a name="getting-started"></a>Introducción

Para empezar a usar los informes, siga los tres pasos que se detallan a continuación:

1. **Cree un área de trabajo de Log Analytics (LA) (o use una existente):**

Debe configurar una o varias áreas de trabajo de LA para almacenar los datos de los informes de copia de seguridad. La ubicación y la suscripción donde se puede crear este área de trabajo es independiente de la ubicación y la suscripción donde existen los almacenes. 

Consulte el artículo siguiente: [Creación de un área de trabajo de Log Analytics en Azure Portal](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace) para configurar un área de trabajo de LA.

De forma predeterminada, los datos de un área de trabajo de LA se conservan durante 30 días. Para ver los datos de un horizonte temporal más largo, cambie el período de retención del área de trabajo de LA. Para cambiar el período de retención, consulte el artículo siguiente: [Administración del uso y los costos con los registros de Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage).

2. **Configure los valores de diagnóstico para los almacenes:**

Los recursos de Azure Resource Manager como, por ejemplo, el almacén de Recovery Services, registran información acerca de las operaciones programadas y las operaciones desencadenadas por el usuario como datos de diagnóstico. 

En la sección de supervisión del almacén de Recovery Services, seleccione **Configuración de diagnóstico** y especifique el destino de los datos de diagnóstico del almacén de Recovery Services. [Más información sobre el uso de eventos de diagnóstico](https://docs.microsoft.com/azure/backup/backup-azure-diagnostic-events).

![Hoja de Configuración de diagnóstico](./media/backup-azure-configure-backup-reports/resource-specific-blade.png)

Azure Backup también proporciona una directiva integrada de Azure Policy, que automatiza la configuración de diagnóstico para todos los almacenes de un ámbito determinado. Consulte el siguiente artículo para obtener información sobre el uso de esta directiva: [Configuración de los valores de diagnóstico del almacén a gran escala](https://docs.microsoft.com/azure/backup/azure-policy-configure-diagnostics)

> [!NOTE]
> Una vez que configure los diagnósticos, las inserciones de datos iniciales pueden tardar hasta 24 horas en completarse. Una vez que los datos comienzan a fluir en el área de trabajo de LA, es posible que no pueda ver los datos en los informes inmediatamente, ya que los datos para el día parcial actual no se muestran en los informes (más detalles [aquí](https://docs.microsoft.com/azure/backup/configure-reports#conventions-used-in-backup-reports)). Por lo tanto, se recomienda empezar a ver los informes 2 días después de configurar los almacenes para enviar datos a Log Analytics.

3. **Visualice los informes en Azure Portal:**

Una vez que haya configurado los almacenes para enviar los datos a LA, puede visualizar los informes de copia de seguridad; para ello, vaya a la hoja de cualquier almacén y haga clic en el elemento de menú **Informes de copias de seguridad**. 

![Panel del almacén](./media/backup-azure-configure-backup-reports/vault-dashboard.png)

Al hacer clic en este vínculo, se abre el libro del informe de copia de seguridad.

> [!NOTE]
> * Actualmente, la carga inicial del informe puede tardar hasta 1 minuto.
> * El almacén de Recovery Services es simplemente un punto de entrada para los informes de copia de seguridad. Una vez que el libro del informe de copia de seguridad se abra en la hoja de un almacén, podrá ver los datos agregados en todos los almacenes (si selecciona el conjunto adecuado de las áreas de trabajo de LA).

A continuación se muestra una descripción de las distintas pestañas que contiene el informe:

* **Resumen**: la pestaña Resumen proporciona información general de alto nivel sobre el conjunto de copias de seguridad. En la pestaña Resumen, puede obtener una vista rápida del número total de elementos de copia de seguridad, el almacenamiento en la nube total consumido, el número de instancias protegidas y la tasa de éxito de los trabajos por tipo de carga de trabajo. Para obtener información más detallada sobre un tipo de artefacto de copia de seguridad específico, vaya a las pestañas correspondientes.

![Pestaña Resumen](./media/backup-azure-configure-backup-reports/summary.png)

* **Elementos de copia de seguridad**: la pestaña Elementos de copia de seguridad permite ver información y tendencias sobre el almacenamiento en la nube consumido en el nivel de elemento de copia de seguridad. Por ejemplo, si usa la copia de seguridad de SQL en máquinas virtuales de Azure, puede ver el almacenamiento en la nube consumido por cada base de datos SQL de la que se realiza copia de seguridad. También puede ver los datos de los elementos de copia de seguridad de un estado de protección determinado. Por ejemplo, al hacer clic en el icono **Protección detenida** en la parte superior de la pestaña, se filtran todos los widgets siguientes para mostrar solo los datos de los elementos de copia de seguridad en estado de protección detenida.

![Pestaña Elementos de copia de seguridad](./media/backup-azure-configure-backup-reports/backup-items.png)

* **Uso**: la pestaña uso le ayuda a ver los parámetros de facturación principales de las copias de seguridad. La información que se muestra en esta pestaña corresponde al nivel de entidad de facturación (contenedor protegido). Por ejemplo, en el caso de que se realice una copia de seguridad de un servidor de DPM en Azure, puede ver la tendencia de las instancias protegidas y el almacenamiento en la nube consumido para el servidor de DPM. Del mismo modo, si usa SQL en Azure Backup o SAP HANA en Azure Backup, esta pestaña proporciona información relacionada con el uso en el nivel de la máquina virtual en la que se encuentran estas bases de datos.

![Pestaña Uso](./media/backup-azure-configure-backup-reports/usage.png)

* **Trabajos**: la pestaña Trabajos le permite ver tendencias de ejecución prolongada de los trabajos, como el número de trabajos con errores por día y las causas principales de los errores de los trabajos. Puede ver esta información en un nivel agregado y en el nivel de elemento de copia de seguridad. Al hacer clic en un elemento de copia de seguridad determinado en una cuadrícula, puede ver información detallada sobre cada trabajo que se desencadenó en ese elemento de copia de seguridad en el intervalo de tiempo seleccionado.

![Pestaña Trabajos](./media/backup-azure-configure-backup-reports/jobs.png)

* **Directivas**: la pestaña Directivas le permite ver información sobre todas las directivas activas, como el número de elementos asociados y el almacenamiento en la nube total consumido por los elementos de los que se ha realizado una copia de seguridad por una directiva determinada. Al hacer clic en una directiva determinada, puede ver información sobre cada uno de los elementos de copia de seguridad asociados.

![Pestaña Directivas](./media/backup-azure-configure-backup-reports/policies.png)

## <a name="exporting-to-excel"></a>Exportación a Excel

Al hacer clic en el botón de flecha abajo situado en la parte superior derecha de cualquier widget (tabla o gráfico), se exporta el contenido de ese widget como una hoja de Excel, tal cual con los filtros existentes aplicados. Para exportar más filas de una tabla a Excel, puede aumentar el número de filas que se muestran en la página mediante la lista desplegable **Filas por página** en la parte superior de cada cuadrícula.

## <a name="pinning-to-dashboard"></a>Anclar al panel

Haga clic en el icono de anclaje en la parte superior de cada widget para anclar el widget al panel de Azure Portal. Esto le ayudará a crear paneles personalizados adaptados para mostrar la información más importante que necesita.

## <a name="cross-tenant-reports"></a>Informes entre inquilinos

Si es usuario de [Azure Lighthouse](https://docs.microsoft.com/azure/lighthouse/) con acceso delegado a las suscripciones en varios entornos de inquilinos, puede usar el filtro de suscripción predeterminado (haciendo clic en el icono de filtro en la parte superior derecha de Azure Portal) para elegir todas las suscripciones para las que desee ver datos. Esto le permitirá seleccionar áreas de trabajo de LA de los inquilinos para ver informes multiinquilino.

## <a name="conventions-used-in-backup-reports"></a>Convenciones usadas en los informes de copia de seguridad

* Los filtros funcionan de izquierda a derecha y de arriba abajo en cada pestaña, es decir, cualquier filtro solo se aplica a aquellos widgets que se colocan a la derecha del filtro o debajo de ese filtro. 
* Al hacer clic en un icono coloreado, se filtran los widgets situados debajo del icono para los registros que pertenecen al valor de ese icono. Por ejemplo, al hacer clic en el icono *Protección detenida* en la pestaña Elementos de copia de seguridad, se filtran las cuadrículas y los gráficos siguientes para mostrar solo los datos de los elementos de copia de seguridad en estado de protección detenida.
* No se pueden hacer clic en los iconos que no son de color.
* Los datos del día parcial actual no aparecen en los informes. Por lo tanto, cuando el valor seleccionado de **Intervalo de tiempo** es, ***Últimos 7 días***, el informe muestra los registros de los últimos 7 días completados (que no incluyen el día actual).
* El informe muestra los detalles de los trabajos (aparte de los trabajos de registro) que se **desencadenaron** en el intervalo de tiempo seleccionado. 
* Los valores que se muestran para el almacenamiento en la nube y las instancias protegidas corresponden al **final** del intervalo de tiempo seleccionado.
* Los elementos de copia de seguridad que se muestran en los informes son los que existen al **final** del intervalo de tiempo seleccionado. No se muestran los elementos de copia de seguridad que se eliminaron en medio del intervalo de tiempo seleccionado. También se aplica la misma convención para las directivas de copia de seguridad.

## <a name="query-load-times"></a>Tiempos de carga de las consultas

Los widgets del informe de copia de seguridad se basan en consultas de Kusto, que se ejecutan en las áreas de trabajo de LA del usuario. Como estas consultas normalmente implican el procesamiento de grandes cantidades de datos, con varias combinaciones para habilitar información más completa, es posible que los widgets no se carguen de forma instantánea cuando el usuario visualiza informes en un conjunto extenso de copias de seguridad. En la tabla siguiente se proporciona una estimación aproximada del tiempo que pueden tardar en cargarse los distintos widgets, en función del número de elementos de copia de seguridad y el intervalo de tiempo para el que se visualiza el informe:

| **N.º de orígenes de datos**                         | **Horizonte temporal** | **Tiempos de carga (aprox.)**                                              |
| --------------------------------- | ------------- | ------------------------------------------------------------ |
| ~5 K                       | 1 mes          | Iconos: 5 a 10 segundos <br> Cuadrículas: 5 a 10 segundos <br> Gráficos: 5 a 10 segundos <br> Filtros de nivel de informe: 5 a 10 segundos|
| ~5 K                       | 3 meses          | Iconos: 5 a 10 segundos <br> Cuadrículas: 5 a 10 segundos <br> Gráficos: 5 a 10 segundos <br> Filtros de nivel de informe: 5 a 10 segundos|
| ~10 K                       | 3 meses          | Iconos: 15 a 20 segundos <br> Cuadrículas: 15 a 20 segundos <br> Gráficos: 1 a 2 minutos <br> Filtros de nivel de informe: 25 a 30 segundos|
| ~15 K                       | 1 mes          | Iconos: 15 a 20 segundos <br> Cuadrículas: 15 a 20 segundos <br> Gráficos: 50 a 60 segundos <br> Filtros de nivel de informe: 20 a 25 segundos|
| ~15 K                       | 3 meses          | Iconos: 20 a 30 segundos <br> Cuadrículas: 20 a 30 segundos <br> Gráficos: 2 a 3 minutos <br> Filtros de nivel de informe: 50 a 60 segundos |

## <a name="what-happened-to-the-power-bi-reports"></a>¿Qué ha ocurrido con los informes de Power BI?
* Nuestra versión anterior de la plantilla de Power BI para la creación de informes (que tiene como origen de los datos una cuenta de Azure Storage) está en una ruta de desuso. Se recomienda comenzar a enviar los datos de diagnóstico del almacén a Log Analytics, como se describió anteriormente, para ver los informes.

* Además, el esquema V1 de envío de datos de diagnóstico a una cuenta de almacenamiento o a un área de trabajo de LA también se encuentra en una ruta de desuso. Esto significa que, si ha escrito consultas personalizadas o automatizaciones basadas en el esquema v1, se recomienda actualizar estas consultas para usar el esquema V2 actualmente admitido.

## <a name="next-steps"></a>Pasos siguientes
[Más información sobre la supervisión y la generación de informes con Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-monitor-alert-faq)