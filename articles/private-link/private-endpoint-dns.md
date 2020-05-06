---
title: Configuración de DNS para puntos de conexión privados de Azure
description: Conozca la configuración de DNS para puntos de conexión privados de Azure
services: private-link
author: mblanco77
ms.service: private-link
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: allensu
ms.openlocfilehash: 477a5ffa971120d1a98c09ac4ae8ebda1c82b770
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82209033"
---
# <a name="azure-private-endpoint-dns-configuration"></a>Configuración de DNS para puntos de conexión privados de Azure


Al conectarse a un recurso de vínculo privado mediante un nombre de dominio completo (FQDN) como parte de la cadena de conexión, es importante establecer correctamente la configuración de DNS para que se resuelva en la dirección IP privada asignada. Es posible que los servicios de Azure existentes ya tengan una configuración de DNS que usar al conectarse a través de un punto de conexión público. Este se debe invalidar para conectarse con el punto de conexión privado. 
 
La interfaz de red asociada con el punto de conexión privado contiene el conjunto completo de información necesaria para configurar el DNS, incluidos el FQDN y las direcciones IP privadas asignadas a un recurso de vínculo privado determinado. 
 
Puede usar las siguientes opciones para establecer la configuración de DNS para los puntos de conexión privados: 
- **Use el archivo de host (solo se recomienda para pruebas)** . Puede usar el archivo de host en una máquina virtual para invalidar el DNS.  
- **Use una zona DNS privada**. Puede usar [zonas DNS privadas](../dns/private-dns-privatednszone.md) para invalidar la resolución DNS de un punto de conexión privado determinado. Una zona DNS privada se puede vincular a la red virtual para resolver dominios específicos.
- **Use el servidor DNS personalizado**. Puede usar su propio servidor DNS para invalidar la resolución DNS para un recurso de vínculo privado determinado. Si el [servidor DNS](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server) se hospeda en una red virtual, puede crear una regla de reenvío de DNS para usar una zona DNS privada con el fin de simplificar la configuración de todos los recursos de vínculo privado.
 
> [!IMPORTANT]
> No se recomienda invalidar una zona que esté en uso activamente para resolver puntos de conexión públicos. Las conexiones a los recursos no podrán resolverse correctamente sin el reenvío de DNS al DNS público. Para evitar problemas, cree un nombre de dominio diferente o siga el nombre sugerido para cada servicio que aparece a continuación. 

## <a name="azure-services-dns-zone-configuration"></a>Configuración de zonas DNS de los servicios de Azure
Los servicios de Azure crearán un registro de DNS de nombre canónico (CNAME) en el DNS público para redirigir la resolución a los nombres de dominio privados sugeridos. Podrá invalidar la resolución con la dirección IP privada de los puntos de conexión privados. 
 
No es necesario que las aplicaciones cambien la dirección URL de conexión. Al intentar resolver mediante un DNS público, el servidor DNS se resolverá ahora en los puntos de conexión privados. El proceso no afecta a las aplicaciones existentes. 

En el caso de los servicios de Azure, use los nombres de zona recomendados tal y como se describe en la tabla siguiente:

