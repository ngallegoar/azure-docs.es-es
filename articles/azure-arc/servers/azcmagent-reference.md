---
title: Interfaz de la CLI del agente de máquinas conectadas a Azure
description: Documentación de referencia de la CLI del agente de máquinas conectadas a Azure
author: bobbytreed
manager: carmonm
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
ms.topic: reference
ms.date: 11/04/2019
ms.author: robreed
ms.openlocfilehash: d35c5e283f2e1e2f8afd431d83775167dc2a531a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73510397"
---
# <a name="azure-connected-machine-agent-cli-interface"></a>Interfaz de la CLI del agente de máquinas conectadas a Azure

La herramienta `Azcmagent` (agente de máquinas conectadas a Azure) se usa para configurar y solucionar problemas de conexión con Azure en máquinas que no son de Azure.

El propio agente es un proceso de demonio llamado `himdsd` en Linux y un servicio de Windows llamado `himds` en Windows.

Por lo general, `azcmagent connect` se usa para establecer una conexión entre esta máquina y Azure, y `azcmagent disconnect` si decide que ya no desea esa conexión. Los otros comandos son para la solución de problemas u otros casos especiales.

## <a name="options"></a>Opciones

```none
  -h, --help      help for azcmagent
  -v, --verbose   Increase logging verbosity to show all logs
```

## <a name="see-also"></a>CONSULTE TAMBIÉN

