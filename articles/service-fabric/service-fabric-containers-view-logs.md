---
title: Visualización de los registros de contenedor en Azure Service Fabric
description: Describe cómo ver los registros de contenedor para servicios de contenedor de Service Fabric en ejecución utilizando Service Fabric Explorer.
ms.topic: conceptual
ms.date: 05/15/2018
ms.openlocfilehash: c47a408b272f95dbfcf3d791c644bfeb52254a72
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75458188"
---
# <a name="view-logs-for-a-service-fabric-container-service"></a>Visualización de registros para un servicio de contenedor de Service Fabric
Azure Service Fabric es un orquestador de contenedores que admite [contenedores Linux y Windows](service-fabric-containers-overview.md).  Este artículo describe cómo ver los registros de contenedor de un servicio de contenedor en ejecución o de un contenedor no alcanzado para así poder realizar diagnósticos y solucionar problemas.

## <a name="access-the-logs-of-a-running-container"></a>Acceso a los registros de un contenedor en ejecución
Registros de contenedor se pueden acceder mediante [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).  En un explorador web, vaya a `http://mycluster.region.cloudapp.azure.com:19080/Explorer` para abrir Service Fabric Explorer desde el punto de conexión de administración del clúster.  

Los registros de contenedor se encuentran en el nodo de clúster en el que se ejecuta la instancia de servicio de contenedor. Como ejemplo, obtenga los registros del contenedor de front-end web de la [aplicación de ejemplo de votación de Linux](service-fabric-quickstart-containers-linux.md). En la vista de árbol, expanda **Cluster**> **Applications (Aplicaciones)** > **VotingType**>**fabric:/Voting/azurevotefront**.  A continuación, expanda la partición (d1aa737e-f22a-e347-be16-eec90be24bc1, en este ejemplo) y compruebe que el contenedor se está ejecutando en el nodo de clúster *_lnxvm_0*.

En la vista de árbol, busque el paquete de código en el nodo  *_lnxvm_0* expandiendo **Nodes (nodos)** > **_lnxvm_0**>**fabric:/Voting**> **azurevotfrontPkg**>**Code Packages (Paquetes de código)** >**code (código)** .  A continuación, seleccione la opción **Registros de contenedor** para que se muestren los registros de contenedor.

![Plataforma de Service Fabric][Image1]

## <a name="access-the-logs-of-a-dead-or-crashed-container"></a>Acceso a los registros de un contenedor no alcanzado o bloqueado
A partir de la versión v6.2, puede obtener los registros de un contenedor no alcanzado o bloqueado con las [API de REST](/rest/api/servicefabric/sfclient-index) o los comandos de la [CLI de Service Fabric (SFCTL)](service-fabric-cli.md).

### <a name="set-container-retention-policy"></a>Establecimiento de la directiva de retención de contenedor
Para ayudar a diagnosticar los errores de inicio del contenedor, Service Fabric (versión 6.1 o posterior) admite la retención de contenedores que finalizaron o que produjeron un error en el inicio. Esta directiva se puede establecer en el archivo **ApplicationManifest.xml** tal y como se muestra en el siguiente fragmento de código:
```xml
 <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" ContainersRetentionCount="2"  RunInteractive="true"> 
 ```

El valor **ContainersRetentionCount** especifica el número de contenedores que se conservarán cuando se produzca un error en ellos. Si se especifica un valor negativo, se conservarán todos los contenedores con errores. Si no se especifica el atributo **ContainersRetentionCount**, no se conservará ningún contenedor. El atributo **ContainersRetentionCount** también admite parámetros de aplicación, por lo que los usuarios pueden especificar valores diferentes para los clústeres de prueba y de producción. Utilice restricciones de colocación para seleccionar un destino para el servicio de contenedor en un nodo concreto al usar estas características, para impedir que este se mueva a otros nodos. Los contenedores que se conserven mediante esta característica deben quitarse manualmente.

El valor **RunInteractive** se corresponde con las [marcas](https://docs.docker.com/engine/reference/commandline/run/#options) `--interactive` y `tty` de Docker. Cuando esta opción se establece en true en el archivo de manifiesto, estas marcas se usan para iniciar el contenedor.  

### <a name="rest"></a>REST
Use la operación [Obtener registros de contenedor implementados en nodos](/rest/api/servicefabric/sfclient-api-getcontainerlogsdeployedonnode) para obtener los registros de un contenedor bloqueado. Especifique el nombre del nodo que se encontraba en ejecución en el contenedor, el nombre de la aplicación, el nombre del manifiesto de servicio y el nombre del paquete de código.  Especifique `&Previous=true`. La respuesta contendrá los registros de contenedor del contenedor no alcanzado de la instancia del paquete de código.

La forma del URI de la solicitud será similar a la siguiente:

```
/Nodes/{nodeName}/$/GetApplications/{applicationId}/$/GetCodePackages/$/ContainerLogs?api-version=6.2&ServiceManifestName={ServiceManifestName}&CodePackageName={CodePackageName}&Previous={Previous}
```

Solicitud de ejemplo:
```
GET http://localhost:19080/Nodes/_Node_0/$/GetApplications/SimpleHttpServerApp/$/GetCodePackages/$/ContainerLogs?api-version=6.2&ServiceManifestName=SimpleHttpServerSvcPkg&CodePackageName=Code&Previous=true  
```

Cuerpo de la respuesta 200:
```json
{   "Content": "Exception encountered: System.Net.Http.HttpRequestException: Response status code does not indicate success: 500 (Internal Server Error).\r\n\tat System.Net.Http.HttpResponseMessage.EnsureSuccessStatusCode()\r\n" } 
```

### <a name="service-fabric-sfctl"></a>Service Fabric (SFCTL)
Use el comando [sfctl service get-container-logs](service-fabric-sfctl-service.md) para capturar los registros de un contenedor bloqueado.  Especifique el nombre del nodo que se encontraba en ejecución en el contenedor, el nombre de la aplicación, el nombre del manifiesto de servicio y el nombre del paquete de código. Especifique la marca `--previous`.  La respuesta contendrá los registros de contenedor del contenedor no alcanzado de la instancia del paquete de código.

```
sfctl service get-container-logs --node-name _Node_0 --application-id SimpleHttpServerApp --service-manifest-name SimpleHttpServerSvcPkg --code-package-name Code –-previous
```
Respuesta:
```json
{   "content": "Exception encountered: System.Net.Http.HttpRequestException: Response status code does not indicate success: 500 (Internal Server Error).\r\n\tat System.Net.Http.HttpResponseMessage.EnsureSuccessStatusCode()\r\n" }
```

## <a name="next-steps"></a>Pasos siguientes
- Trabaje en el [tutorial para la creación de una aplicación contenedora en Linux](service-fabric-tutorial-create-container-images.md).
- Más información acerca de [Service Fabric y los contenedores](service-fabric-containers-overview.md)

[Image1]: media/service-fabric-containers-view-logs/view-container-logs-sfx.png
