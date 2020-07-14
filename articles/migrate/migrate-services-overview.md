---
title: Acerca de Azure Migrate
description: Más información sobre el servicio de Azure Migrate
ms.topic: overview
ms.date: 04/15/2020
ms.custom: mvc
ms.openlocfilehash: 72f2dfb3e5e05466892d838af4e1c84c855b94a9
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86122088"
---
# <a name="about-azure-migrate"></a>Acerca de Azure Migrate

En este artículo se proporciona una rápida descripción general del servicio Azure Migrate.

Azure Migrate proporciona un centro centralizado para evaluar y migrar a Azure servidores, infraestructuras, aplicaciones y datos locales. Ofrece lo siguiente:

- **Plataforma de migración unificada**: un único portal para iniciar, ejecutar y realizar un seguimiento de la migración a Azure.
- **Rango de herramientas**: Rango de herramientas para la evaluación y migración Las herramientas de Azure Migrate incluyen Server Assessment y Azure Migrate: Server Migration. Azure Migrate también se integra con otros servicios y herramientas de Azure, así como con ofertas de proveedores de software independientes (ISV).
- **Evaluación y migración** En el centro de Azure Migrate, puede evaluar y migrar:
    - **Servidores**: evalúe servidores locales y mígrelos a máquinas virtuales de Azure o Azure VMware Solution (AVS, versión preliminar).
    - **Bases de datos**: Evalúe bases de datos locales y mígrelas a Azure SQL Database o a una instancia administrada de SQL.
    - **Aplicaciones web** Evalúe aplicaciones web locales y mígrelas a Azure App Service mediante Azure App Service Migration Assistant.
    - **Escritorios virtuales**: Evalúe la infraestructura de escritorio virtual (VDI) local y mígrela a Windows Virtual Desktop en Azure.
    - **Data**: Migre grandes cantidades de datos a Azure de manera rápida y rentable gracias a los productos de Azure Data Box.

## <a name="integrated-tools"></a>Herramientas integradas

El centro de Azure Migrate incluye estas herramientas:

