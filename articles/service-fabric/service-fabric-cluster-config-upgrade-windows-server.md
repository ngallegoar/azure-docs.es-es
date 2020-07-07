---
title: Actualización de la configuración de un clúster independiente
description: Obtenga información acerca de cómo actualizar la configuración que ejecuta un clúster de Service Fabric independiente.
author: dkkapur
ms.topic: conceptual
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: 8d0279cc323f7eee87feb2a596a4c2df0b4667e1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "82790854"
---
# <a name="upgrade-the-configuration-of-a-standalone-cluster"></a>Actualización de la configuración de un clúster independiente 

En cualquier sistema moderno, la capacidad de actualización es clave para el éxito a largo plazo del producto. Un clúster de Azure Service Fabric es un recurso que usted posee. En este artículo se describe cómo actualizar las opciones de configuración de un clúster de Service Fabric independiente.

## <a name="customize-cluster-settings-in-the-clusterconfigjson-file"></a>Personalización de la configuración del clúster en el archivo ClusterConfig.json
Los clústeres independientes se configuran mediante el archivo *ClusterConfig.json*. Para obtener más información acerca de las distintas opciones de configuración, vea [Opciones de configuración de un clúster de Windows independiente](service-fabric-cluster-manifest.md).

Puede agregar, actualizar o eliminar la configuración en la sección `fabricSettings` en la sección [Propiedades del clúster](./service-fabric-cluster-manifest.md#cluster-properties) de *ClusterConfig.json*. 

Por ejemplo, el siguiente código JSON agrega una nueva configuración *MaxDiskQuotaInMB* a la sección *Diagnósticos* de `fabricSettings`:

```json
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

Cuando haya modificado la configuración del archivo ClusterConfig.json, [pruebe la configuración del clúster](#test-the-cluster-configuration) y, después, [actualice la configuración del clúster](#upgrade-the-cluster-configuration) para aplicar la configuración al clúster. 

## <a name="test-the-cluster-configuration"></a>Prueba de la configuración del clúster
Antes de iniciar la actualización de la configuración, puede probar el nuevo archivo JSON de configuración del clúster mediante la ejecución del siguiente script de PowerShell en el paquete independiente:

```powershell
TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File>
```

O bien, puede utilizar este script:

```powershell
TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File> -FabricRuntimePackagePath <Path to the .cab file which you want to test the configuration against>
```

Algunas configuraciones no se pueden actualizar, como los puntos de conexión, el nombre del clúster, la dirección IP del nodo, etc. El nuevo archivo JSON de configuración del clúster se prueba en el antiguo y generará errores en la ventana de PowerShell si hay cualquier problema.

## <a name="upgrade-the-cluster-configuration"></a>Actualizar la configuración del clúster
Para actualizar la configuración del clúster, ejecute [Start-ServiceFabricClusterConfigurationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade). El dominio de actualización procesa la actualización de la configuración.

```powershell
Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
```

## <a name="upgrade-cluster-certificate-configuration"></a>Actualización de la configuración de un certificado de clúster
Se utiliza un certificado de clúster para la autenticación entre nodos de clúster. La sustitución del certificado debe realizarse con precaución adicional ya que en caso de error se bloqueará la comunicación entre los nodos del clúster.

Se admiten cuatro opciones:  

* Actualización del certificado único: La ruta de actualización es Certificado A (principal) -> Certificado B (principal) -> Certificado C (principal) ->...

* Actualización de dos certificados: la ruta de actualización es Certificado A (principal) -> Certificado A (principal) y B (secundario) -> Certificado B (principal) -> Certificado B (principal) y C (secundario) -> Certificado C (principal) ->...

* Actualización del tipo de certificado: configuración de certificado basada en huella digital <-> Configuración de certificado basada en CommonName. Por ejemplo, Huella digital del certificado A (principal) y Huella digital B (secundaria) -> Certificado CommonName C.

* Actualización de la huella digital del emisor de certificado: la ruta de actualización de software es Certificado CN=A,Emisor huella digital=IT1 (principal) -> Certificado CN=A,Emisor huella digital=IT1,IT2 (principal) -> Certificado CN=A,Emisor huella digital=IT2 (principal)".


## <a name="next-steps"></a>Pasos siguientes
* Obtenga información sobre cómo personalizar la [configuración de clúster de Service Fabric](service-fabric-cluster-fabric-settings.md).
* Obtenga información sobre cómo [escalar o reducir horizontalmente el clúster](service-fabric-cluster-scale-in-out.md).
* Obtenga información sobre [actualizaciones de aplicaciones](service-fabric-application-upgrade.md).

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
