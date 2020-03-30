---
title: Versión preliminar de implementación de Azure Service Fabric Docker Compose
description: Azure Service Fabric acepta el formato de Docker Compose para facilitar la organización de los contenedores existentes con Service Fabric. Esta compatibilidad se encuentra actualmente en versión preliminar.
ms.topic: conceptual
ms.date: 2/23/2018
ms.openlocfilehash: f84dd0ecb7a4002182c8455bfd86354d794a6f7c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79236696"
---
# <a name="docker-compose-deployment-support-in-azure-service-fabric-preview"></a>Compatibilidad con la implementación de Docker Compose en Azure Service Fabric (versión preliminar)

Docker usa el archivo [docker compose.yml](https://docs.docker.com/compose) para definir aplicaciones de varios contenedores. Para que a los clientes familiarizados con Docker les sea más fácil organizar las aplicaciones de contenedores existentes en Azure Service Fabric, se ha incluido compatibilidad de versión preliminar con la implementación de Docker Compose de forma nativa en la plataforma. Service Fabric puede aceptar la versión 3 y posterior de los archivos `docker-compose.yml`. 

Dado que esta compatibilidad está en versión preliminar, solo se admite un subconjunto de directivas de Compose.

Para usar esta versión preliminar, cree el clúster con la versión 5.7 o superior del entorno en tiempo de ejecución de Service Fabric a través de Azure Portal junto con el SDK correspondiente. 

> [!NOTE]
> Esta característica se encuentra en versión preliminar y no se admite en producción.
> Los ejemplos siguientes se basan en la versión en tiempo de ejecución 6.0 y en la versión del SDK 2.8.

## <a name="deploy-a-docker-compose-file-on-service-fabric"></a>Implementación de un archivo de Docker Compose en Service Fabric

Los siguientes comandos crean una aplicación de Service Fabric (llamada `fabric:/TestContainerApp`), la cual se puede supervisar y administrar como cualquier otra aplicación de Service Fabric. Puede usar el nombre de aplicación especificado para las consultas de mantenimiento.
Service Fabric reconoce "DeploymentName" como identificador de la implementación de Compose.

### <a name="use-powershell"></a>Uso de PowerShell

Cree una implementación Compose de Service Fabric a partir de un archivo docker-compose.yml ejecutando el siguiente comando en PowerShell:

```powershell
New-ServiceFabricComposeDeployment -DeploymentName TestContainerApp -Compose docker-compose.yml [-RegistryUserName <>] [-RegistryPassword <>] [-PasswordEncrypted]
```

`RegistryUserName` y `RegistryPassword` hacen referencia al nombre de usuario y la contraseña del registro de contenedor. Después de haber completado la implementación, puede comprobar su estado mediante el siguiente comando:

```powershell
Get-ServiceFabricComposeDeploymentStatus -DeploymentName TestContainerApp
```

Para eliminar la implementar Compose mediante PowerShell, use el siguiente comando:

```powershell
Remove-ServiceFabricComposeDeployment  -DeploymentName TestContainerApp
```

Para iniciar una actualización de implementación de Compose mediante PowerShell, use el siguiente comando:

```powershell
Start-ServiceFabricComposeDeploymentUpgrade -DeploymentName TestContainerApp -Compose docker-compose-v2.yml -Monitored -FailureAction Rollback
```

Para revertir una actualización de implementación de Compose mediante PowerShell, use el siguiente comando:

```powershell
Start-ServiceFabricComposeDeploymentRollback -DeploymentName TestContainerApp
```

Después de que se acepta la actualización, puede realizarse el seguimiento de su progreso mediante el comando siguiente:

```powershell
Get-ServiceFabricComposeDeploymentUpgrade -DeploymentName TestContainerApp
```

### <a name="use-azure-service-fabric-cli-sfctl"></a>Uso de la CLI de Azure Service Fabric (sfctl)

También puede ejecutar el siguiente comando de la CLI de Service Fabric:

```shell
sfctl compose create --deployment-name TestContainerApp --file-path docker-compose.yml [ [ --user --encrypted-pass ] | [ --user --has-pass ] ] [ --timeout ]
```

Después de haber creado la implementación, puede comprobar su estado mediante el siguiente comando:

```shell
sfctl compose status --deployment-name TestContainerApp [ --timeout ]
```

Para eliminar la implementación de Compose, use el siguiente comando:

```shell
sfctl compose remove  --deployment-name TestContainerApp [ --timeout ]
```

Para iniciar una actualización de la implementación de Compose, use el comando siguiente:

```shell
sfctl compose upgrade --deployment-name TestContainerApp --file-path docker-compose-v2.yml [ [ --user --encrypted-pass ] | [ --user --has-pass ] ] [--upgrade-mode Monitored] [--failure-action Rollback] [ --timeout ]
```

Para revertir la actualización de la implementación de Compose, use el comando siguiente:

```shell
sfctl compose upgrade-rollback --deployment-name TestContainerApp [ --timeout ]
```

Después de que se acepta la actualización, puede realizarse el seguimiento de su progreso mediante el comando siguiente:

```shell
sfctl compose upgrade-status --deployment-name TestContainerApp
```

## <a name="supported-compose-directives"></a>Directivas de Compose admitidas

Esta versión preliminar admite un subconjunto de las opciones de configuración en el formato de la versión 3 de Compose, lo que incluye los siguientes primitivos:

* Services > Deploy > Replicas
* Services > Deploy > Placement > Constraints
* Services > Deploy > Resources > Limits
    * -cpu-shares
    * -memory
    * -memory-swap
* Services > Commands
* Services > Environment
* Services > Ports
* Services > Image
* Services > Isolation (solo para Windows)
* Services > Logging > Driver
* Services > Logging > Driver > Options
* Volumen e implementación > Volumen

Configure el clúster para exigir límites de recursos, como se describe en [Gobernanza de recursos de Service Fabric](service-fabric-resource-governance.md). El resto de directivas de Docker Compose no se admiten en esta versión preliminar.

### <a name="ports-section"></a>Sección de puertos

Especifique el protocolo HTTP o HTTPS en la sección de puertos que usará el agente de escucha del servicio de Service Fabric. Así se asegurará de que el protocolo de punto de conexión se publica correctamente con el servicio de nombres para permitir que el proxy inverso reenvíe las solicitudes:
* Para enrutar a los servicios Compose de Service Fabric no seguros, especifique **/http**. Por ejemplo: **"80:80/http"** .
* Para enrutar a los servicios Compose de Service Fabric seguros, especifique **/https**. Por ejemplo: **"443:443/https"** .

> [!NOTE]
> La sintaxis de la sección de puertos /http y /https es específica de Service Fabric a fin de registrar la dirección URL correcta para el agente de escucha de Service Fabric.  Se puede producir un error de validación si la sintaxis del archivo de Docker Compose se valida mediante programación.

## <a name="servicednsname-computation"></a>Cálculo de ServiceDnsName

Si el nombre de servicio especificado en un archivo de Compose es un nombre de dominio completo (es decir, contiene un punto, [.]), el nombre DNS registrado por Service Fabric será `<ServiceName>` (incluido el punto). En caso contrario, cada segmento de la ruta del nombre de aplicación se convierte en una etiqueta de dominio del nombre DNS del servicio, siendo el primer segmento de la ruta el que se convierte en la etiqueta de dominio de nivel superior.

Por ejemplo, si el nombre de aplicación especificado es `fabric:/SampleApp/MyComposeApp`, `<ServiceName>.MyComposeApp.SampleApp` sería el nombre DNS registrado.

## <a name="compose-deployment-instance-definition-versus-service-fabric-app-model-type-definition"></a>Implementación de Compose (definición de instancia) frente al modelo de aplicación de Service Fabric (definición de tipo)

Un archivo docker-compose.yml describe un conjunto implementable de contenedores que incluye sus propiedades y configuraciones.
Por ejemplo, el archivo puede contener las variables de entorno y los puertos. También puede especificar parámetros de implementación como, por ejemplo, las restricciones de colocación, los límites de recursos y los nombres DNS, en el archivo docker-compose.yml.

El [modelo de aplicación de Service Fabric](service-fabric-application-model.md) usa tipos de servicio y tipos de aplicación, en los que puede tener muchas instancias de aplicaciones del mismo tipo. Por ejemplo, puede tener una instancia de aplicación por cliente. Este modelo basado en tipos admite varias versiones del mismo tipo de aplicación registrado con el tiempo de ejecución.

Por ejemplo, un cliente A puede tener una aplicación con una instancia creada con el tipo 1.0 de AppTypeA y un cliente B puede tener otra aplicación con una instancia creada con el mismo tipo y versión. Los tipos de aplicación se definen en los manifiestos de aplicación, y se puede especificar el nombre de la aplicación y los parámetros de implementación al crear la aplicación.

Aunque este modelo ofrece flexibilidad, está prevista la compatibilidad con un modelo más sencillo de implementación basado en instancias donde los tipos son implícitos en el archivo de manifiesto. En este modelo, cada aplicación obtiene su propio manifiesto independiente. Esta iniciativa se va a trasladar a la versión preliminar agregando compatibilidad con el archivo docker-compose.yml, que es un formato de implementación basado en instancias.

## <a name="next-steps"></a>Pasos siguientes

* Descripción del [modelo de aplicación de Service Fabric](service-fabric-application-model.md)
* [Introducción a la CLI de Service Fabric](service-fabric-cli.md)
