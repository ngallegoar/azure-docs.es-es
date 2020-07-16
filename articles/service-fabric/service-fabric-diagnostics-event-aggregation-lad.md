---
title: Agregación de eventos con Linux Azure Diagnostics
description: Obtenga información sobre la agregación y la recopilación de eventos con LAD para la supervisión y el diagnóstico de clústeres de Azure Service Fabric.
author: srrengar
ms.topic: conceptual
ms.date: 2/25/2019
ms.author: srrengar
ms.openlocfilehash: 453258bb5768a2faa8f4e42cce86d02125283026
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259189"
---
# <a name="event-aggregation-and-collection-using-linux-azure-diagnostics"></a>Recopilación y agregación de eventos con Linux Azure Diagnostics
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Cuando se ejecuta un clúster de Azure Service Fabric, es conveniente recopilar los registros de todos los nodos en una ubicación central. La presencia de los registros en una ubicación central facilita el análisis y la solución de los problemas del clúster o de las aplicaciones y los servicios que se ejecutan en ese clúster.

Uno de los métodos para cargar y recopilar registros es usar la extensión Linux Azure Diagnostics (LAD), que carga registros en Azure Storage, y también tiene la opción de enviar registros a Azure Application Insights o Event Hubs. Además, puede usar un proceso externo para leer los eventos desde el almacenamiento y colocarlos en un producto de plataforma de análisis, como los [registros de Azure Monitor](./service-fabric-diagnostics-oms-setup.md) u otra solución de análisis de registros.

## <a name="log-and-event-sources"></a>Orígenes de eventos y registros

### <a name="service-fabric-platform-events"></a>Eventos de plataforma de Service Fabric
Service Fabric genera una serie de registros de serie a través de [LTTng](https://lttng.org), incluidos eventos de runtime o eventos operativos. Estos registros se almacenan en la ubicación que la plantilla de Resource Manager del clúster especifica. Para obtener o establecer los detalles de la cuenta de almacenamiento, busque la etiqueta **AzureTableWinFabETWQueryable** y busque **StoreConnectionString**.

### <a name="application-events"></a>Eventos de aplicación
 Eventos emitidos desde el código de los servicios y las aplicaciones especificados por el usuario al instrumentar el software. Puede utilizar cualquier solución de registro que escriba archivos de registro basados en texto, por ejemplo, LTTng. Para obtener más información, consulte la documentación de LTTng sobre el seguimiento de la aplicación.

[Supervisión y diagnóstico de servicios en una configuración de desarrollo de máquina local](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)

## <a name="deploy-the-diagnostics-extension"></a>Implementación de la extensión de Diagnósticos
El primer paso para recopilar registros será implementar la extensión WAD en cada una de las máquinas virtuales del clúster de Service Fabric. La extensión de Diagnósticos recopila registros en cada máquina virtual y los carga a la cuenta de almacenamiento que especifique. 

Para implementar la extensión de Diagnósticos en las máquinas virtuales del clúster como parte de la creación de dicho clúster, establezca **Diagnósticos** en **Activado**. Después de crear el clúster, no se puede cambiar esta configuración mediante el portal, por lo que tendrá que realizar los cambios apropiados en la plantilla de Resource Manager.

Esto configura el agente LAD para supervisar los archivos de registro especificados. Siempre que se anexa una nueva línea al archivo, se crea una entrada syslog que se envía al almacenamiento (tabla) especificado.


## <a name="next-steps"></a>Pasos siguientes

1. Compruebe la [documentación de LTTng](https://lttng.org/docs) y el [uso de LAD](../virtual-machines/extensions/diagnostics-linux.md) para obtener información más detallada sobre qué eventos debe examinar durante la solución de problemas.
2. [Configuración del agente de Log Analytics](service-fabric-diagnostics-event-analysis-oms.md) con el fin de recopilar métricas, supervisar contenedores implementados en el clúster y visualizar los registros 
