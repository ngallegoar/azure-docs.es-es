---
title: Recopilación de datos de CollectD en Azure Monitor | Microsoft Docs
description: CollectD es un demonio de Linux de código abierto que recopila periódicamente datos de aplicaciones e información de nivel de sistema.  En este artículo se proporciona información sobre la recopilación de datos de CollectD en Azure Monitor.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/27/2018
ms.openlocfilehash: 488f273336da05738609333f911fe3a90ba59496
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86111990"
---
# <a name="collect-data-from-collectd-on-linux-agents-in-azure-monitor"></a>Recopilación de datos de CollectD en agentes de Linux en Azure Monitor
[CollectD](https://collectd.org/) es un demonio de Linux de código abierto que recopila periódicamente métricas de rendimiento de aplicaciones e información de nivel de sistema. Las aplicaciones de ejemplo incluyen la máquina virtual Java (JVM), MySQL Server y Nginx. En este artículo se proporciona información sobre la recopilación de datos de rendimiento de CollectD en Azure Monitor.

Puede encontrar una lista completa de los complementos disponibles en la [tabla de complementos](https://collectd.org/wiki/index.php/Table_of_Plugins).

![Introducción a CollectD](media/data-sources-collectd/overview.png)

La siguiente configuración de CollectD se incluye en el agente de Log Analytics para Linux con el fin de enrutar los datos de CollectD al agente de Log Analytics para Linux.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

```xml
LoadPlugin write_http

<Plugin write_http>
   <Node "oms">
      URL "127.0.0.1:26000/oms.collectd"
      Format "JSON"
      StoreRates true
   </Node>
</Plugin>
```

Además, si va a usar una versión de CollectD anterior a la 5.5, utilice en su lugar la siguiente configuración.

```xml
LoadPlugin write_http

<Plugin write_http>
   <URL "127.0.0.1:26000/oms.collectd">
      Format "JSON"
      StoreRates true
   </URL>
</Plugin>
```

La configuración de CollectD usa el complemento`write_http` predeterminado para enviar datos de métricas de rendimiento al agente de Log Analytics para Linux mediante el puerto 26000. 

> [!NOTE]
> Este puerto se puede configurar como un puerto definido de forma personalizada, en caso necesario.

El agente de Log Analytics para Linux también escucha en el puerto 26000 las métricas de CollectD y, después, las convierte a métricas de esquema de Azure Monitor. Esta es la configuración del agente de Log Analytics para Linux: `collectd.conf`.

```xml
<source>
   type http
   port 26000
   bind 127.0.0.1
</source>

<filter oms.collectd>
   type filter_collectd
</filter>
```

> [!NOTE]
> CollectD se configura de forma predeterminada para leer valores a un [intervalo](https://collectd.org/wiki/index.php/Interval) de 10 segundos. Como esto afecta directamente al volumen de datos que se envían a los registros de Azure Monitor, puede que tenga que ajustar este intervalo dentro de la configuración de CollectD para lograr un buen equilibrio entre los requisitos de supervisión y los costos y el uso asociados de los registros de Azure Monitor.

## <a name="versions-supported"></a>Versiones admitidas
- En la actualidad, Azure Monitor admite CollectD 4.8 y versiones superiores.
- Para la recopilación de métricas de CollectD, se necesita el agente de Log Analytics para Linux versión 1.1.0-217 o posteriores.


## <a name="configuration"></a>Configuración
Estos son los pasos básicos para configurar la recopilación de datos de CollectD en Azure Monitor.

1. Configure CollectD para enviar datos al agente de Log Analytics para Linux mediante el complemento write_http.  
2. Configure el agente de Log Analytics para Linux de forma que escuche datos de CollectD en el puerto adecuado.
3. Reinicie CollectD y el agente de Log Analytics para Linux.

### <a name="configure-collectd-to-forward-data"></a>Configuración de CollectD para reenviar datos 

1. Para enrutar los datos de CollectD al agente de Log Analytics para Linux, es necesario agregar `oms.conf` al directorio de configuración de CollectD. El destino de este archivo depende de la distribución de Linux de su máquina.

    Si su directorio de configuración de CollectD está ubicado en /etc/collectd.d/:

    ```console
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd.d/oms.conf
    ```

    Si su directorio de configuración de CollectD está ubicado en /etc/collectd/collectd.conf.d/:

    ```console
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd/collectd.conf.d/oms.conf
    ```

    >[!NOTE]
    >Para las versiones de CollectD anteriores a la 5.5, deberá modificar las etiquetas de `oms.conf`, como se mostró anteriormente.
    >

2. Copie collectd.conf en el directorio de configuración deseado de omsagent del área de trabajo.

    ```console
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/collectd.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/
    sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/collectd.conf
    ```

3. Reinicie CollectD y el agente de Log Analytics para Linux con los comandos siguientes.

    ```console
    sudo service collectd restart
    sudo /opt/microsoft/omsagent/bin/service_control restart
    ```

## <a name="collectd-metrics-to-azure-monitor-schema-conversion"></a>Conversión de métricas de CollectD en esquemas de Azure Monitor
Para mantener un modelo conocido entre las métricas de infraestructura ya recopiladas por el agente de Log Analytics para Linux y las nuevas métricas recopiladas por CollectD, se usa la siguiente asignación de esquemas:

| Campo de métrica de CollectD | Campo de Azure Monitor |
|:--|:--|
| `host` | Computer |
| `plugin` | None |
| `plugin_instance` | Nombre de instancia<br>Si **plugin_instance** es *null*, entonces InstanceName =" *_Total*". |
| `type` | ObjectName |
| `type_instance` | CounterName<br>Si **type_instance** es *null*, entonces CounterName=**en blanco**. |
| `dsnames[]` | CounterName |
| `dstypes` | None |
| `values[]` | CounterValue |

## <a name="next-steps"></a>Pasos siguientes
* Obtenga información acerca de las [consultas de registros](../log-query/log-query-overview.md) para analizar los datos recopilados de soluciones y orígenes de datos. 
* Use [Campos personalizados](custom-fields.md) para analizar datos de registros de Syslog en campos individuales.
