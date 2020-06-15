---
title: Novedades de Azure Migrate
description: Obtenga información sobre las novedades y actualizaciones recientes del servicio Azure Migrate.
ms.topic: overview
ms.date: 04/19/2020
ms.custom: mvc
ms.openlocfilehash: db52c8ce5e1b7e25d4af0c570189597e05b86588
ms.sourcegitcommit: 79508e58c1f5c58554378497150ffd757d183f30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84331872"
---
# <a name="whats-new-in-azure-migrate"></a>Novedades de Azure Migrate

[Azure Migrate](migrate-services-overview.md) le ayuda a detectar, evaluar y migrar servidores, aplicaciones y datos locales a la nube de Microsoft Azure. En este artículo se resumen las nuevas versiones y características de Azure Migrate.

## <a name="update-june-2020"></a>Actualización (junio de 2020)

Nuevas versiones de dispositivo de Azure Migrate.

### <a name="azure-public-cloud"></a>Nube pública de Azure

**Escenario*** | **Descargar** | **SHA256**
--- | --- | ---
Hyper-V (8.93 MB) | [La versión más reciente](https://aka.ms/migrate/appliance/hyperv) |  572be425ea0aca69a9aa8658c950bc319b2bdbeb93b440577264500091c846a1
VMware (10,9 GB) | [La versión más reciente](https://aka.ms/migrate/appliance/vmware) | cacbdaef927fe5477fa4e1f494fcb7203cbd6b6ce7402b79f234bc0fe69663dd
Físico (63,1 MB) | [La versión más reciente](https://go.microsoft.com/fwlink/?linkid=2105112) | 0a27adf13cc5755e4b23df0c05732c6ac08d1fe8850567cb57c9906fbc3b85a0


### <a name="azure-government"></a>Azure Government

**Escenario*** | **Descargar** | **SHA256**
--- | --- | ---
Hyper-V (63,1 MB) | [La versión más reciente](https://go.microsoft.com/fwlink/?linkid=2120200&clcid=0x409) |  2c5e73a1e5525d4fae468934408e43ab55ff397b7da200b92121972e683f9aa3
VMware (63,1 MB) | [La versión más reciente](https://go.microsoft.com/fwlink/?linkid=2120300&clcid=0x409 ) | 3d5822038646b81f458d89d706832c0a2c0e827bfa9b0a55cc478eaf2757a4de
Físico (63,1 MB) | [La versión más reciente](https://go.microsoft.com/fwlink/?linkid=2120100&clcid=0x409) | 93dfef131026e70acdfad2769cd208ff745ab96a96f013cdf3f9e1e61c9b37e1



## <a name="update-april-2020"></a>Actualización (abril de 2020)

Azure Migrate admite implementaciones en Azure Government. 

- Puede descubrir y evaluar máquinas virtuales de VMware, máquinas virtuales de Hyper-V y servidores físicos.
- Puede migrar máquinas virtuales de VMware locales, máquinas virtuales Hyper-V y servidores físicos a Azure.
- En el caso de VMware, puede realizar una migración con agente o sin agente. [Más información](server-migrate-overview.md).
- [Examine](migrate-support-matrix.md#supported-geographies-azure-government) regiones y zonas geográficas compatibles para obtener Azure Government.
- El [análisis de dependencias basado en agente](concepts-dependency-visualization.md#agent-based-analysis) no está disponible en Azure Government.
- Las características en versión preliminar se admiten en Azure Government, concretamente el [análisis de dependencias sin agente ](concepts-dependency-visualization.md#agentless-analysis) y la [detección de aplicaciones](how-to-discover-applications.md).


## <a name="update-march-2020"></a>Actualización (marzo de 2020)

Ahora hay disponible una instalación basada en scripts para configurar el [dispositivo de Azure Migrate](migrate-appliance.md):

- La instalación basada en script es una alternativa a la instalación .OVA (VMware)/VHD (Hyper-V) del dispositivo.
- Proporciona un script de instalador de PowerShell que se puede usar para configurar el dispositivo para VMware/Hyper-V en una máquina existente que ejecute Windows Server 2016.

## <a name="update-november-2019"></a>Actualización (noviembre de 2019)

Se han agregado varias características nuevas a Azure Migrate:

- **Evaluación de servidores físicos**. Ahora se admite la evaluación de servidores físicos locales, además de la migración de servidores físicos que ya se admite.
- **Evaluación basada en la importación**. Ahora se admite la evaluación de máquinas con metadatos y datos de rendimiento proporcionados en un archivo CSV.
- **Detección de aplicaciones**: Azure Migrate admite ahora la detección en el nivel de aplicación de aplicaciones, roles y características mediante el uso del dispositivo Azure Migrate. Actualmente solo se admite para máquinas virtuales de VMware y solo se limita a la detección (actualmente no se admite la evaluación). [Más información](how-to-discover-applications.md)
- **Visualización de dependencias sin agentes**: Ya no necesita instalar agentes explícitamente para la visualización de dependencias. Ahora se admiten tanto sin agente como basados en agente.
- **Virtual Desktop**: Use herramientas de ISV para evaluar y migrar la infraestructura de escritorio virtual (VDI) local a Windows Virtual Desktop en Azure.
- **Aplicación web**: Migration Assistant de Azure App Service, que se usa para evaluar y migrar aplicaciones web, ahora se integra en Azure Migrate.

Se han agregado a Azure Migrate nuevas herramientas para la evaluación y la migración:

- **Rackware**: Migración a la nube de ofertas.
- **Movere**: Evaluación de ofertas.

[Más información](migrate-services-overview.md) sobre el uso de herramientas y ofertas de ISV para la evaluación y la migración en Azure Migrate.

## <a name="azure-migrate-current-version"></a>Versión actual de Azure Migrate

La versión actual de Azure Migrate (publicada en julio de 2019) proporciona varias características nuevas:

- **Plataforma de migración unificada**: Ahora, Azure Migrate ofrece un único portal para centralizar, administrar y realizar el seguimiento del proceso de migración a Azure, con un flujo de implementación mejorado y una experiencia de portal.
- **Herramientas de evaluación y migración**: Azure Migrate proporciona herramientas nativas y se integra con otros servicios de Azure, así como con herramientas de fabricante de software independiente (ISV). [Más información](migrate-services-overview.md#isv-integration) acerca de la integración de ISV.
- **Evaluación de Azure Migrate**: Puede usar la herramienta de evaluación de servidores de Azure Migrate para evaluar VM de VMware y de Hyper-V para la migración a Azure. También puede evaluar la migración con otros servicios de Azure y herramientas de ISV.
- **Migración de Azure Migrate**: La herramienta de migración de servidores de Azure Migrate permite migrar VM de VMware locales y VM de Hyper-V a Azure, así como servidores físicos, otros servidores virtualizados y VM de nube privada o pública. Además, puede migrar a Azure mediante herramientas de ISV.
- **Dispositivo con Azure Migrate**: Azure Migrate implementa un dispositivo ligero para la detección y evaluación de VM de VMware locales y VM de Hyper-V.
    - La evaluación y la migración de servidores de Azure Migrate usan este dispositivo para realizar una migración sin agente.
    - El dispositivo detecta continuamente los metadatos de servidor y los datos de rendimiento con fines de evaluación y migración.  
- **Migración de VM de VMware**:  La migración de servidores de Azure Migrate proporciona un par de métodos para migrar VM de VMware locales a Azure.  Una migración sin agente mediante el dispositivo de Azure Migrate y una migración basada en agente que usa un dispositivo de replicación e implementa un agente en cada VM que quiere migrar. [Más información](server-migrate-overview.md)
 - **Valoración y migración de bases de datos**: desde Azure Migrate, puede evaluar las bases de datos locales para la migración a Azure mediante Azure Database Migration Assistant. Puede migrar bases de datos con Azure Database Migration Service.
- **Migración de aplicaciones web**: puede evaluar las aplicaciones web con una dirección URL de punto de conexión pública mediante Azure App Service. Para la migración de aplicaciones de .NET internas, puede descargar y ejecutar App Service Migration Assistant.
- **Data Box**: importe grandes cantidades de datos sin conexión a Azure mediante Azure Data Box en Azure Migrate.

## <a name="azure-migrate-previous-version"></a>Versión anterior de Azure Migrate

Si usaba la versión anterior de Azure Migrate (solo se admitía la valoración de máquinas virtuales de VMware locales), ahora debería usar la versión actual. En la versión anterior, ya no puede crear nuevos proyectos de Azure Migrate ni realizar nuevas detecciones. Todavía puede obtener acceso a los proyectos existentes. Para hacerlo, en Azure Portal > **Todos los servicios**, busque **Azure Migrate**. En las notificaciones de Azure Migrate, hay un vínculo para acceder a antiguos proyectos de Azure Migrate.



## <a name="next-steps"></a>Pasos siguientes

- [Más información](https://azure.microsoft.com/pricing/details/azure-migrate/) sobre los precios de Azure Migrate.
- [Repase las preguntas más frecuentes](resources-faq.md) sobre Azure Migrate.
- Pruebe nuestros tutoriales para evaluar las [VM de VMware](tutorial-assess-vmware.md) y [las VM de Hyper-V](tutorial-assess-hyper-v.md).
