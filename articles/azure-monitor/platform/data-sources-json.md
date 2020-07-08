---
title: Recopilación de datos JSON personalizados en Azure Monitor | Microsoft Docs
description: Los orígenes de datos JSON personalizados se pueden recopilar en Azure Monitor mediante el agente de Log Analytics para Linux.  Estos orígenes de datos personalizados pueden ser scripts simples que devuelven JSON, como curl o uno de los más de 300 complementos de FluentD. En este artículo se describe la configuración necesaria para esta recopilación de datos.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/28/2018
ms.openlocfilehash: 407257dbe9fbfa560153d5044263fc4c947cb05c
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86111939"
---
# <a name="collecting-custom-json-data-sources-with-the-log-analytics-agent-for-linux-in-azure-monitor"></a>Recopilación de orígenes de datos JSON personalizados con el agente de Log Analytics para Linux en Azure Monitor
[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

Los orígenes de datos JSON personalizados se pueden recopilar en [Azure Monitor](data-platform.md) mediante el agente de Log Analytics para Linux.  Estos orígenes de datos personalizados pueden ser scripts simples que devuelven JSON, como [curl](https://curl.haxx.se/) o uno de los más de [300 complementos de FluentD](https://www.fluentd.org/plugins/all). En este artículo se describe la configuración necesaria para esta recopilación de datos.


> [!NOTE]
> El agente de Log Analytics para Linux (versión v1.1.0-217 y posteriores) es necesario para los datos JSON personalizados.

## <a name="configuration"></a>Configuración

### <a name="configure-input-plugin"></a>Configuración del complemento de entrada

Para recopilar datos JSON en Azure Monitor, agregue `oms.api.` al principio de una etiqueta de FluentD en un complemento de entrada.

Por ejemplo, el siguiente es un archivo de configuración independiente `exec-json.conf` en `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`.  Este archivo usa el complemento `exec` de FuentD para ejecutar un comando de curl cada 30 segundos.  La salida de este comando se recopila mediante el complemento de salida JSON.

```xml
<source>
  type exec
  command 'curl localhost/json.output'
  format json
  tag oms.api.httpresponse
  run_interval 30s
</source>

<match oms.api.httpresponse>
  type out_oms_api
  log_level info

  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms_api_httpresponse*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
```

Será necesario cambiar la propiedad del archivo de configuración agregado bajo `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/` con el siguiente comando.

`sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/exec-json.conf`

### <a name="configure-output-plugin"></a>Configuración del complemento de salida 
Agregue la siguiente configuración de complemento de salida a la configuración principal en `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` o como un archivo de configuración independiente colocado en `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`.

```xml
<match oms.api.**>
  type out_oms_api
  log_level info

  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms_api*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
```

### <a name="restart-log-analytics-agent-for-linux"></a>Reiniciar el agente de Log Analytics para Linux
Reinicie el servicio del agente de Log Analytics para Linux con el siguiente comando.

```console
sudo /opt/microsoft/omsagent/bin/service_control restart 
```

## <a name="output"></a>Output
Los datos se recopilarán en Azure Monitor con un tipo de registro de `<FLUENTD_TAG>_CL`.

Por ejemplo, la etiqueta personalizada `tag oms.api.tomcat` en Azure Monitor con un tipo de registro de `tomcat_CL`.  Podría recuperar todos los registros de este tipo con la consulta de registros siguiente.

```console
Type=tomcat_CL
```

Se admiten datos JSON anidados, pero se indexan en función del campo principal. Por ejemplo, se devuelven los datos JSON siguientes desde una consulta de registro como `tag_s : "[{ "a":"1", "b":"2" }]`.

```json
{
    "tag": [{
        "a":"1",
        "b":"2"
    }]
}
```


## <a name="next-steps"></a>Pasos siguientes
* Obtenga información acerca de las [consultas de registros](../log-query/log-query-overview.md) para analizar los datos recopilados de soluciones y orígenes de datos. 
