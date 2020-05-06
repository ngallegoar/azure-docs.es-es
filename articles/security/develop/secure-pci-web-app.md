---
title: Protección de la aplicación web para PCI DSS | Microsoft Docs
description: Esta aplicación de ejemplo implementa procedimientos recomendados de seguridad que mejoran la seguridad de la aplicación y la organización al desarrollar en Azure.
keywords: na
services: security
documentationcenter: na
author: fehase
manager: rkarlin
editor: ''
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2019
ms.author: terrylan
ms.openlocfilehash: 4ab3697824ff4a47e7b8f281b531cae610ffdc3b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82187586"
---
# <a name="develop-a-secure-infrastructure-for-a-pci-app"></a>Desarrollo de una infraestructura segura para una aplicación PCI

## <a name="overview"></a>Información general

Esta infraestructura segura para una aplicación del sector de las tarjetas de pago (PCI) proporciona una guía para la implementación de un entorno de plataforma como servicio (PaaS) para este sector, que es adecuado para la recopilación, el almacenamiento y la recuperación de datos de titulares de tarjetas. Esta solución automatiza la implementación y la configuración de recursos de Azure para una arquitectura de referencia común, la cual muestra las formas en que los clientes pueden cumplir los requisitos específicos de seguridad y cumplimiento y sirve como base para que los clientes creen y configuren sus propias soluciones en Azure. La solución implementa un subconjunto de requisitos similares a los estándares de seguridad de datos del sector de las tarjetas de pago (PCI DSS 3.2).

Con este ejemplo se implementa automáticamente una arquitectura de referencia de aplicación web PaaS con controles de seguridad configurados previamente para ayudar a los clientes a cumplir con los requisitos de PCI DSS 3.2. La solución consta de plantillas de Azure Resource Manager y scripts de PowerShell que guían la configuración y la implementación de recursos.

Debe seguir los pasos que se describen en este artículo secuencialmente para asegurarse de que los componentes de la aplicación estén configurados correctamente. La base de datos, Azure App Service, la instancia de Azure Key Vault y la instancia de Azure Application Gateway son dependientes entre sí.

Los scripts de implementación configuran la infraestructura. Después de ejecutar los scripts de implementación, deberá realizar cierta configuración manual en Azure Portal para vincular los componentes y los servicios.

Este ejemplo está dirigido a desarrolladores experimentados en Azure que trabajan en el sector minorista y quieren crear una aplicación comercial con la infraestructura segura de Azure.

> [!NOTE]
> Esta arquitectura está diseñada para servir como base para que los clientes puedan ajustarse a sus requisitos específicos y no debe usarse tal cual en un entorno de producción.

Implementar una aplicación sin modificar en este entorno no es suficiente para satisfacer por completo los requisitos de PCI DSS 3.2. Tenga en cuenta lo siguiente:

- Esta arquitectura fundamental proporciona una línea de base para que los clientes puedan usar Azure de forma que sea compatible con PCI DSS 3.2.
- Los clientes tienen la responsabilidad de realizar las evaluaciones de seguridad y cumplimiento adecuadas de cualquier solución integrada mediante esta arquitectura, ya que los requisitos pueden variar en función de las características de implementación de cada cliente.

Al desarrollar e implementar esta aplicación, aprenderá a:

