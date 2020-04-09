---
title: Introducción a las etiquetas de servicio de Azure
titlesuffix: Azure Virtual Network
description: Obtenga información sobre las etiquetas de servicio. Las etiquetas de servicio ayudan a minimizar la complejidad de la creación de reglas de seguridad.
services: virtual-network
documentationcenter: na
author: jispar
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/12/2020
ms.author: jispar
ms.reviewer: kumud
ms.openlocfilehash: 7da20a0bf87b33f05ea7f1d457157c5b7ee2ec7b
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631528"
---
# <a name="virtual-network-service-tags"></a>Etiquetas de servicio de red virtual
<a name="network-service-tags"></a>

Una etiqueta de servicio representa un grupo de prefijos de direcciones IP de un servicio de Azure determinado. Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian, lo que minimiza la complejidad de las actualizaciones frecuentes en las reglas de seguridad de red.

Puede usar etiquetas de servicio para definir controles de acceso a la red en [grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules)  o  [Azure Firewall](https://docs.microsoft.com/azure/firewall/service-tags). Utilice etiquetas de servicio en lugar de direcciones IP específicas al crear reglas de seguridad. Al especificar el nombre de la etiqueta de servicio (por ejemplo, **ApiManagement**) en el campo de *origen* o *destino* apropiado de una regla, puede permitir o denegar el tráfico para el servicio correspondiente.

Puede usar etiquetas de servicio para lograr el aislamiento de red y proteger los recursos de Azure de Internet general mientras accede a los servicios de Azure que tienen puntos de conexión públicos. Cree reglas de grupo de seguridad de red de entrada y salida para denegar el tráfico hacia y desde **Internet** y permitir el tráfico desde y hacia **AzureCloud** u otras [etiquetas de servicio disponibles](#available-service-tags) de servicios específicos de Azure.

## <a name="available-service-tags"></a>Etiquetas de servicio disponibles
En la tabla siguiente se incluyen todas las etiquetas de servicio disponibles que se pueden usar en reglas de [grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules).

Las columnas indican si la etiqueta:

- Es adecuada para reglas que cubren el tráfico entrante o saliente.
- Es compatible con el ámbito [regional](https://azure.microsoft.com/regions).
- Se puede usar en las reglas de [Azure Firewall](https://docs.microsoft.com/azure/firewall/service-tags).

De forma predeterminada, las etiquetas de servicio reflejan los intervalos de toda la nube. Algunas etiquetas de servicio también permiten un control más preciso, ya que restringen los intervalos de direcciones IP correspondientes a una región especificada. Por ejemplo, la etiqueta de servicio **Storage** representa a Azure Storage para toda la nube, pero **Storage.WestUS** limita el rango solo a los intervalos de direcciones IP de almacenamiento de la región Oeste de EE. UU. En la tabla siguiente se indica si cada etiqueta de servicio admite este ámbito regional.  

| Etiqueta | Propósito | ¿Se puede usar para tráfico entrante o saliente? | ¿Puede ser regional? | ¿Se puede usar con Azure Firewall? |
| --- | -------- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **ActionGroup** | Grupo de acciones. | Entrada | No | No |
| **ApiManagement** | Tráfico de administración para implementaciones dedicadas de Azure API Management. <br/><br/>*Nota:* Esta etiqueta representa el punto de conexión de servicio de API Management de Azure para el plano de control por región. Esto permite a los clientes realizar operaciones de administración en las API, operaciones, directivas, valores con nombre configurados en el servicio de API Management.  | Entrada | Sí | Sí |
| **ApplicationInsightsAvailability** | Disponibilidad de Application Insights. | Entrada | No | No |
| **AppConfiguration** | App Configuration. | Salida | No | No |
| **AppService**    | Azure App Service. Esta etiqueta se recomienda para reglas de seguridad de salida a front-ends de aplicaciones web. | Salida | Sí | Sí |
| **AppServiceManagement** | Tráfico de administración para las implementaciones dedicadas a App Service Environment. | Ambos | No | Sí |
| **AzureActiveDirectory** | Azure Active Directory. | Salida | No | Sí |
| **AzureActiveDirectoryDomainServices** | Tráfico de administración para las implementaciones dedicadas a Azure Active Directory Domain Services. | Ambos | No | Sí |
| **AzureAdvancedThreatProtection** | Azure Advanced Threat Protection | Salida | No | No |
| **AzureBackup** |Azure Backup.<br/><br/>*Nota:* Esta etiqueta tiene dependencia de las etiquetas **Storage** y **AzureActiveDirectory**. | Salida | No | Sí |
| **AzureBotService** | Azure Bot Service. | Salida | No | No |
| **AzureCloud** | Todos las [direcciones IP públicas del centro de recursos](https://www.microsoft.com/download/details.aspx?id=56519). | Salida | Sí | Sí |
| **AzureCognitiveSearch** | Azure Cognitive Search. <br/><br/>Esta etiqueta o las direcciones IP que cubre esta etiqueta se pueden usar para conceder a los indexadores un acceso seguro a los orígenes de datos. Consulte la [documentación de conexión de indexación](https://docs.microsoft.com/azure/search/search-indexer-troubleshooting#connection-errors) para obtener más detalles. <br/><br/> *Nota*: La dirección IP del servicio de búsqueda no se incluye en la lista de intervalos IP para esta etiqueta de servicio y **también se debe agregar** al firewall de IP de los orígenes de datos. | Entrada | No | No |
| **AzureConnectors** | Conectores de Azure Logic Apps para las conexiones de sondeo y back-end. | Entrada | Sí | Sí |
| **AzureContainerRegistry** | Azure Container Registry. | Salida | Sí | Sí |
| **AzureCosmosDB** | Azure Cosmos DB. | Salida | Sí | Sí |
| **AzureDatabricks** | Azure Databricks. | Ambos | No | No |
| **AzureDataExplorerManagement** | Administración de Azure Data Explorer. | Entrada | No | No |
| **AzureDataLake** | Azure Data Lake Storage Gen1. | Salida | No | Sí |
| **AzureDevSpaces** | Azure Dev Spaces. | Salida | No | No |
| **AzureEventGrid** | Azure Event Grid. <br/><br/>*Nota:* Esta etiqueta cubre los puntos de conexión de Azure Event Grid en Centro y Sur de EE. UU., Este de EE. UU., Este de EE. UU. 2, Oeste de EE. UU. 2 y Centro de EE. UU. solamente. | Ambos | No | No |
| **AzureFrontDoor.Frontend** <br/> **AzureFrontDoor.Backend** <br/> **AzureFrontDoor.FirstParty**  | Azure Front Door. | Ambos | No | No |
| **AzureInformationProtection** | Azure Information Protection.<br/><br/>*Nota:* Esta etiqueta presenta dependencia con las etiquetas **AzureActiveDirectory**, **AzureFrontDoor.Frontend** y **AzureFrontDoor.FirstParty**. | Salida | No | No |
| **AzureIoTHub** | Azure IoT Hub. | Salida | No | No |
| **AzureKeyVault** | Azure Key Vault.<br/><br/>*Nota:* Esta etiqueta tiene dependencia en la etiqueta **AzureActiveDirectory**. | Salida | Sí | Sí |
| **AzureLoadBalancer** | Equilibrador de carga de la infraestructura de Azure. La etiqueta se traduce en la [dirección IP virtual del host](security-overview.md#azure-platform-considerations) (168.63.129.16) donde se originan los sondeos de mantenimiento de Azure. Esto no incluye el tráfico al recurso de Azure Load Balancer. Si no usa Azure Load Balancer, puede reemplazar esta regla. | Ambos | No | No |
| **AzureMachineLearning** | Azure Machine Learning. | Ambos | No | Sí |
| **AzureMonitor** | Log Analytics, Application Insights, AzMon y métricas personalizadas (puntos de conexión GiG).<br/><br/>*Nota:* Para Log Analytics, esta etiqueta tiene dependencia en la etiqueta **Storage**. | Salida | No | Sí |
| **AzureOpenDatasets** | Azure Open Datasets.<br/><br/>*Nota:* Esta etiqueta tiene dependencia de las etiquetas **AzureFrontDoor.Frontend** y **Storage**. | Salida | No | No |
| **AzurePlatformDNS** | El servicio DNS de infraestructura básica (predeterminado).<br/><br>Puede usar esta etiqueta para deshabilitar el DNS predeterminado. Tenga cuidado al usar esta etiqueta. Se recomienda leer las [consideraciones sobre la plataforma Azure](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations). También se recomienda realizar pruebas antes de usar esta etiqueta. | Salida | No | No |
| **AzurePlatformIMDS** | Azure Instance Metadata Service (IMDS), que es un servicio de infraestructura básico.<br/><br/>Puede usar esta etiqueta para deshabilitar el IMDS predeterminado. Tenga cuidado al usar esta etiqueta. Se recomienda leer las [consideraciones sobre la plataforma Azure](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations). También se recomienda realizar pruebas antes de usar esta etiqueta. | Salida | No | No |
| **AzurePlatformLKM** | Servicio de administración de claves o licencias de Windows.<br/><br/>Puede usar esta etiqueta para deshabilitar los valores predeterminados para licencias. Tenga cuidado al usar esta etiqueta. Se recomienda leer las [consideraciones sobre la plataforma Azure](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations).  También se recomienda realizar pruebas antes de usar esta etiqueta. | Salida | No | No |
| **AzureResourceManager** | Azure Resource Manager. | Salida | No | No |
| **AzureSignalR** | Azure SignalR. | Salida | No | No |
| **AzureSiteRecovery** | Azure Site Recovery.<br/><br/>*Nota:* Esta etiqueta presenta dependencia con las etiquetas **AzureActiveDirectory**, **AzureKeyVault**, **EventHub**, **GuestAndHybridManagement** y **Storage**. | Salida | No | No |
| **AzureTrafficManager** | Direcciones IP de sondeo de Azure Traffic Manager.<br/><br/>En [Preguntas más frecuentes sobre Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs), puede encontrar más información acerca de las direcciones IP de sondeo de Traffic Manager. | Entrada | No | Sí |  
| **BatchNodeManagement** | Tráfico de administración para implementaciones dedicadas de Azure Batch. | Ambos | No | Sí |
| **CognitiveServicesManagement** | Los intervalos de direcciones para el tráfico para Azure Cognitive Services. | Ambos | No | No |
| **DataFactory**  | Azure Data Factory | Ambos | No | No |
| **DataFactoryManagement** | Tráfico de administración para Azure Data Factory. | Salida | No | No |
| **Dynamics365ForMarketingEmail** | Los intervalos de direcciones del servicio de correo electrónico de marketing de Dynamics 365. | Salida | Sí | No |
| **ElasticAFD** | Azure Front Door elástico. | Ambos | No | No |
| **EventHub** | Azure Event Hubs. | Salida | Sí | Sí |
| **GatewayManager** | Tráfico de administración para implementaciones dedicadas a Azure VPN Gateway y Application Gateway. | Entrada | No | No |
| **GuestAndHybridManagement** | Azure Automation y Configuración de invitado | Salida | No | Sí |
| **HDInsight** | HDInsight de Azure. | Entrada | Sí | No |
| **Internet** | Espacio de direcciones IP que se encuentra fuera de la red virtual y es accesible a través de la red pública de Internet.<br/><br/>El intervalo de direcciones incluye el [espacio de direcciones IP públicas propiedad de Azure](https://www.microsoft.com/download/details.aspx?id=41653). | Ambos | No | No |
| **LogicApps** | Logic Apps. | Ambos | No | No |
| **LogicAppsManagement** | Tráfico de administración para Logic Apps. | Entrada | No | No |
| **MicrosoftCloudAppSecurity** | Microsoft Cloud App Security. | Salida | No | No |
| **MicrosoftContainerRegistry** | Registro de contenedor para imágenes de contenedor de Microsoft. <br/><br/>*Nota:* Esta etiqueta presenta dependencia con la etiqueta **AzureFrontDoor.FirstParty**. | Salida | Sí | Sí |
| **PowerQueryOnline** | Power Query Online. | Ambos | No | No |
| **Service Bus** | Tráfico de Azure Service Bus que usa el nivel de servicio Premium. | Salida | Sí | Sí |
| **ServiceFabric** | Azure Service Fabric.<br/><br/>*Nota:* Esta etiqueta representa el punto de conexión de servicio de Service Fabric para el plano de control por región. Esto permite a los clientes realizar operaciones de administración para sus clústeres de Service Fabric desde la red virtual (punto de conexión, p. ej. https:// westus.servicefabric.azure.com) | Ambos | No | No |
| **Sql** | Azure SQL Database, Azure Database for MySQL, Azure Database for PostgreSQL y Azure SQL Data Warehouse.<br/><br/>*Nota:* Esta etiqueta representa el servicio, no instancias específicas del mismo. Por ejemplo, la etiqueta representa el servicio Azure SQL Database, pero no una cuenta de un servidor o base de datos SQL específicos. Esta etiqueta no se aplica a ninguna instancia administrada de SQL. | Salida | Sí | Sí |
| **SqlManagement** | Tráfico de administración para implementaciones dedicadas de SQL. | Ambos | No | Sí |
| **Storage** | Azure Storage. <br/><br/>*Nota:* Esta etiqueta representa el servicio, no instancias específicas del mismo. Por ejemplo, la etiqueta representa el servicio Azure Storage, pero no una cuenta de específica de este. | Salida | Sí | Sí |
| **StorageSyncService** | Servicio de sincronización de almacenamiento. | Ambos | No | No |
| **WindowsVirtualDesktop** | Windows Virtual Desktop. | Ambos | No | No |
| **VirtualNetwork** | El espacio de direcciones de red virtual (todos los intervalos de direcciones IP definidos para la red virtual), todos los espacios de direcciones locales conectados, las redes virtuales [del mismo nivel](virtual-network-peering-overview.md), las redes virtuales conectadas a una [puerta de enlace de red virtual](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%3ftoc.json), la [dirección IP virtual del host](security-overview.md#azure-platform-considerations) y los prefijos de dirección usados en las [rutas definidas por el usuario](virtual-networks-udr-overview.md). Esta etiqueta también puede contener rutas predeterminadas. | Ambos | No | No |

>[!NOTE]
>En el modelo de implementación clásico (antes de Azure Resource Manager), se admite un subconjunto de las etiquetas enumeradas en la tabla anterior. Estas etiquetas se escriben de forma diferente:
>
>| Ortografía clásica | Etiqueta de Resource Manager equivalente |
>|---|---|
>| AZURE_LOADBALANCER | AzureLoadBalancer |
>| INTERNET | Internet |
>| VIRTUAL_NETWORK | VirtualNetwork |

> [!NOTE]
> Las etiquetas de servicios de los servicios de Azure indican los prefijos de dirección de la nube específica que se va a usar. Por ejemplo, los intervalos de direcciones IP subyacentes que se corresponden con el valor de la etiqueta de **Sql** en la nube pública de Azure serán distintos de los intervalos subyacentes en la nube de Azure China.

> [!NOTE]
> Si implementa un [punto de conexión de servicio de red virtual](virtual-network-service-endpoints-overview.md) para un servicio, como Azure Storage o Azure SQL Database, Azure agrega una [ruta](virtual-networks-udr-overview.md#optional-default-routes) a una subred de red virtual para el servicio. Los prefijos de dirección de la ruta son los mismos prefijos de dirección, o intervalos CIDR, que los de la etiqueta de servicio correspondiente.

## <a name="service-tags-on-premises"></a>Etiquetas de servicio en un entorno local  
Puede obtener información de intervalo y la etiqueta de servicio actual para incluirla como parte de las configuraciones del firewall local. Esta información es la lista actual en un momento dado de los intervalos de direcciones IP que corresponden a cada etiqueta de servicio. Puede obtener la información mediante programación o a través de la descarga de un archivo JSON, tal y como se describe en las secciones siguientes.

### <a name="use-the-service-tag-discovery-api-public-preview"></a>Uso de Service Tag Discovery API (versión preliminar pública)
Puede recuperar mediante programación la lista actual de etiquetas de servicio, junto con los detalles del intervalo de direcciones IP:

- [REST](https://docs.microsoft.com/rest/api/virtualnetwork/servicetags/list)
- [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.network/Get-AzNetworkServiceTag?view=azps-2.8.0&viewFallbackFrom=azps-2.3.2)
- [CLI de Azure](https://docs.microsoft.com/cli/azure/network?view=azure-cli-latest#az-network-list-service-tags)

> [!NOTE]
> Si bien se encuentra en versión preliminar pública, Discovery API podría devolver información menos actual que la información devuelta por las descargas de JSON. (Consulte la sección siguiente).


### <a name="discover-service-tags-by-using-downloadable-json-files"></a>Detección de etiquetas de servicio mediante archivos JSON descargables 
Puede descargar archivos JSON que contengan la lista actual de etiquetas de servicio, junto con los detalles del intervalo de direcciones IP. Estas listas se actualizan y publican semanalmente. Las ubicaciones de cada nube son:

- [Azure público](https://www.microsoft.com/download/details.aspx?id=56519)
- [Azure US Government](https://www.microsoft.com/download/details.aspx?id=57063)  
- [Azure en China](https://www.microsoft.com/download/details.aspx?id=57062) 
- [Azure Alemania](https://www.microsoft.com/download/details.aspx?id=57064)   

> [!NOTE]
>Un subconjunto de esta información se ha publicado en archivos XML para [Azure público](https://www.microsoft.com/download/details.aspx?id=41653), [Azure China](https://www.microsoft.com/download/details.aspx?id=42064) y [Azure Alemania](https://www.microsoft.com/download/details.aspx?id=54770). Estas descargas XML dejarán de usarse en el 30 de junio de 2020 y ya no estarán disponibles después de esa fecha. Debería migrar mediante Discovery API o las descargas de archivos JSON como se describió en las secciones anteriores.

### <a name="tips"></a>Sugerencias 
- Puede detectar actualizaciones de una publicación a la siguiente si observa los valores *changeNumber* en aumento en el archivo JSON. Cada subsección (por ejemplo, **Storage.WestUS**) tiene su propio *changeNumber* que se incrementa a medida que se producen cambios. El nivel superior de *changeNumber* del archivo aumenta cuando alguna de las subsecciones ha cambiado.
- Para ver ejemplos de cómo analizar la información de la etiqueta de servicio (por ejemplo, obtener todos los intervalos de direcciones para Storage en Oeste de EE. UU.), consulte la documentación de [PowerShell de Service Tag Discovery API](https://aka.ms/discoveryapi_powershell).

## <a name="next-steps"></a>Pasos siguientes
- Aprenda a [crear un grupo de seguridad de red](tutorial-filter-network-traffic.md).
