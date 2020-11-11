---
title: Obtención de registros para solucionar problemas de los servicios de datos habilitados para Azure Arc
description: Aprenda a obtener archivos de registro de un controlador de datos para solucionar problemas de los servicios de datos habilitados para Azure Arc.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 0c4cff7583f08fe27649cee464fcef802cddd88f
ms.sourcegitcommit: bbd66b477d0c8cb9adf967606a2df97176f6460b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93234057"
---
# <a name="get-logs-to-troubleshoot-azure-arc-enabled-data-services"></a>Obtención de registros para solucionar problemas de los servicios de datos habilitados para Azure Arc

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="prerequisites"></a>Requisitos previos

Antes de continuar, debe disponer de lo siguiente:

* [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]. Para obtener más información, consulte [Instalación de las herramientas de cliente para implementar y administrar los servicios de datos habilitados para Azure Arc](./install-client-tools.md).
* Una cuenta de administrador para iniciar sesión en el controlador de datos habilitado para Azure Arc.

## <a name="get-log-files"></a>Obtención de los archivos de registro

Puede obtener los registros de los servicios de todos los pods o de pods específicos para solucionar problemas. Uno de los modos de conseguirlos es mediante herramientas de Kubernetes estándar, como el comando `kubectl logs`. En este artículo, usará la herramienta [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)], que facilita la obtención de todos los registros a la vez.

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

El comando `azdata arc dc debug copy-logs` proporciona las siguientes opciones para administrar la salida:

* Puede utilizar el parámetro `--target-folder` para que la salida de los archivos de registro llegue a otro directorio.
* Para comprimir los archivos, omita el parámetro `--skip-compress`.
* Si omite `--exclude-dumps`, desencadena e incluye volcados de memoria. No se recomienda este método a menos que el servicio de soporte técnico de Microsoft haya solicitado los volcados de memoria. Para obtener un volcado de memoria, al crear el controlador de datos, su valor `allowDumps` debe establecerse en `true`.
* Filtre el contenido por nombre para recopilar registros de un pod (`--pod`) o un contenedor (`--container`) específicos.
* Puede pasar los parámetros `--resource-kind` y `--resource-name` para filtrar el contenido y recopilar los registros de un recurso personalizado específico. El valor del parámetro `resource-kind` debe ser uno de los nombres de definición de recursos personalizados. Puede recuperar esos nombres mediante el comando `kubectl get customresourcedefinition`.

Con estos parámetros, puede reemplazar los elementos de `<parameters>` del ejemplo siguiente: 

```console
azdata arc dc debug copy-logs --target-folder <desired folder> --exclude-dumps --skip-compress -resource-kind <custom resource definition name> --resource-name <resource name> --namespace <namespace name>
```

Por ejemplo:

```console
#azdata arc dc debug copy-logs --target-folder C:\temp\logs --exclude-dumps --skip-compress --resource-kind postgresql-12 --resource-name pg1 --namespace arc
```

La siguiente jerarquía de carpetas es un ejemplo. Está organizada por nombre de pod, después por contenedor y después por jerarquía de directorios en el contenedor.

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