- Crear una instancia de Azure Key Vault, y almacenar y recuperar sus secretos
- Implementar Azure Database for Azure SQL, configurar datos seguros y autorizar el acceso a ellos
- Implementar la aplicación web de Azure con App Service Environment, que es un entorno dedicado aislado con acceso de firewall de front-end
- Cree y configure una instancia de Azure Application Gateway con un firewall que use [el conjunto de 10 reglas principales de OWASP](https://coreruleset.org/).
- Habilite el cifrado de datos en tránsito y en reposo mediante los servicios de Azure.
- Configurar Azure Policy y Azure Blueprints para evaluar el cumplimiento.

Después de desarrollar e implementar esta aplicación, habrá configurado la aplicación web de ejemplo siguiente junto con las medidas de configuración y seguridad que se describen.

## <a name="architecture-diagram-and-components"></a>Componentes y diagrama de la arquitectura
La aplicación es una aplicación de n niveles típica con tres niveles. El front-end, el back-end y el nivel de base de datos con los componentes de supervisión y administración de secretos integrados se muestran aquí:

![Arquitectura de la aplicación](./media/secure-pci-web-app/architecture.png)

La arquitectura consta de los siguientes componentes:

- [App Service Environment v2](https://docs.microsoft.com/azure/app-service/environment/intro/). Proporciona la instancia de App Service Environment y el equilibrador de carga de la arquitectura de la aplicación.
- [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/). Proporciona la puerta de enlace y el firewall para la arquitectura de la aplicación.
- [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview). Proporciona un servicio de Application Performance Management (APM) extensible en varias plataformas.
- [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/) Almacena y cifra los secretos de la aplicación y administra la creación de directivas de acceso en torno a ellos.
- [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis/). Proporciona el servicio de administración de acceso e identidades basado en la nube, inicio de sesión y acceso a los recursos.
- [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview). Proporciona el servicio para hospedar el dominio.
- [Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview/). Escala las aplicaciones y crea alta disponibilidad para los servicios.
- [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-introduction/). Proporciona una solución para escenarios de almacenamiento de datos modernos.
- [Aplicación web de Azure](https://docs.microsoft.com/azure/app-service/overview/).  Proporciona un servicio basado en HTTP para hospedar aplicaciones web.
- [Azure Database for MySQL](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview/) Almacena de forma segura los datos de la aplicación.
- [Azure Blueprints](https://docs.microsoft.com/azure/governance/blueprints/overview/). Proporciona especificaciones y cumplimiento con determinados estándares y requisitos.
- [Azure Security Center](https://docs.microsoft.com/azure/security-center/)
- [Azure Policy](https://docs.microsoft.com/azure/governance/policy/overview). Evalúa los recursos que incumplen las directivas asignadas.

## <a name="threat-model"></a>Modelo de amenazas
El proceso de modelado de amenazas consiste en identificar posibles amenazas de seguridad para la empresa y la aplicación y, luego, asegurarse que exista un plan de mitigación adecuado.

En este ejemplo, se usó [Microsoft Threat Modeling Tool](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool) para implementar el modelado de amenazas para la aplicación de ejemplo segura. Mediante la creación de diagramas de los componentes y los flujos de datos, puede identificar los problemas y las amenazas al principio del proceso de desarrollo. Esto ahorra tiempo y dinero más tarde.

Este es el modelo de amenazas para la aplicación de ejemplo:

![Modelo de amenazas](./media/secure-pci-web-app/threat-model.png)

En la captura de pantalla siguiente se muestran algunas amenazas de ejemplo y posibles vulnerabilidades que genera Threat Modeling Tool. El modelo de amenazas ofrece información general sobre la superficie expuesta a ataques y solicita a los desarrolladores que piensen en cómo mitigar los problemas.

![Salida del modelo de amenazas](./media/secure-web-app/threat-model-output.png)

Por ejemplo, la inyección de SQL en la salida del modelo de amenazas anterior se mitiga mediante la limpieza de las entradas del usuario y el uso de funciones almacenadas en Azure Database for PostgreSQL. Esta mitigación evita la ejecución arbitraria de consultas durante las lecturas y escrituras de datos.

Los desarrolladores mejoran la seguridad general del sistema mediante la mitigación de cada una de las amenazas de la salida del modelo de amenazas.

## <a name="deployment"></a>Implementación
### <a name="prerequisites"></a>Prerrequisitos
Para poner en marcha la aplicación, debe instalar estas herramientas:

- Un editor de código para modificar y ver el código de la aplicación. [Visual Studio Code](https://code.visualstudio.com/) es una opción de código abierto.
- La [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest&viewFallbackFrom=azure-cli-latest) en el equipo de desarrollo.
- [GIT](https://git-scm.com/) en el sistema. GIT se usa para clonar el código fuente localmente.
- [jq](https://stedolan.github.io/jq/), una herramienta de UNIX fácil de usar para consultar JSON.

Este ejemplo se compone de archivos de configuración JSON y scripts de PowerShell que se controlan mediante el servicio API de Azure Resource Manager para implementar recursos en Azure. Puede encontrar instrucciones detalladas acerca de la implementación [aquí](https://github.com/Azure-Samples/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms-AzureRM).

#### <a name="quickstart"></a>Guía de inicio rápido

1.  Clone o descargue [este](https://github.com/Azure-Samples/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms-AzureRM.git) repositorio de GitHub en su estación de trabajo local.
2.  Revise 0-Setup-AdministrativeAccountAndPermission.md y ejecute los comandos proporcionados.
3.  Implemente una solución de prueba con datos de ejemplo Contoso o pruebe un entorno de producción inicial.

    Este script implementa recursos de Azure para una demostración de una tienda web mediante datos de ejemplo de Contoso.

En este ejemplo, ejecutará el script de implementación que implementa la aplicación web en App Service y crea los recursos.

Hay muchas maneras de implementar aplicaciones en Azure, entre las que se incluyen las siguientes:

- Plantillas del Administrador de recursos de Azure
- PowerShell
- Azure CLI
- Azure Portal
- Azure DevOps

## <a name="guidance-and-recommendations"></a>Instrucciones y recomendaciones

### <a name="network"></a>Red
La arquitectura define una red virtual privada con un espacio de direcciones de 10.200.0.0/16.
 ![Virtual_Network](./media/secure-pci-web-app/virtual-network.png)

### <a name="network-security-groups"></a>Grupos de seguridad de red
Los grupos de seguridad de red (https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) contienen listas de control de acceso (ACL) que permiten o deniegan el tráfico en una red virtual. Los grupos de seguridad de red pueden usarse para proteger el tráfico en una subred o un nivel de máquina virtual individual. Existen los siguientes grupos de seguridad de red:

- 1 grupo de seguridad de red para Application Gateway
- 1 grupo de seguridad de red para App Service Environment
- 1 grupo de seguridad de red para Azure SQL Database
- 1 grupo de seguridad de red para host de tipo bastión

Cada uno de los grupos de seguridad de red tiene puertos y protocolos específicos abiertos para que la solución pueda funcionar de forma segura y correcta. Además, las siguientes opciones de configuración están habilitadas para cada grupo de seguridad de red:

- Los eventos y registros de diagnóstico (https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) ) se habilitan y almacenan en una cuenta de almacenamiento.
- Los registros de Azure Monitor están conectados a los diagnósticos del grupo de seguridad de red (https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json).

### <a name="nsg-config"></a>Configuración de NSG
La configuración de NSG para App Service Environment debe realizarse como se muestra en la imagen siguiente.

 ![NSG_Config](./media/secure-pci-web-app/nsg-config.png)

cada subred está asociada a su grupo de seguridad de red correspondiente.

### <a name="config"></a>Config
Las subredes se configuran como se muestra en la imagen siguiente.
 ![Config](./media/secure-pci-web-app/config.png) (Configuración).

### <a name="azure-dns"></a>Azure DNS
el sistema de nombres de dominio (DNS) es responsable de traducir (o resolver) el nombre del sitio web o del servicio en su dirección IP. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) es un servicio de hospedaje para dominios DNS que ofrece resolución de nombres mediante la infraestructura de Azure. Al hospedar dominios en Azure, los usuarios pueden administrar los registros DNS con las mismas credenciales, API, herramientas y facturación que con los demás servicios de Azure. Azure DNS es compatible con dominios DNS privados.

### <a name="protect-data"></a>Protección de datos
Para contribuir a proteger los datos en la nube, debe tener en cuenta los posibles estados que pueden tener los datos y los controles disponibles para ese estado. Los procedimientos recomendados para el cifrado y seguridad de datos de Azure están relacionados con los siguientes estados de datos:

- En reposo: incluye toda la información acerca de los objetos de almacenamiento, contenedores y tipos que existen de forma estática en los soportes físicos, ya sean discos magnéticos u ópticos.
- En tránsito: cuando se transfieren datos entre componentes, ubicaciones o programas, están en tránsito. Algunos ejemplos son la transferencia a través de la red, a través de un bus de servicio (del entorno local a la nube y viceversa, incluidas las conexiones híbridas como ExpressRoute), o durante el proceso de entrada/salida.

### <a name="azure-storage"></a>Azure Storage
Para cumplir los requisitos de los datos cifrados en reposo, en todo [Azure Storage](https://azure.microsoft.com/services/storage/) se usa Azure Key Vault para mantener el control de las claves que acceden a los datos y los cifran. Esto ayuda a proteger los datos de los titulares de tarjetas en cumplimiento de los compromisos de seguridad de la organización y los requisitos de cumplimiento definidos por PCI DSS 3.2.

### <a name="azure-disk-encryption"></a>Azure Disk Encryption
Azure Disk Encryption aprovecha la característica BitLocker de Windows para proporcionar el cifrado de volumen de los discos de datos. La solución se integra con Azure Key Vault para ayudar a controlar y administrar las claves de cifrado del disco.

### <a name="azure-sql-database"></a>Azure SQL Database
La instancia de Azure SQL Database usa las siguientes medidas de seguridad de base de datos:

- La [autenticación y autorización de Active Directory](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) permiten la administración de identidades de usuarios de bases de datos y otros servicios de Microsoft en una ubicación central.
- La [auditoría de bases de datos SQL](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started/) realiza un seguimiento de eventos de bases de datos y los escribe en un registro de auditoría de una cuenta de almacenamiento de Azure.
- Azure SQL Database está configurado para usar el [cifrado de datos transparente](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), que realiza el cifrado y descifrado en tiempo real de la base de datos, las copias de seguridad asociadas y los archivos de registro de transacciones, para proteger la información en reposo. El cifrado de datos transparente garantiza que los datos almacenados no hayan estado sujetos a un acceso no autorizado.
- Las [reglas de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) impiden el acceso a los servidores de bases de datos hasta que se otorgan los permisos adecuados. Asimismo, otorgan acceso a las bases de datos según la dirección IP de origen de cada solicitud.
- La [detección de amenazas de SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) permite detectar y responder ante posibles amenazas a medida que se producen, mediante el envío de alertas de seguridad para actividades sospechosas en la base de datos, posibles puntos vulnerables, ataques por inyección de código SQL y patrones anómalos de acceso a la base de datos.
- Las [columnas cifradas](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) garantizan que los datos confidenciales nunca aparezcan como texto no cifrado en el sistema de base de datos. Después de habilitar el cifrado de datos, solo las aplicaciones cliente o los servidores de aplicaciones con acceso a las claves pueden acceder a los datos de texto no cifrado.
- El [enmascaramiento dinámico de datos de SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) limita la exposición de datos confidenciales al enmascarar los datos para usuarios o aplicaciones sin privilegios. El enmascaramiento dinámico de datos puede detectar de forma automática datos potencialmente confidenciales y sugerir las máscaras adecuadas que se pueden aplicar. Esto ayuda a identificar y reducir el acceso a datos de modo que no salgan de la base de datos mediante un acceso no autorizado. Los clientes son responsables de ajustar la configuración del enmascaramiento dinámico de datos para cumplir con el esquema de la base de datos.

### <a name="identity-management"></a>Administración de identidades
Las siguientes tecnologías proporcionan funcionalidades de administración del acceso a datos del titular de tarjeta en el entorno de Azure:

- Azure Active Directory es el directorio multiinquilino basado en la nube y el servicio de administración de identidades de Microsoft. Todos los usuarios de la solución se crean en Azure Active Directory, incluidos los usuarios que acceden a Azure SQL Database.
- La autenticación para acceder a la aplicación se realiza con Azure Active Directory. Para obtener más información, consulte [Integración de aplicaciones con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/plan-an-application-integration). Además, el cifrado de columnas de la base de datos usa Azure Active Directory para autenticar la aplicación en Azure SQL Database. Para más información, consulte cómo [proteger los datos confidenciales en Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview).
- El control de acceso basado en rol de Azure permite a los administradores especificar permisos de acceso bien definidos para conceder solo el nivel de acceso que los usuarios necesitan para realizar su trabajo. En lugar de dar a cada usuario permisos ilimitados para los recursos de Azure, los administradores pueden permitir solo ciertas acciones para acceder a los datos del titular de tarjeta. El acceso a la suscripción está limitado al administrador de la suscripción.
- Azure Active Directory Privileged Identity Management permite a los clientes minimizar el número de usuarios con acceso a determinada información, como los datos del titular de tarjeta. Los administradores pueden usar Azure Active Directory Privileged Identity Management para detectar, restringir y supervisar las identidades con privilegios y su acceso a los recursos. Esta funcionalidad también puede utilizarse para aplicar un acceso administrativo a petición, Just-in-Time, cuando sea necesario.
- Azure Active Directory Identity Protection detecta posibles puntos vulnerables que afectan a las identidades de una organización, configura respuestas automatizadas para detectar acciones sospechosas relacionadas con esas identidades, investiga incidentes sospechosos y toma las medidas oportunas para resolverlos.

### <a name="secrets-management"></a>Administración de secretos
La solución usa Azure Key Vault para la administración de claves y secretos. Azure Key Vault ayuda a proteger claves criptográficas y secretos usados por servicios y aplicaciones en la nube. Las siguientes funcionalidades de Azure Key Vault ayudan a los clientes a proteger los datos y el acceso a dichos datos:

- Se configuran directivas de acceso avanzadas según las necesidades.
- Se definen directivas de acceso a Key Vault con los permisos mínimos requeridos para las claves y los secretos.
- Todas las claves y los secretos en Key Vault tienen fechas de expiración.
- Todas las claves de Key Vault están protegidas por módulos de seguridad de hardware especializados. El tipo de clave es una clave RSA de 2048 bits protegida por HSM.
- Con Key Vault, puede cifrar claves y secretos (por ejemplo, claves de autenticación, claves de cuenta de almacenamiento, claves de cifrado de datos, archivos .PFX y contraseñas) mediante claves que están protegidas por módulos de seguridad de hardware (HSM).
- Puede usar RBAC para asignar permisos a los usuarios, los grupos y las aplicaciones en un ámbito determinado.
- También puede usar Key Vault para administrar los certificados TLS con renovación automática.
- Los registros de diagnóstico de Key Vault están habilitados con un período de retención de al menos 365 días.
- Las operaciones criptográficas permitidas para las claves están restringidas únicamente a las requeridas.

### <a name="azure-security-center"></a>Azure Security Center
Con Azure Security Center, los clientes pueden aplicar y administrar de forma centralizada las directivas de seguridad entre cargas de trabajo, limitar la exposición a amenazas y detectar ataques y responder a ellos. Además, Azure Security Center accede a las configuraciones existentes de los servicios de Azure para proporcionar recomendaciones de configuración y servicio que ayuden a mejorar la postura de seguridad y a proteger los datos.

Azure Security Center usa una variedad de funcionalidades de detección para alertar a los clientes de posibles ataques contra sus entornos. Estas alertas contienen información útil acerca de lo que desencadenó la alerta, los recursos objetivo y el origen del ataque. Azure Security Center cuenta con un conjunto de alertas de seguridad predefinidas, que se desencadenan cuando tiene lugar una amenaza o actividad sospechosa. Las reglas de alertas personalizadas de Azure Security Center permiten a los clientes definir nuevas alertas de seguridad basadas en los datos ya recopilados en el entorno.

Azure Security Center proporciona alertas de seguridad e incidentes clasificados por orden de prioridad, lo que facilita a los clientes la detección y solución de posibles problemas de seguridad. Se genera un informe de inteligencia de amenazas por cada amenaza detectada para ayudar a los equipos de respuesta a incidentes a investigar y corregir las amenazas.

### <a name="azure-application-gateway"></a>Azure Application Gateway
la arquitectura reduce el riesgo de aparición de puntos vulnerables en la seguridad, ya que usa Azure Application Gateway con el firewall de aplicaciones web configurado y el conjunto de reglas OWASP habilitado. Entre estas funcionalidades, cabe destacar:

- SSL de un extremo a otro
- Deshabilitar TLS v1.0 y v1.1
- Habilitar TLSv1.2
- Firewall de aplicaciones web (modo de prevención).
- Modo de prevención con el conjunto de reglas OWASP 3.0
- Habilitación del registro de diagnósticos
- Sondeos de estado personalizados
- Azure Security Center y Azure Advisor proporcionan protección adicional y notificaciones. Asimismo, Azure Security Center también proporciona un sistema de reputación.

### <a name="logging-and-auditing"></a>Registro y auditoría
Los servicios de Azure proporcionan un registro completo de la actividad de usuario y del sistema, así como de mantenimiento del sistema:

- los [registros de actividad](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) proporcionan información detallada sobre las operaciones realizadas en los recursos de la suscripción. Los registros de actividad pueden ayudar a determinar el iniciador de una operación, el momento en que se produce y el estado.
- los [registros de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) son todos los registros emitidos por todos los recursos. Estos registros incluyen registros del sistema de eventos de Windows, registros de Azure Storage, registros de auditoría de Key Vault y registros de firewall y acceso a Application Gateway. Todos los registros de diagnóstico se escriben en una cuenta de almacenamiento de Azure centralizada y cifrada para su archivado. El usuario puede configurar la retención hasta 730 días para cumplir los requisitos de retención específicos de una organización.

### <a name="azure-monitor-logs"></a>Registros de Azure Monitor
esos registros se consolidan en [registros de Azure Monitor](https://azure.microsoft.com/services/log-analytics/) para el procesamiento, el almacenamiento y la creación de informes de panel. Una vez recopilados, los datos se organizan en tablas independientes para cada tipo de datos dentro de las áreas de trabajo de Log Analytics, lo que permite que todos los datos se puedan analizar conjuntamente con independencia de su origen. Además, Azure Security Center se integra con los registros de Azure Monitor, lo que permite a los clientes usar consultas de Kusto para acceder a sus datos de eventos de seguridad y combinarlos con datos de otros servicios.

Como parte de esta arquitectura se incluyen las siguientes [soluciones de supervisión](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) de Azure:

- [Active Directory Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): la solución Active Directory Health Check evalúa el riesgo y el estado de los entornos de servidor a intervalos regulares y proporciona una lista prioritaria de recomendaciones específicas para la infraestructura de servidor implementada.
- [SQL Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): la solución SQL Health Check evalúa el riesgo y el estado de los entornos de servidor a intervalos regulares y proporciona a los clientes una lista prioritaria de recomendaciones específicas para la infraestructura de servidor implementada.
- [Agent Health](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): la solución Agent Health notifica el número de agentes implementados y su distribución geográfica, así como el número de agentes que no responden y el de agentes que envían datos operativos.
- [Activity Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): la solución Activity Log Analytics ayuda a los clientes a analizar los registros de actividad de todas las suscripciones de Azure para un cliente.

### <a name="azure-monitor"></a>Azure Monitor
[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) ayuda a los usuarios a realizar un seguimiento del rendimiento, mantener la seguridad e identificar tendencias y permite a las organizaciones auditar, crear alertas y archivar datos, incluido el seguimiento de las llamadas a API en sus recursos de Azure.

### <a name="application-insights"></a>Application Insights
[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) es un servicio de Application Performance Management extensible para desarrolladores web en varias plataformas. Application Insights detecta anomalías de rendimiento y los clientes pueden utilizarlo para supervisar la aplicación web activa. Incluye herramientas de análisis eficaces que ayudan a los clientes a diagnosticar problemas y comprender lo que hacen realmente los usuarios con la aplicación. Está diseñado para ayudar a los clientes a mejorar continuamente el rendimiento y la facilidad de uso.

### <a name="azure-key-vault"></a>Azure Key Vault
Cree un almacén para la organización en el que almacenar las claves, y mantenga la responsabilidad de las tareas operativas como se indica a continuación:

- Los datos almacenados en el almacén de claves incluyen:

   - Clave de Application Insights
   - Clave de acceso de almacenamiento de los datos
   - Cadena de conexión
   - Nombre de la tabla de datos
   - Credenciales de usuario

- Se configuran directivas de acceso avanzadas según las necesidades
- Se definen directivas de acceso a Key Vault con los permisos mínimos requeridos para las claves y los secretos
- Todas las claves y los secretos en Key Vault tienen fechas de expiración
- Todas las claves de Key Vault están protegidas con HSM [Tipo de clave = clave RSA de 2048 bits protegida con HSM]
- Se otorgan los permisos necesarios mínimos a todos los usuarios e identidades mediante el control de acceso basado en rol (RBAC)
- Las aplicaciones no comparten un almacén de claves a menos que confíen entre sí y que necesiten tener acceso a los mismos secretos en tiempo de ejecución
- Los registros de diagnóstico de Key Vault están habilitados con un período de retención de al menos 365 días.
- Las operaciones criptográficas permitidas para las claves están restringidas únicamente a las requeridas

### <a name="vpn-and-expressroute"></a>VPN y ExpressRoute
Debe configurarse un túnel VPN seguro o [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) para establecer una conexión segura a los recursos implementados como parte de esta arquitectura de referencia de aplicaciones web de PaaS. Al configurar una VPN o ExpressRoute adecuadamente, los clientes pueden agregar una capa de protección para los datos en tránsito.

Mediante la implementación de un túnel VPN seguro con Azure, se puede crear una conexión privada virtual entre una red local y una red virtual de Azure. Esta conexión tiene lugar a través de Internet y permite a los clientes colocar con seguridad la información en un "túnel" dentro de un vínculo cifrado entre la red del cliente y Azure. La VPN de sitio a sitio es una tecnología segura y madura que empresas de todos los tamaños han implementado durante décadas. Se usa el modo de túnel IPsec en esta opción como mecanismo de cifrado.

Dado que el tráfico dentro del túnel VPN atraviesa Internet con una VPN de sitio a sitio, Microsoft ofrece otra opción de conexión aún más segura. Azure ExpressRoute es un vínculo de WAN dedicado entre Azure y una ubicación local o un proveedor de hospedaje de Exchange. Como las conexiones de ExpressRoute no se realizan a través de una conexión a Internet, ofrecen más confiabilidad, más velocidad, menor latencia y mayor seguridad que las conexiones a Internet normales. Además, dado que se trata de una conexión directa del proveedor de telecomunicaciones del cliente, los datos no viajan a través de Internet y, por lo tanto, no están expuestos a ella.

Hay [disponibles](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) procedimientos recomendados para implementar una red híbrida segura que extienda una red local a Azure.

## <a name="cost-considerations"></a>Consideraciones sobre el costo
En caso de no tener ninguna cuenta de Azure, puede crear una gratuitamente. Vaya a la [página de la cuenta gratuita](https://azure.microsoft.com/free/) para comenzar, ver lo que puede hacer con una cuenta gratuita de Azure y averiguar qué productos son gratis durante 12 meses.

Para implementar los recursos de la aplicación de ejemplo con las características de seguridad, debe pagar algunas características prémium. A medida que la aplicación se escala y los niveles gratis y las pruebas que ofrece Azure deben actualizarse para cumplir los requisitos de la aplicación, es posible que los costos aumenten. Puede usar la [calculadora de precios](https://azure.microsoft.com/pricing/calculator/) de Azure para calcular los costos.

## <a name="next-steps"></a>Pasos siguientes
Los siguientes artículos pueden ayudarle a diseñar, desarrollar e implementar aplicaciones seguras.

- [Diseño](secure-design.md)
- [Desarrollo](secure-develop.md)
- [Implementación](secure-deploy.md)
