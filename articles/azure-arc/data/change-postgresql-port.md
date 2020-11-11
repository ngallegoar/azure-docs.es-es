---
title: Cambio del puerto de PostgreSQL
description: Cambie el puerto donde escucha el grupo de servidores de Hiperescala de PostgreSQL habilitado para Azure Arc.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 11/02/2020
ms.topic: how-to
ms.openlocfilehash: 45424408c790e4921be48464818c55fe74313fd3
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93328660"
---
# <a name="change-the-port-on-which-the-server-group-is-listening"></a>Cambio del puerto donde escucha el grupo de servidores 

Cambiar el puerto es una operación de edición estándar del grupo de servidores. Para cambiar el puerto, ejecute el comando siguiente:
```console
 azdata arc postgres server edit -n <server group name> --port <desired port number>
```

Por ejemplo, supongamos que el nombre del grupo de servidores es _postgres01_ , y quiere que escuche en el puerto _866_. Ejecutaría el comando siguiente:
```console
 azdata arc postgres server edit -n postgres01 --port 866
```

## <a name="verify-that-the-port-was-changed"></a>Comprobación del que el puerto ha cambiado

Para comprobar que el puerto ha cambiado, ejecute el siguiente comando para mostrar la configuración del grupo de servidores:
```console
azdata arc postgres server show -n <server group name>
```

En el resultado de ese comando, examine el número de puerto que se muestra para el elemento "port" en la sección "service" de las especificaciones del grupo de servidores.
Como alternativa, en el elemento externalEndpoint de la sección "status" de las especificaciones del grupo de servidores puede comprobar que la dirección IP esté seguida del número de puerto que ha configurado.

Como ilustración, si continuamos con el ejemplo anterior, ejecutaría el comando:
```console
azdata arc postgres server show -n postgres01
```

y vería el puerto 866 al que se hace referencia aquí:

```console
"service": {
      "port": 866,
      "type": "LoadBalancer"
    },
```
y aquí.

```console
"status": {
    "externalEndpoint": "12.678.345.09:866",
    "logSearchDashboard": "https://12.345.678.90:30777/kibana/app/kibana#/discover?_a=(query:(language:kuery,query:'custom_resource_name:postgres01'))",
    "metricsDashboard": "https://12.345.678.90:30777/grafana/d/postgres-metrics?var-Namespace=arc&var-Name=postgres01",
    "readyPods": "3/3",
    "state": "Ready"
  }
```
## <a name="next-steps"></a>Pasos siguientes
- Lea acerca de [cómo conectarse a su grupo de servidores](get-connection-endpoints-and-connection-strings-postgres-hyperscale.md).
- Obtenga información sobre cómo puede configurar otros aspectos del grupo de servidores en la sección Instrucciones\Administrar\Configuración y escalado de la documentación.