|Nombre del recurso de Private Link   |Subrecurso  |Nombre de zona  |
|---------|---------|---------|
|SQL DB (Microsoft.Sql/servers)    |  Sql Server (sqlServer)        |   privatelink.database.windows.net       |
|Azure Synapse Analytics (Microsoft.Sql/servers)    |  Sql Server (sqlServer)        | privatelink.database.windows.net |
|Cuenta de almacenamiento (Microsoft.Storage/storageAccounts)    |  Blob (blob, blob_secondary)        |    privatelink.blob.core.windows.net      |
|Cuenta de almacenamiento (Microsoft.Storage/storageAccounts)    |    Tabla (table, table_secondary)      |   privatelink.table.core.windows.net       |
|Cuenta de almacenamiento (Microsoft.Storage/storageAccounts)    |    Cola (queue, queue_secondary)     |   privatelink.queue.core.windows.net       |
|Cuenta de almacenamiento (Microsoft.Storage/storageAccounts)   |    Archivo (file, file_secondary)      |    privatelink.file.core.windows.net      |
|Cuenta de almacenamiento (Microsoft.Storage/storageAccounts)     |  Web (web, web_secondary)        |    privatelink.web.core.windows.net      |
|Sistema de archivos Data Lake Gen2 (Microsoft.Storage/storageAccounts)  |  Sistema de archivos Data Lake Gen2 (dfs, dfs_secondary)        |     privatelink.dfs.core.windows.net     |
|Azure Cosmos DB (Microsoft.AzureCosmosDB/databaseAccounts)|SQL    |privatelink.documents.azure.com|
|Azure Cosmos DB (Microsoft.AzureCosmosDB/databaseAccounts)|MongoDB    |privatelink.mongo.cosmos.azure.com|
|Azure Cosmos DB (Microsoft.AzureCosmosDB/databaseAccounts)|Cassandra|privatelink.cassandra.cosmos.azure.com|
|Azure Cosmos DB (Microsoft.AzureCosmosDB/databaseAccounts)|Gremlin    |privatelink.gremlin.cosmos.azure.com|
|Azure Cosmos DB (Microsoft.AzureCosmosDB/databaseAccounts)|Tabla|privatelink.table.cosmos.azure.com|
|Azure Database for PostgreSQL: servidor único (Microsoft.DBforPostgreSQL/servers)|postgresqlServer|privatelink.postgres.database.azure.com|
|Azure Database for MySQL (Microsoft.DBforMySQL/servers)|mysqlServer|privatelink.mysql.database.azure.com|
|Azure Database for MariaDB (Microsoft.DBforMariaDB/servers)|mariadbServer|privatelink.mariadb.database.azure.com|
|Azure Key Vault (Microsoft.KeyVault/vaults)|almacén|privatelink.vaultcore.azure.net|
|Azure Kubernetes Service: API de Kubernetes (Microsoft.ContainerService/managedClusters)    | managedCluster | {guid}.privatelink.{región}.azmk8s.io|
|Azure Search (Microsoft.Search/searchServices)|searchService|privatelink.search.windows.net|   
|Azure Container Registry (Microsoft.ContainerRegistry/registries) | Registro | privatelink.azurecr.io |
|Azure App Configuration (Microsoft.Appconfiguration/configurationStores)| configurationStore | privatelink.azconfig.io|
|Azure Backup (Microsoft.RecoveryServices/vaults)| almacén |privatelink.{region}.backup.windowsazure.com|
|Centro de eventos de Azure (Microsoft.EventHub/namespaces)| espacio de nombres |privatelink.servicebus.windows.net|
|Azure Service Bus (Microsoft.ServiceBus/namespaces) | espacio de nombres |privatelink.servicebus.windows.net|
|Azure Relay (Microsoft.Relay/namespaces) | espacio de nombres |privatelink.servicebus.windows.net|
|Azure Event Grid (Microsoft.EventGrid/topics)     | topic | topic.{region}.privatelink.eventgrid.azure.net|
|Azure Event Grid (Microsoft.EventGrid/domains) | dominio | domain.{region}.privatelink.eventgrid.azure.net |
|Azure Web Apps (Microsoft.Web/sites)    | site | privatelink.azurewebsites.net |
|Azure Machine Learning (Microsoft.MachineLearningServices/workspaces)    | área de trabajo | privatelink.api.azureml.ms |
 


## <a name="dns-configuration-scenarios"></a>Escenarios de configuración de DNS

El FQDN de los servicios resuelve una dirección IP pública, por lo que debe cambiar la configuración de DNS para resolver la dirección IP privada del punto de conexión privado.

DNS es un componente fundamental para que la aplicación funcione correctamente al resolver de forma correcta la dirección IP del punto de conexión privado.

En función de sus preferencias, los siguientes escenarios están disponibles para la resolución DNS integrada:

- [Cargas de trabajo de red virtual sin un servidor DNS personalizado](#virtual-network-workloads-without-custom-dns-server)


## <a name="virtual-network-workloads-without-custom-dns-server"></a>Cargas de trabajo de red virtual sin un servidor DNS personalizado

Esta configuración es adecuada para cargas de trabajo de red virtual sin un servidor DNS personalizado. En este escenario, el cliente consulta la dirección IP del punto de conexión privado en el DNS proporcionado de Azure [168.63.129.16](../virtual-network/what-is-ip-address-168-63-129-16.md). Azure DNS será responsable de la resolución DNS de las zonas DNS privadas.


 > [!NOTE]
> En este escenario se usa la zona DNS privada recomendada de Azure SQL Database. Para otros servicios, puede ajustar el modelo mediante la siguiente referencia [Configuración de zonas DNS de los servicios de Azure](#azure-services-dns-zone-configuration).

Para realizar la configuración correctamente, necesitaría los siguientes recursos:

- Red virtual de cliente

- Zona DNS privada [privatelink.database.windows.net](../dns/private-dns-privatednszone.md) con [registro tipo A](../dns/dns-zones-records.md#record-types)

- Información del punto de conexión privado (nombre del registro FQDN y dirección IP privada)

En el diagrama siguiente se muestra la secuencia de la resolución DNS de cargas de trabajo de red virtual que usan la zona DNS privada.

:::image type="content" source="media/private-endpoint-dns/single-vnet-azure-dns.png" alt-text="red virtual única y DNS proporcionada por Azure":::

Este modelo se puede extender a varias redes virtuales del mismo nivel que están asociadas al mismo punto de conexión privado. Esto puede realizarse con la [adición de nuevos vínculos de red virtual](../dns/private-dns-virtual-network-links.md) a la zona DNS privada para todas las redes virtuales del mismo nivel.

 > [!IMPORTANT]
>  Se requiere una única zona DNS privada para esta configuración. La creación de varias zonas con el mismo nombre para diferentes redes virtuales necesitaría operaciones manuales para combinar los registros de DNS.

En este escenario, hay una topología de red en estrella tipo [hub-and-spoke](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) con las redes radiales que comparten un punto de conexión privado común y todas las redes virtuales radiales están vinculadas a la misma zona DNS privada. 

:::image type="content" source="media/private-endpoint-dns/hub-and-spoke-azure-dns.png" alt-text="hub-and-spoke con el DNS proporcionado por Azure":::


## <a name="next-steps"></a>Pasos siguientes
- [Más información sobre los puntos de conexión privados](private-endpoint-overview.md)