**Herramienta** | **Evaluación y migración** | **Detalles**
--- | --- | ---
**Azure Migrate: Server Assessment** | Evalúe servidores. | Detecte y evalúe máquinas virtuales de VMware locales, máquinas virtuales de Hyper-V y servidores físicos para preparar la migración a Azure.
**Azure Migrate: Server Migration** | Migre servidores. | Migre máquinas virtuales de VMware locales, máquinas virtuales de Hyper-V, servidores físicos, otras máquinas virtualizadas y máquinas virtuales de nube pública a Azure.
**Data Migration Assistant** | Evalúe las bases de datos de SQL Server para la migración a Azure SQL Database, Instancia administrada de Azure SQL o máquinas virtuales de Azure que ejecutan SQL Server. | Data Migration Assistant ayuda a identificar posibles problemas que bloquean la migración. Identifica características no admitidas, nuevas características que puede aprovechar después de la migración y la ruta de acceso correcta para la migración de la base de datos. [Más información](/sql/dma/dma-overview?view=sql-server-2017).
**Azure Database Migration Service** | Migre bases de datos locales a máquinas virtuales de Azure en las que se ejecutan SQL Server, Azure SQL Database o instancias administradas de SQL. | [Más información](../dms/dms-overview.md) sobre Database Migration Service.
**Movere** | Evalúe servidores. | [Más información](#movere) acerca de Movere.
**Migration Assistant para aplicaciones web** | Evalúe aplicaciones web locales y mígrelas a Azure. |  Use Azure App Service Migration Assistant para evaluar sitios web locales para la migración a Azure App Service.<br/><br/> Use Migration Assistant para migrar aplicaciones web de .NET y PHP a Azure. [Más información](https://appmigration.microsoft.com/) sobre Azure App Service Migration Assistant.
**Azure Data Box** | Migre datos sin conexión. | Use los productos de Azure Data Box para trasladar grandes cantidades de datos sin conexión a Azure. [Más información](../databox/index.yml).

> [!NOTE]
> Si se encuentra en Azure Government, las herramientas externas integradas y las ofertas del ISV no pueden enviar datos a proyectos de Azure Migrate. Las herramientas se pueden usar de forma independiente.

## <a name="isv-integration"></a>Integración de ISV

Azure Migrate se integra en varias ofertas de ISV. 

**ISV**    | **Característica**
--- | ---
[Carbonite](https://www.carbonite.com/globalassets/files/datasheets/carb-migrate4azure-microsoft-ds.pdf) | Migre servidores.
[Cloudamize](https://www.cloudamize.com/platform) | Evalúe servidores.
[Corent Technology](https://www.corenttech.com/AzureMigrate/) | Evalúe y migre servidores.
[Device42](https://docs.device42.com/) | Evalúe servidores.
[Lakeside](https://go.microsoft.com/fwlink/?linkid=2104908) | Evalúe VDI.
[RackWare](https://go.microsoft.com/fwlink/?linkid=2102735) | Migre servidores.
[Turbonomic](https://learn.turbonomic.com/azure-migrate-portal-free-trial) | Evalúe servidores.
[UnifyCloud](https://www.cloudatlasinc.com/cloudrecon/) | Evalúe servidores y bases de datos.

## <a name="azure-migrate-server-assessment-tool"></a>Azure Migrate: Herramienta Server Assessment

Azure Migrate: La herramienta Server Assessment detecta y evalúa máquinas virtuales de VMware locales, máquinas virtuales de Hyper-V y servidores físicos para la migración a Azure. 

Esto es lo que hace la herramienta:

- **Preparación para Azure**: Evalúa si las máquinas locales están listas para la migración a Azure.
- **Ajuste de tamaño de Azure**: estima el tamaño de las VM de Azure o el número de nodos de Azure VMware después de la migración.
- **Estimación de costos de Azure**: Calcule el costo de la ejecución de servidores locales en Azure.
- **Análisis de dependencias**: Identifica las dependencias entre servidores y las estrategias de optimización para mover servidores interdependientes a Azure. Más información sobre Server Assessment con [análisis de dependencias](concepts-dependency-visualization.md).

Server Assessment usa un [dispositivo de Azure Migrate](migrate-appliance.md) ligero que se implementa en el entorno local.

- El dispositivo se ejecuta en una máquina virtual o en un servidor físico. Puede instalarlo fácilmente mediante una plantilla descargada.
- El dispositivo detecta las máquinas locales. También envía continuamente los metadatos y los datos de rendimiento de las máquinas a Azure Migrate.
- La detección del dispositivo es sin agente. No se instala nada en las máquinas detectadas.
- Después de la detección del dispositivo, puede recopilar las máquinas detectadas en grupos y ejecutar evaluaciones de cada uno de ellos.

## <a name="azure-migrate-server-migration-tool"></a>Azure Migrate: Herramienta Server Migration

Azure Migrate: La herramienta Server Migration le ayuda a migrar a Azure:

**Migrar** | **Detalles**
--- | ---
Máquinas virtuales de VMware locales | Migre máquinas virtuales a Azure mediante la migración basada en agente o sin agente.<br/><br/> Para la migración sin agente, Server Migration usa un dispositivo de Azure Migrate que se implementa en el entorno local. Es el mismo tipo de dispositivo que se usa para Server Assessment.<br/><br/> En el caso de la migración basada en agente, Server Assessment usa un dispositivo de replicación.
VM de Hyper-V locales | Migre máquinas virtuales a Azure.<br/><br/> Server Assessment usa agentes de proveedor instalados en el host de Hyper-V para la migración.
Servidores físicos locales | Es posible migrar máquinas físicas a Azure. También se pueden migrar otras máquinas1 virtualizadas y máquinas virtuales de otras nubes públicas, si se tratan como máquinas virtuales con el fin de realizar la migración. | Server Assessment usa un dispositivo de replicación para la migración.


## <a name="selecting-assessment-and-migration-tools"></a>Selección de herramientas de valoración y migración

En el centro de Azure Migrate, seleccione la herramienta que desea usar para la evaluación o migración, y agréguela a un proyecto de Azure Migrate. Si agrega una herramienta de ISV o a Movere:

- Para empezar, obtenga una licencia o regístrese para obtener una evaluación gratuita, para lo que debe seguir las instrucciones de la herramienta. Cada ISV o herramienta especifica las licencias.
- Cada herramienta tiene una opción para conectarse a Azure Migrate. Siga las instrucciones de la herramienta para conectarse.
- Realice un seguimiento de la migración en todas las herramientas desde el proyecto de Azure Migrate.

## <a name="movere"></a>Movere

Movere es una plataforma de software como servicio (SaaS). Aumenta la inteligencia empresarial, ya que presentar con precisión entornos de TI completos en un solo día. Las organizaciones y las empresas crecen, cambian y se optimizan digitalmente. Cuando lo hacen, Movere les proporciona la confianza necesaria para ver y controlar sus entornos, independientemente de la plataforma, la aplicación o la geografía.

Microsoft ha [adquirido](https://azure.microsoft.com/blog/microsoft-acquires-movere-to-help-customers-unlock-cloud-innovation-with-seamless-migration-tools/) Movere y ha dejado de comercializarse como una oferta independiente. Movere está disponible mediante los programas Microsoft Solution Assessment y Microsoft Cloud Economics. [Más información](https://www.movere.io) acerca de Movere.

Le recomendamos que consulte también Azure Migrate, nuestro servicio de migración integrado. Azure Migrate proporciona un centro para simplificar la migración a la nube. Este centro admite totalmente cargas de trabajo, como los servidores físicos y virtuales, las bases de datos y las aplicaciones. La visibilidad de un extremo a otro permite hacer un seguimiento del progreso en la detección, la evaluación y la migración de forma sencilla.

Con las herramientas de Azure y de los ISV asociados integradas, Azure Migrate tiene una amplio rango de características, entre las que se incluyen:

- Detección de servidores virtuales y físicos.
- Elección del tamaño adecuado en función del rendimiento.
- Planeamiento de costos.
- Valoraciones basadas en la importación.
- Análisis de dependencias de aplicaciones sin agente.

Si buscando ayuda experta para empezar, Microsoft cuenta con [proveedores de servicios administrados expertos de Azure](https://azure.microsoft.com/partners) que le guiarán. Consulte el [sitio web de Azure Migrate](https://azure.microsoft.com/services/azure-migrate/). 

## <a name="azure-migrate-versions"></a>Versiones de Azure Migrate

Hay dos versiones del servicio Azure Migrate.

- **Versión actual**: use esta versión para crear proyectos de Azure Migrate, detectar máquinas locales y organizar valoraciones y migraciones. [Obtenga más información](whats-new.md) sobre las novedades de esta versión.
- **Versión anterior**: La versión anterior de Azure Migrate solo admite la valoración de máquinas virtuales de VMware locales. Si ha usado la versión anterior, ahora debería usar la actual. Con la versión anterior ya no se pueden crear proyectos de Azure Migrate. Y es aconsejable no realizar nuevas detecciones con ella.

    Para acceder a los proyectos existentes en Azure Portal, busque **Azure Migrate** y selecciónelo. El panel de **Azure Migrate** tiene una notificación y un vínculo para acceder a proyectos antiguos de Azure Migrate.

## <a name="next-steps"></a>Pasos siguientes

- Pruebe nuestros tutoriales para evaluar [máquinas virtuales de VMware](tutorial-prepare-vmware.md), [máquinas virtuales de Hyper-V](tutorial-prepare-hyper-v.md) o [servidores físicos](tutorial-prepare-physical.md).
- [Repase las preguntas más frecuentes](resources-faq.md) sobre Azure Migrate.
