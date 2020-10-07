---
title: Información general de Azure Service Fabric
description: Service Fabric es una plataforma de sistemas distribuidos para crear microservicios escalables, confiables y fáciles de administrar.
ms.topic: overview
ms.date: 09/22/2020
ms.custom: contentperfq1
ms.openlocfilehash: 3c282178decc1a07b2c0acc102b279688c42d52e
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91300656"
---
# <a name="overview-of-azure-service-fabric"></a>Información general de Azure Service Fabric

Azure Service Fabric es una [plataforma de sistemas distribuidos](#container-orchestration) que facilita el empaquetado, la implementación y la administración de microservicios y contenedores escalables y confiables. Service Fabric también aborda los desafíos importantes en el [desarrollo y la administración](#application-lifecycle-management) de aplicaciones nativas en la nube.

Un elemento diferenciador clave de Service Fabric es el enfoque prioritario que da a la creación de servicios con estado. Puede usar el [modelo de programación](#stateless-and-stateful-microservices) de Service Fabric o ejecutar servicios con estado en contenedores escritos en cualquier lenguaje o código. Puede crear [clústeres de Service Fabric en cualquier lugar](#any-os-any-cloud), incluidos Windows Server y Linux en entornos locales y otras nubes públicas, además de Azure.

![La plataforma Service Fabric proporciona administración del ciclo de vida, disponibilidad, orquestación, modelos de programación, mantenimiento y supervisión, herramientas de desarrollo y operaciones, y escalabilidad automática (en Azure, en entornos locales, en otras nubes y en la máquina de desarrollo).][Image1]

En la actualidad, Service Fabric se utiliza en muchos servicios Microsoft, como Azure SQL Database, Azure Cosmos DB, Cortana, Microsoft Power BI, Microsoft Intune, Azure Event Hubs, Azure IoT Hub, Dynamics 365, Skype Empresarial y muchos servicios principales de Azure.

## <a name="container-orchestration"></a>Orquestación de contenedores

Service Fabric es el [orquestador de contenedores](service-fabric-cluster-resource-manager-introduction.md) de Microsoft para implementar y administrar microservicios en un clúster de máquinas, que aprovecha las lecciones aprendidas durante la ejecución de servicios de Microsoft a gran escala. Service Fabric puede implementar aplicaciones en cuestión de segundos, con una alta densidad de cientos o miles de aplicaciones o contenedores por máquina. Con Service Fabric, puede mezclar los servicios en procesos y los servicios en contenedores en la misma aplicación.

[Obtenga más información sobre los conceptos principales, los modelos de programación, el ciclo de vida de la aplicación, las pruebas, los clústeres y la supervisión del estado de Service Fabric](service-fabric-content-roadmap.md).

## <a name="stateless-and-stateful-microservices"></a>Microservicios sin estado y con estado

Service Fabric proporciona un entorno de ejecución sofisticado y ligero que admite microservicios con estado y sin él. Un elemento diferenciador clave de Service Fabric es su solida compatibilidad para crear servicios con estado, ya sea con los [modelos de programación integrados](service-fabric-choose-framework.md) o con servicios con estado en contenedores de Service Fabric.

Obtenga más información sobre [escenarios de aplicación](service-fabric-application-scenarios.md) que se benefician de los servicios con estado de Service Fabric.

## <a name="application-lifecycle-management"></a>Administración del ciclo de vida de aplicación

Service Fabric ofrece compatibilidad para todo el ciclo de vida de las aplicaciones y el CI/CD de las aplicaciones de nube que incluye los contenedores: desde el desarrollo hasta la implementación, la supervisión diaria, la administración y el mantenimiento y la eventual retirada. Service Fabric se integra con herramientas de CI/CD como [Azure Pipelines](https://www.visualstudio.com/team-services/), [Jenkins](https://jenkins.io/index.html) y [Octopus Deploy](https://octopus.com/), y puede utilizarse con cualquier otra herramienta de CI/CD popular.

Para más información sobre la administración del ciclo de vida de las aplicaciones, lea [Ciclo de vida de la aplicación de Service Fabric](service-fabric-application-lifecycle.md). Para implementar las aplicaciones existentes en Service Fabric, consulte [Implementación de un archivo ejecutable invitado](service-fabric-deploy-existing-app.md).

## <a name="any-os-any-cloud"></a>Cualquier sistema operativo, cualquier nube

Puede crear clústeres de Service Fabric en muchos entornos, entre los que se incluyen [Azure o en entornos locales](service-fabric-deploy-anywhere.md), en [Windows Server o en Linux](service-fabric-linux-windows-differences.md). Incluso puede crear clústeres en otras nubes públicas. El entorno de desarrollo del SDK de Service Fabric es idéntico al de producción; no se usa ningún emulador. En otras palabras, lo que se ejecuta en el clúster de desarrollo local, es lo que se implementa en los clústeres de otros entornos.

En el caso del [desarrollo en Windows](service-fabric-get-started.md), el SDK de .NET de Service Fabric se integra con Visual Studio y PowerShell. En el caso del [desarrollo en Linux](service-fabric-get-started-linux.md), el SDK de Java de Service Fabric se integra con Eclipse, y Yeoman se usa para generar plantillas para Java, .NET Core y aplicaciones de contenedor.

## <a name="compliance"></a>Cumplimiento normativo

El proveedor de recursos de Azure Service Fabric está disponible en todas las regiones de Azure y cumple todas las certificaciones de cumplimiento de Azure, entre los que se incluyen: SOC, ISO, PCI DSS, HIPAA y GDPR. Para ver una lista completa, consulte [Ofertas de cumplimiento de Microsoft](https://www.microsoft.com/trustcenter/compliance/complianceofferings).

## <a name="next-steps"></a>Pasos siguientes

Cree la primera aplicación en Azure Service Fabric e impleméntela:

> [!div class="nextstepaction"]
> [Inicio rápido de Service Fabric][sf-quickstart]

[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png
[sf-quickstart]: ./service-fabric-quickstart-dotnet.md
