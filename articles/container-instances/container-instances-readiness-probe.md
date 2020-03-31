---
title: Configuración del sondeo de preparación en la instancia de contenedor
description: Aprenda a configurar un sondeo para asegurarse de que los contenedores de Azure Container Instances reciben solicitudes solo cuando están listos.
ms.topic: article
ms.date: 01/30/2020
ms.openlocfilehash: 64bb4a3e429ce820835abbf8e235600e592f7868
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76935678"
---
# <a name="configure-readiness-probes"></a>Configuración de sondeos de preparación

En el caso de las aplicaciones en contenedor que atienden el tráfico, puede que le interese comprobar que el contenedor está listo para administrar las solicitudes entrantes. Azure Container Instances admite sondeos de preparación para incluir configuraciones de modo que no se pueda acceder al contenedor bajo ciertas condiciones. El sondeo de preparación funciona como un [sondeo de preparación de Kubernetes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/). Por ejemplo, puede que una aplicación contenedora tenga que cargar un conjunto de datos grande durante el inicio y no es deseable que se reciban solicitudes durante este tiempo.

En este artículo se explica cómo implementar un grupo de contenedores que incluya un sondeo de preparación, de tal forma que un contenedor solo reciba tráfico cuando el sondeo se realice correctamente.

Azure Container Instances también admite [sondeos de ejecución](container-instances-liveness-probe.md), que se pueden configurar para hacer que un contenedor en mal estado se reinicie automáticamente.

> [!NOTE]
> Actualmente no se puede usar un sondeo de preparación en un grupo de contenedores que se implementa en una red virtual.

## <a name="yaml-configuration"></a>Configuración de YAML

Por ejemplo, cree un archivo `readiness-probe.yaml` con el siguiente fragmento de código que incluya un sondeo de preparación. Este archivo define un grupo de contenedores que consta de un contenedor que ejecuta una aplicación web pequeña. La aplicación se implementa desde la imagen `mcr.microsoft.com/azuredocs/aci-helloworld` pública. Esta aplicación en contenedor también se muestra en [Implementación de una instancia de contenedor en Azure mediante la CLI de Azure](container-instances-quickstart.md) y otros inicios rápidos.

```yaml
apiVersion: 2018-10-01
location: eastus
name: readinesstest
properties:
  containers:
  - name: mycontainer
    properties:
      image: mcr.microsoft.com/azuredocs/aci-helloworld
      command:
        - "/bin/sh"
        - "-c"
        - "node /usr/src/app/index.js & (sleep 240; touch /tmp/ready); wait"
      ports:
      - port: 80
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      readinessProbe:
        exec:
          command:
          - "cat"
          - "/tmp/ready"
        periodSeconds: 5
  osType: Linux
  restartPolicy: Always
  ipAddress:
    type: Public
    ports:
    - protocol: tcp
      port: '80'
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

### <a name="start-command"></a>Comando de inicio

La implementación incluye una propiedad `command` que define un comando de inicio que se ejecuta cuando el contenedor comienza a ejecutarse por primera vez. Esta propiedad acepta una matriz de cadenas. Este comando simula una hora en la que se ejecuta la aplicación web, pero el contenedor no está listo. 

En primer lugar, inicia una sesión de shell y ejecuta un comando `node` para iniciar la aplicación web. También inicia un comando de suspensión durante 240 segundos, tras lo cual crea un archivo llamado `ready` en el directorio `/tmp`:

```console
node /usr/src/app/index.js & (sleep 240; touch /tmp/ready); wait
```

### <a name="readiness-command"></a>Comando Readiness

Este archivo YAML define un elemento `readinessProbe` que admite un comando de preparación `exec` que actúa como comprobación de preparación. Este comando de preparación de ejemplo comprueba la existencia del archivo `ready` en el directorio `/tmp`.

Cuando el archivo `ready` no existe, el comando de preparación se cierra con un valor distinto de cero; el contenedor continúa ejecutándose, pero no se puede tener acceso a él. Cuando el comando se cierra correctamente con un código de salida de 0, el contenedor está listo para su acceso. 

La propiedad `periodSeconds` designa que el comando de preparación debe ejecutarse cada 5 segundos. El sondeo de preparación se ejecuta durante la vigencia del grupo de contenedores.

## <a name="example-deployment"></a>Implementación de ejemplo

Ejecute el siguiente comando para implementar un grupo de contenedores con la configuración de YAML anterior:

```azurecli-interactive
az container create --resource-group myResourceGroup --file readiness-probe.yaml
```

## <a name="view-readiness-checks"></a>Visualización de las comprobaciones de preparación

En este ejemplo, durante los primeros 240 segundos, el comando de preparación produce un error cuando comprueba la existencia del archivo de `ready`. El código de estado devolvió señales de que el contenedor no está listo.

Estos eventos se pueden ver desde Azure Portal o la CLI de Azure. Por ejemplo, el portal muestra que se desencadenan eventos de tipo `Unhealthy` cuando se produce un error en el comando de preparación. 

![Evento incorrecto del portal:][portal-unhealthy]

## <a name="verify-container-readiness"></a>Comprobación de la preparación del contenedor

Después de iniciar el contenedor, puede comprobar que inicialmente no es accesible. Tras el aprovisionamiento, obtenga la dirección IP del grupo de contenedores:

```azurecli
az container show --resource-group myResourceGroup --name readinesstest --query "ipAddress.ip" --out tsv
```

Intente acceder al sitio mientras se produce un error en el sondeo de preparación:

```bash
wget <ipAddress>
```

La salida muestra que el sitio no es accesible inicialmente:
```
$ wget 192.0.2.1
--2019-10-15 16:46:02--  http://192.0.2.1/
Connecting to 192.0.2.1... connected.
HTTP request sent, awaiting response... 
```

Después de 240 segundos, el comando de preparación se ejecuta correctamente, lo que indica que el contenedor está listo. Ahora, al ejecutar el comando `wget`, la operación se realiza correctamente:

```
$ wget 192.0.2.1
--2019-10-15 16:46:02--  http://192.0.2.1/
Connecting to 192.0.2.1... connected.
HTTP request sent, awaiting response...200 OK
Length: 1663 (1.6K) [text/html]
Saving to: ‘index.html.1’

index.html.1                       100%[===============================================================>]   1.62K  --.-KB/s    in 0s      

2019-10-15 16:49:38 (113 MB/s) - ‘index.html.1’ saved [1663/1663] 
```

Cuando el contenedor está listo, también puede acceder a la aplicación web mediante un explorador web.

> [!NOTE]
> El sondeo de preparación continúa ejecutándose mientras dure el grupo de contenedores. Si se produce un error en el comando de preparación en un momento posterior, el contenedor vuelve a dejar de estar accesible. 
> 

## <a name="next-steps"></a>Pasos siguientes

Un sondeo de preparación puede ser útil en escenarios donde intervienen grupos de varios contenedores que se componen de contenedores dependientes. Para más información sobre los escenarios de varios contenedores, consulte [Grupos de contenedores en Azure Container Instances](container-instances-container-groups.md).

<!-- IMAGES -->
[portal-unhealthy]: ./media/container-instances-readiness-probe/readiness-probe-failed.png
