---
title: Actualización de un clúster de Azure Service Fabric independiente
description: Obtenga información acerca de cómo actualizar la versión o configuración de un clúster de Azure Service Fabric independiente.  T
ms.topic: conceptual
ms.date: 11/12/2018
ms.openlocfilehash: 6da9b4c6890895141ecc419382f05f667614fb31
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75451820"
---
# <a name="upgrading-and-updating-a-service-fabric-standalone-cluster"></a>Actualización de un clúster de Service Fabric independiente

Para cualquier sistema moderno, diseñar la capacidad de actualización es clave para lograr el éxito a largo plazo de su producto. Un clúster de Azure Service Fabric independiente es un recurso que usted posee. En este artículo se describe lo que se puede actualizar.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Control de la versión de Service Fabric que se ejecuta en el clúster
Asegúrese de que el clúster siempre ejecuta una [versión compatible de Service Fabric](service-fabric-versions.md). Cuando Microsoft anuncia el lanzamiento de una nueva versión de Service Fabric, se marca la versión anterior para que finalice el soporte después de un mínimo de 60 días a partir de la fecha del anuncio. Las versiones nuevas se anuncian [en el blog del equipo de Service Fabric](https://blogs.msdn.microsoft.com/azureservicefabric/). La nueva versión estará disponible para su elección a partir de ese momento.

Puede configurar el clúster para recibir las actualizaciones automáticas cuando Microsoft las publica o puede seleccionar manualmente la versión compatible de Service Fabric que quiere que tenga el clúster. Para obtener más información, lea [Actualización de la versión de Service Fabric que se ejecuta en el clúster](service-fabric-cluster-upgrade-windows-server.md).

## <a name="customize-configuration-settings"></a>Personalización de la configuración

Pueden establecerse muchas [opciones de configuración](service-fabric-cluster-manifest.md) diferentes en el archivo *ClusterConfig.json*, como el nivel de confiabilidad de las propiedades del nodo y del clúster.  Para obtener más información, lea [Actualización de la configuración de un clúster independiente](service-fabric-cluster-config-upgrade-windows-server.md).  También se pueden personalizar muchas otras opciones de configuración más avanzadas.  Para más información, lea [Service Fabric cluster fabric settings](service-fabric-cluster-fabric-settings.md) (Configuración de un clúster de Service Fabric).

## <a name="define-node-properties"></a>Definición de las propiedades del nodo
A veces, es posible que quiera asegurarse de que ciertas cargas de trabajo solo se ejecuten en determinados tipos de nodos en el clúster. Por ejemplo, algunas cargas de trabajo pueden requerir GPU o SSD, al contrario que otras. Para cada uno de los tipos de nodo de un clúster, puede agregar propiedades de nodo personalizados a los nodos del clúster. Las restricciones de posición son las instrucciones adjuntas a los servicios individuales que se seleccionan para una o más propiedades de nodo. Las restricciones de posición definen dónde deben ejecutarse los servicios.

Para obtener más información acerca de las restricciones de selección de ubicación, de las propiedades del nodo y de cómo definirlas, lea [Restricciones de ubicación y propiedades de nodo](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).
 

## <a name="add-capacity-metrics"></a>Adición de métricas de capacidad
Para cada uno de los tipos de nodo, puede agregar las métricas de capacidad personalizadas que desee usar en las aplicaciones para la carga de informes. Para obtener más información sobre el uso de las métricas de capacidad para notificar la carga, consulte los documentos del Administrador de recursos de clúster de Service Fabric que se describen en [Descripción de un clúster de Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md) y sobre [métricas y cargas](service-fabric-cluster-resource-manager-metrics.md).

## <a name="patch-the-os-in-the-cluster-nodes"></a>Revisión del sistema operativo en los nodos del clúster
La aplicación de orquestación de revisiones (POA) es una aplicación de Service Fabric que automatiza la aplicación de revisiones de sistema operativo en un clúster de Service Fabric sin tiempo de inactividad. La [aplicación de orquestación de revisiones de Windows](service-fabric-patch-orchestration-application.md) se puede implementar en el clúster para instalar revisiones de una manera orquestada, manteniendo los servicios disponibles todo el tiempo. 


## <a name="next-steps"></a>Pasos siguientes
* Aprender a personalizar la [configuración de Service Fabric para el clúster](service-fabric-cluster-fabric-settings.md)
* Aprenda cómo [escalar o reducir horizontalmente el clúster](service-fabric-cluster-scale-up-down.md)
* Obtenga información sobre [actualizaciones de aplicaciones](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG
