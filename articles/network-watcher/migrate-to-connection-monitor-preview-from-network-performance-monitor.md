---
title: Migración a Connection Monitor (versión preliminar) de Network Performance Monitor
titleSuffix: Azure Network Watcher
description: Aprenda a migrar a Connection Monitor (versión preliminar) de Network Performance Monitor.
services: network-watcher
documentationcenter: na
author: vinynigam
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/20/2020
ms.author: vinigam
ms.openlocfilehash: dcbb82c1315e6150ddcfadbb52b2976447329b87
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "89441840"
---
# <a name="migrate-to-connection-monitor-preview-from-network-performance-monitor"></a>Migración a Connection Monitor (versión preliminar) de Network Performance Monitor

Puede migrar las pruebas de Network Performance Monitor (NPM) a la nueva y mejorada característica Connection Monitor (versión preliminar) con un solo clic y sin tiempo de inactividad. Para más información sobre las ventajas, consulte [Connection Monitor (versión preliminar)](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview).

>[!NOTE]
> Solo las pruebas del Monitor de conectividad de servicio se pueden migrar a Connection Monitor (versión preliminar).
>

## <a name="key-points-to-note"></a>Puntos clave a tener en cuenta

La migración ayuda a generar los siguientes resultados:

* Los agentes locales y la configuración de firewall funcionarán tal cual. No es preciso realizar cambios. Los agentes de Log Analytics instalados en Azure Virtual Machines deben reemplazarse por la extensión Network Watcher.
* Las pruebas existentes se asignan a Connection Monitor (versión preliminar) -> Grupo de prueba -> Test format (Formato de prueba). Al seleccionar **Editar**, puede ver y modificar las propiedades de la nueva característica Connection Monitor (versión preliminar), descargar una plantilla para realizar cambios en ella y enviar la plantilla a través de Azure Resource Manager.
* Los agentes envían datos tanto al área de trabajo de Log Analytics como a las métricas.
* Supervisión de datos:
   * **Datos en Log Analytics**: antes de la migración, los datos permanecen en el área de trabajo en la que se configura NPM en la tabla NetworkMonitoring. Después de la migración, los datos pasan a la tabla NetworkMonitoring y a la tabla ConnectionMonitor_CL en la misma área de trabajo. Una vez deshabilitadas las pruebas en NPM, los datos solo se almacenan en la tabla ConnectionMonitor_CL.
   * **Alertas, paneles e integraciones basados en el registro**: debe editar manualmente las consultas en función de la nueva tabla ConnectionMonitor_CL. Para volver a crear las alertas en las métricas, consulte [Supervisión de la conectividad de red con Connection Monitor (versión preliminar)](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview#metrics-in-azure-monitor).
    
## <a name="prerequisites"></a>Requisitos previos

* Asegúrese de que Network Watcher esté habilitado en la suscripción y la región del área de trabajo de Log Analytics.
* Las máquinas virtuales de Azure con agentes de Log Analytics instalados deberán estar habilitadas con la extensión Network Watcher.

## <a name="migrate-the-tests"></a>Migración de las pruebas

Para migrar las pruebas de Network Performance Monitor a Connection Monitor (versión preliminar), haga lo siguiente:

1. En Network Watcher, seleccione **Connection Monitor** y, luego, la pestaña **Migrate tests from NPM** (Migrar pruebas desde NPM). 

    ![Captura de pantalla que muestra el panel "Migrate tests from NPM" (Migrar pruebas desde NPM) en Network Watcher | Connection Monitor (versión preliminar).](./media/connection-monitor-2-preview/migrate-npm-to-cm-preview.png)
    
1. En las listas desplegables, seleccione la suscripción y el área de trabajo y, luego, seleccione la característica de NPM que quiere migrar. Actualmente, solo se pueden migrar pruebas del Monitor de conectividad de servicio.  
1. Seleccione **Import** (Importar) para migrar las pruebas.

Una vez iniciada la migración, tienen lugar los siguientes cambios: 
* Se crea un recurso de monitor de conexión.
   * Se crea un monitor de conexión por región y suscripción. En el caso de las pruebas con agentes locales, el nuevo nombre del monitor de conexión tiene el formato `<workspaceName>_"on-premises"`. En el caso de las pruebas con agentes de Azure, el nuevo nombre del monitor de conexión tiene el formato `<workspaceName>_<Azure_region_name>`.
   * Los datos de supervisión ahora se almacenan en la misma área de trabajo de Log Analytics donde está habilitado NPM, en una tabla nueva llamada Connectionmonitor_CL. 
   * El nombre de la prueba se mantiene como nombre del grupo de pruebas. No se migra la descripción de la prueba.
   * Se crean puntos de conexión de origen y destino y se usan en el grupo de pruebas creado. En el caso de los agentes locales, el formato de los puntos de conexión es `<workspaceName>_"endpoint"_<FQDN of on-premises machine>`. En el caso de Azure, si las pruebas que se migran contienen agentes que no están en ejecución, debe habilitar los agentes y volver a migrarlas.
   * El puerto de destino y el intervalo de sondeo se trasladan a la configuración de prueba llamada *TC_\<testname>* y *TC_\<testname>_AppThresholds*. El protocolo se establece según los valores de puerto. Los umbrales de éxito y otras propiedades opcionales se dejan en blanco.
* NPM no está deshabilitado, por lo que las pruebas migradas pueden seguir enviando datos a las tablas NetworkMonitoring y ConnectionMonitor_CL. Este enfoque garantiza que las integraciones y alertas existentes basadas en el registro no resulten afectadas.
* El monitor de conexión recién creado estará visible en Connection Monitor (versión preliminar).

Después de la migración:
* Deshabilite manualmente las pruebas en NPM. Hasta que lo haga, se le seguirá cobrando. 
* Al deshabilitar NPM, vuelva a crear las alertas en la tabla ConnectionMonitor_CL o use las métricas. 
* Migre cualquier integración externa a la tabla ConnectionMonitor_CL. Algunos ejemplos de integración externa son los paneles de Power BI y Grafana, y las integraciones con sistemas de Administración de eventos e información de seguridad (SIEM).


## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Connection Monitor (versión preliminar), consulte:
* [Migración de Connection Monitor a Connection Monitor (versión preliminar)](migrate-to-connection-monitor-preview-from-connection-monitor.md)
* [Creación de Connection Monitor (versión preliminar) mediante Azure Portal](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview-create-using-portal)
