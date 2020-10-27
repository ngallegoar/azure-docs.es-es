---
title: Obtención de registros para solucionar problemas del controlador de datos habilitado para Azure Arc
description: Obtenga registros de servicio para solucionar problemas del controlador de datos habilitado para Azure Arc.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 625092e0557d40051e1ffd538a496c20edc0222f
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92320199"
---
# <a name="get-azure-arc-enabled-data-services-logs"></a>Obtención de registros de los servicios de datos habilitados para Azure Arc

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="prerequisites"></a>Requisitos previos

Antes de continuar, necesitará lo siguiente:

* [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]. [Instrucciones de instalación](./install-client-tools.md).
* Una cuenta de administrador para iniciar sesión en el controlador de servicios de datos habilitados para Azure Arc.

## <a name="get-azure-arc-enabled-data-services-logs"></a>Obtención de registros de los servicios de datos habilitados para Azure Arc

Puede obtener los registros de los servicios de datos habilitados para Azure Arc de todos los pods o de pods específicos para solucionar problemas. Para ello, puede usar herramientas estándar de Kubernetes, como el comando `kubectl logs`. Asimismo, en este artículo, utilizará la herramienta [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)], que facilita la obtención de todos los registros a la vez.

1. Inicie sesión en el controlador de datos con una cuenta de administrador.

   ```console
   azdata login
   ```

2. A continuación, ejecute el comando siguiente para hacer un volcado de los registros:

   ```console
   azdata arc dc debug copy-logs --namespace <namespace name> --exclude-dumps --skip-compress
   ```

   Por ejemplo:

   ```console
   #azdata arc dc debug copy-logs --namespace arc --exclude-dumps --skip-compress
   ```

El controlador de datos crea los archivos de registro en el directorio de trabajo actual en un subdirectorio denominado `logs`. 

## <a name="options"></a>Opciones

`azdata arc dc debug copy-logs` proporciona las siguientes opciones para administrar la salida.

* Puede utilizar el parámetro `--target-folder` para que la salida de los archivos de registro llegue a otro directorio.
* Para comprimir los archivos, omita el parámetro `--skip-compress`.
* Desencadene e incluya volcados de memoria omitiendo `--exclude-dumps`. No se recomienda este método a menos que el Soporte técnico de Microsoft haya solicitado los volcados de memoria. El volcado de memoria requiere que el valor `allowDumps` del controlador de datos esté establecido en `true` cuando se cree el controlador de datos.
* Filtre el contenido por nombre para recopilar registros de un pod (`--pod`) o un contenedor (`--container`) específicos.
* Filtre el contenido para recopilar los registros de un recurso personalizado específico pasando los parámetros `--resource-kind` y `--resource-name`. El valor del parámetro `resource-kind` debe ser uno de los nombres de definición de recursos personalizados que puede recuperar el comando `kubectl get customresourcedefinition`.

Con estos parámetros, puede reemplazar `<parameters>` en el ejemplo siguiente. 

```console
azdata arc dc debug copy-logs --target-folder <desired folder> --exclude-dumps --skip-compress -resource-kind <custom resource definition name> --resource-name <resource name> --namespace <namespace name>
```

Por ejemplo

```console
#azdata arc dc debug copy-logs --target-folder C:\temp\logs --exclude-dumps --skip-compress --resource-kind postgresql-12 --resource-name pg1 --namespace arc
```

Ejemplo de jerarquía de carpetas. Tenga en cuenta que la jerarquía de carpetas está organizada por nombre de pod, después por contenedor y después por jerarquía de directorios en el contenedor.

```output
<export directory>
├───debuglogs-arc-20200827-180403
│   ├───bootstrapper-vl8j2
│   │   └───bootstrapper
│   │       ├───apt
│   │       └───fsck
│   ├───control-j2dm5
│   │   ├───controller
│   │   │   └───controller
│   │   │       ├───2020-08-27
│   │   │       └───2020-08-28
│   │   └───fluentbit
│   │       ├───agent
│   │       ├───fluentbit
│   │       └───supervisor
│   │           └───log
│   ├───controldb-0
│   │   ├───fluentbit
│   │   │   ├───agent
│   │   │   ├───fluentbit
│   │   │   └───supervisor
│   │   │       └───log
│   │   └───mssql-server
│   │       ├───agent
│   │       ├───mssql
│   │       ├───mssql-server
│   │       └───supervisor
│   │           └───log
│   ├───controlwd-ln6j8
│   │   └───controlwatchdog
│   │       └───controlwatchdog
│   ├───logsdb-0
│   │   └───elasticsearch
│   │       ├───agent
│   │       ├───elasticsearch
│   │       ├───provisioner
│   │       └───supervisor
│   │           └───log
│   ├───logsui-7gg2d
│   │   └───kibana
│   │       ├───agent
│   │       ├───apt
│   │       ├───fsck
│   │       ├───kibana
│   │       └───supervisor
│   │           └───log
│   ├───metricsdb-0
│   │   └───influxdb
│   │       ├───agent
│   │       ├───influxdb
│   │       └───supervisor
│   │           └───log
│   ├───metricsdc-2f62t
│   │   └───telegraf
│   │       ├───agent
│   │       ├───apt
│   │       ├───fsck
│   │       ├───supervisor
│   │       │   └───log
│   │       └───telegraf
│   ├───metricsdc-jznd2
│   │   └───telegraf
│   │       ├───agent
│   │       ├───apt
│   │       ├───fsck
│   │       ├───supervisor
│   │       │   └───log
│   │       └───telegraf
│   ├───metricsdc-n5vnx
│   │   └───telegraf
│   │       ├───agent
│   │       ├───apt
│   │       ├───fsck
│   │       ├───supervisor
│   │       │   └───log
│   │       └───telegraf
│   ├───metricsui-h748h
│   │   └───grafana
│   │       ├───agent
│   │       ├───grafana
│   │       └───supervisor
│   │           └───log
│   └───mgmtproxy-r5zxs
│       ├───fluentbit
│       │   ├───agent
│       │   ├───fluentbit
│       │   └───supervisor
│       │       └───log
│       └───service-proxy
│           ├───agent
│           ├───nginx
│           └───supervisor
│               └───log
└───debuglogs-kube-system-20200827-180431
    ├───coredns-8bbb65c89-kklt7
    │   └───coredns
    ├───coredns-8bbb65c89-z2vvr
    │   └───coredns
    ├───coredns-autoscaler-5585bf8c9f-g52nt
    │   └───autoscaler
    ├───kube-proxy-5c9s2
    │   └───kube-proxy
    ├───kube-proxy-h6x56
    │   └───kube-proxy
    ├───kube-proxy-nd2b7
    │   └───kube-proxy
    ├───metrics-server-5f54b8994-vpm5r
    │   └───metrics-server
    └───tunnelfront-db87f4cd8-5xwxv
        ├───tunnel-front
        │   ├───apt
        │   └───journal
        └───tunnel-probe
            ├───apt
            ├───journal
            └───openvpn
```

## <a name="next-steps"></a>Pasos siguientes

[azdata arc dc debug copy-logs](/sql/azdata/reference/reference-azdata-arc-dc-debug#azdata-arc-dc-debug-copy-logs?toc=/azure/azure-arc/data/toc.json&bc=/azure/azure-arc/data/breadcrumb/toc.json)