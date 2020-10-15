---
title: Ejemplos de manifiestos de aplicación de Azure Service Fabric
description: Obtenga información acerca de cómo establecer la configuración de un manifiesto de servicio y de aplicación para una aplicación de Service Fabric.
author: peterpogorski
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: pepogors
ms.openlocfilehash: 6f44d2ddaac11545a3a3dc52fd20e02263c6ad67
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75451638"
---
# <a name="service-fabric-application-and-service-manifest-examples"></a>Ejemplos de manifiestos de servicio y de aplicación de Service Fabric
Esta sección contiene ejemplos de manifiestos de servicio y de aplicación. Estos ejemplos no están diseñados para mostrar escenarios importantes, sino para explicar las distintas configuraciones que están disponibles y cómo usarlas. 

El siguiente es un índice de las características mostradas y los manifiestos de ejemplo de las que forman parte.

|Característica|Manifest|
|---|---|
|[Gobernanza de recursos](service-fabric-resource-governance.md)|[Manifiesto de aplicación de Reliable Services](service-fabric-manifest-example-reliable-services-app.md#application-manifest), [Manifiesto de aplicación de contenedor](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Ejecución de un servicio como una cuenta de administrador local](service-fabric-application-runas-security.md)|[Implementación de una aplicación de Reliable Services](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Aplicación de una directiva predeterminada a todos los paquetes de código de servicio](service-fabric-application-runas-security.md#apply-a-default-policy-to-all-service-code-packages)|[Implementación de una aplicación de Reliable Services](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Creación de entidades de usuario y grupo](service-fabric-application-runas-security.md)|[Implementación de una aplicación de Reliable Services](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|compartir un paquete de datos entre instancias de servicio|[Implementación de una aplicación de Reliable Services](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Invalidación de puntos de conexión de servicio](service-fabric-service-manifest-resources.md#overriding-endpoints-in-servicemanifestxml)|[Implementación de una aplicación de Reliable Services](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Ejecución de un script al iniciar el servicio](service-fabric-run-script-at-service-startup.md)|[Manifiesto del servicio VotingWeb](service-fabric-manifest-example-reliable-services-app.md#votingweb-service-manifest)|
|[Definición de una punto de conexión HTTPS](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md#define-an-https-endpoint-in-the-service-manifest)|[Manifiesto del servicio VotingWeb](service-fabric-manifest-example-reliable-services-app.md#votingweb-service-manifest)|
|[Declaración de un paquete de configuración](service-fabric-application-and-service-manifests.md)|[Manifiesto del servicio VotingData](service-fabric-manifest-example-reliable-services-app.md#votingdata-service-manifest)|
|[Declaración de un paquete de datos](service-fabric-application-and-service-manifests.md)|[Manifiesto del servicio VotingData](service-fabric-manifest-example-reliable-services-app.md#votingdata-service-manifest)|
|[Invalidación de variables de entorno](service-fabric-get-started-containers.md#configure-and-set-environment-variables)|[Manifiesto de la aplicación de contenedor](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Configuración de la asignación de puerto a host de contenedor](service-fabric-get-started-containers.md#configure-container-port-to-host-port-mapping-and-container-to-container-discovery)| [Manifiesto de la aplicación de contenedor](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Configuración de la autenticación de Container Registry](service-fabric-get-started-containers.md#configure-container-repository-authentication)|[Manifiesto de la aplicación de contenedor](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Establecimiento del modo de aislamiento](service-fabric-get-started-containers.md#configure-isolation-mode)|[Manifiesto de la aplicación de contenedor](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Especificación de las imágenes de contenedores de compilación específica del sistema operativo](service-fabric-get-started-containers.md#specify-os-build-specific-container-images)|[Manifiesto de la aplicación de contenedor](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Establecimiento de variables de entorno](service-fabric-get-started-containers.md#configure-and-set-environment-variables)|[Manifiesto del servicio FrontEndService de contenedor](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest), [Manifiesto del servicio BackEndService de contenedor](service-fabric-manifest-example-container-app.md#backendservice-service-manifest)|
|[Configuración de un punto de conexión](service-fabric-get-started-containers.md#configure-communication)|[Manifiesto del servicio FrontEndService de contenedor](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest), [Manifiesto del servicio BackEndService de contenedor](service-fabric-manifest-example-container-app.md#backendservice-service-manifest), [Manifiesto del servicio VotingData](service-fabric-manifest-example-reliable-services-app.md#votingdata-service-manifest)|
|pasar comandos al contenedor|[Manifiesto del servicio FrontEndService de contenedor](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest)|
|[Importación de un certificado en un contenedor](service-fabric-securing-containers.md)|[Manifiesto del servicio FrontEndService de contenedor](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest)|
|[Configuración de un controlador de volumen](service-fabric-containers-volume-logging-drivers.md)|[Manifiesto del servicio BackEndService de contenedor](service-fabric-manifest-example-container-app.md#backendservice-service-manifest)|