* [azcmagent connect](#azcmagent-connect): conecta esta máquina a Azure
* [azcmagent disconnect](#azcmagent-disconnect): desconecta esta máquina de Azure
* [azcmagent reconnect](#azcmagent-reconnect): vuelve a conectar esta máquina a Azure
* [azcmagent show](#azcmagent-show): obtiene los metadatos de la máquina y el estado del agente. Esta opción se emplea principalmente para la solución de problemas.
* [azcmagent version](#azcmagent-version): muestra la versión híbrida del agente de administración

## <a name="azcmagent-connect"></a>azcmagent connect

Conecta esta máquina a Azure

### <a name="synopsis"></a>Sinopsis

Crea un recurso en Azure que representa a esta máquina.

Usa las opciones de autenticación proporcionadas para crear un recurso en Azure Resource Manager que representa a esta máquina. El recurso está en la suscripción y en el grupo de recursos solicitado, y los datos sobre la máquina se almacenan en la región de Azure especificada por el parámetro de ubicación.
El nombre predeterminado del recurso es el nombre de host de esta máquina si no se reemplaza.

Un certificado correspondiente a la identidad asignada por el sistema de esta máquina se descarga y se almacena localmente. Una vez que este paso complete el servicio y la configuración de invitado de los **metadatos de máquinas conectadas a Azure**, el agente comenzará a sincronizarse con la nube de Azure.

Opciones de autenticación:

* Token de acceso `azcmagent connect --access-token <> --subscription-id <> --resource-group <> --location <>`
* Id. de entidad de servicio y secreto `azcmagent connect --service-principal-id <> --service-principal-secret <> --tenant-id <tenantid> --subscription-id <> --resource-group <> --location <>`
* Inicio de sesión del dispositivo (interactivo) `azcmagent connect --tenant-id <> --subscription-id <> --resource-group <> --location <>`

### <a name="syntax"></a>Sintaxis

```none
azcmagent connect [flags]
```

### <a name="options"></a>Opciones

```none
      --access-token string               Access token
  -h, --help                              help for connect
  -l, --location string                   Location of the resource [Required]
      --physical-location string          Physical location of the resource
  -g, --resource-group string             Name of the resource group. [Required]
  -n, --resource-name string              Name of the resource. Defaults to Host Name
  -i, --service-principal-id string       Service Principal Id
  -p, --service-principal-secret string   Service Principal Secret
  -s, --subscription-id string            Subscription Id [Required]
  -t, --tags string                       Tags for resource
      --tenant-id string                  Tenant Id
```

## <a name="azcmagent-disconnect"></a>azcmagent disconnect

Desconecta esta máquina de Azure

### <a name="synopsis"></a>Sinopsis

Elimina el recurso de Azure que representa este servidor.

Este comando usa las opciones de autenticación proporcionadas para quitar el recurso de Azure Resource Manager que representa a esta máquina. Después de este punto, se desconectará el **servicio de metadatos de máquinas conectadas a Azure** y el agente de configuración de invitado. Este comando no detiene ni quita los servicios: quite el paquete para poder hacerlo.

Este comando requiere privilegios más altos que el rol "incorporación de máquinas conectadas a Azure".

Una vez desconectada una máquina, use `azcmagent connect`, no `azcmagent reconnect` si desea crear un nuevo recurso para ella en Azure.

Opciones de autenticación:

* Token de acceso `azcmagent disconnect --access-token <>`
* Id. de entidad de servicio y secreto `azcmagent disconnect --service-principal-id <> --service-principal-secret <> --tenant-id <tenantid>`
* Inicio de sesión interactivo del dispositivo `azcmagent disconnect --tenant-id <>`

### <a name="syntax"></a>Sintaxis

```none
azcmagent disconnect [flags]
```

### <a name="options"></a>Opciones

```none
      --access-token string               Access token
  -h, --help                              help for disconnect
  -r, --resource-group string             Name of the resource group
  -n, --resource-name string              Name of the resource
  -i, --service-principal-id string       Service Principal Id
  -p, --service-principal-secret string   Service Principal Secret
  -s, --subscription-id string            Subscription Id
  -t, --tenant-id string                  Tenant Id
```

## <a name="azcmagent-reconnect"></a>azcmagent reconnect

Vuelve a conectar esta máquina a Azure

### <a name="synopsis"></a>Sinopsis

Vuelva a conectar la máquina con credenciales no válidas a Azure.

Si una máquina ya tiene un recurso en Azure, pero no puede autenticarse en ella, se puede volver a conectar con este comando. Esto es posible si una máquina se desactivó el tiempo suficiente para que su certificado expire (al menos 45 días).

Si una máquina se desconectó con `azcmagent disconnect`, use `azcmagent connect` en su lugar.

Este comando usa las opciones de autenticación proporcionadas para recuperar nuevas credenciales correspondientes al recurso de Azure Resource Manager que representa a esta máquina.

Este comando requiere privilegios más altos que el rol **incorporación de máquinas conectadas a Azure**.

Opciones de autenticación

* Token de acceso `azcmagent reconnect --access-token <>`
* Id. de entidad de servicio y secreto `azcmagent reconnect --service-principal-id <> --service-principal-secret <> --tenant-id <tenantid>`
* Inicio de sesión interactivo del dispositivo `azcmagent reconnect --tenant-id <>`

### <a name="syntax"></a>Sintaxis

```none
azcmagent reconnect [flags]
```

### <a name="options"></a>Opciones

```none
      --access-token string               Access token
  -h, --help                              help for reconnect
  -l, --location string                   Location of the resource
  -g, --resource-group string             Name of the resource group.
  -n, --resource-name string              Name of the resource. Defaults to Host Name
  -i, --service-principal-id string       Service Principal Id
  -p, --service-principal-secret string   Service Principal Secret
  -s, --subscription-id string            Subscription Id
      --tenant-id string                  tenant id
```

## <a name="azcmagent-show"></a>azcmagent show

Obtiene los metadatos de la máquina y el estado del agente. Esta opción se emplea principalmente para la solución de problemas.

### <a name="synopsis"></a>Sinopsis

Obtiene los metadatos de la máquina y el estado del agente. Esta opción se emplea principalmente para la solución de problemas.


### <a name="syntax"></a>Sintaxis

```
azcmagent show [flags]
```

### <a name="options"></a>Opciones

```
  -h, --help   help for show
```

## <a name="azcmagent-version"></a>azcmagent version

Muestra la versión híbrida del agente de administración

### <a name="synopsis"></a>Sinopsis

Muestra la versión híbrida del agente de administración

### <a name="syntax"></a>Sintaxis

```none
azcmagent version [flags]
```

### <a name="options"></a>Opciones

```none
  -h, --help   help for version
```
