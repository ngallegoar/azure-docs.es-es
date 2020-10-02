---
title: Acceso seguro a los recursos del indexador
titleSuffix: Azure Cognitive Search
description: Información general conceptual de las opciones de seguridad de nivel de red para el acceso a datos de Azure mediante indexadores en Azure Cognitive Search.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/07/2020
ms.openlocfilehash: 5075c4858f9584cb19442e19d9009d46d0e00ff8
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2020
ms.locfileid: "89463353"
---
# <a name="indexer-access-to-data-sources-using-azure-network-security-features"></a>Acceso del indexador a orígenes de datos mediante las características de seguridad de red de Azure

Los indexadores de Azure Cognitive Search pueden realizar llamadas salientes a varios recursos de Azure durante la ejecución. En este artículo se explican los conceptos relacionados con el acceso del indexador a los recursos cuando estos recursos están protegidos por firewalls de IP, puntos de conexión privados y otros mecanismos de seguridad de nivel de red. En la tabla siguiente se enumeran los posibles tipos de recursos a los que puede tener acceso un indexador en una ejecución habitual.

| Recurso | Propósito dentro de la ejecución del indexador |
| --- | --- |
| Azure Storage (blobs, tablas, ADLS Gen 2) | Origen de datos |
| Azure Storage (blobs, tablas) | Conjuntos de aptitudes (almacenamiento en caché de documentos enriquecidos y almacenamiento de proyecciones del almacén de conocimiento) |
| Azure Cosmos DB (varias API) | Origen de datos |
| Azure SQL Database | Origen de datos |
| SQL Server en máquinas virtuales IaaS de Azure | Origen de datos |
| Instancias administradas de SQL | Origen de datos |
| Azure Functions | Host para aptitudes de API web personalizadas |
| Cognitive Services | Está asociado al conjunto de habilidades que se usará para facturar el enriquecimiento por encima del límite de 20 documentos gratuitos |

> [!NOTE]
> El recurso de servicio cognitivo asociado a un conjunto de habilidades se usa para la facturación, en función de los enriquecimientos realizados y escritos en el índice de búsqueda. No se utiliza para el acceso a las API de Cognitive Services. El acceso desde la canalización de enriquecimiento de un indexador hasta las API de Cognitive Services se produce a través de un canal de comunicación seguro, donde los datos en tránsito están fuertemente cifrados y nunca se almacenan en reposo.

Los clientes pueden proteger estos recursos a través de varios mecanismos de aislamiento de red que ofrece Azure. A excepción del recurso de servicio cognitivo, los indexadores tienen una capacidad limitada para acceder a todos los demás recursos, incluso si están aislados de la red, como se describe en la tabla siguiente.

| Recurso | Restricción de IP | Punto de conexión privado |
| --- | --- | ---- |
| Azure Storage (blobs, tablas, ADLS Gen 2) | Solo se admite si la cuenta de almacenamiento y el servicio de búsqueda se encuentran en regiones diferentes. | Compatible |
| Azure Cosmos DB: SQL API | Compatible | Compatible |
| Azure Cosmos DB: Cassandra, Mongo y Gremlin API | Compatible | No compatible |
| Azure SQL Database | Compatible | Compatible |
| SQL Server en máquinas virtuales IaaS de Azure | Compatible | N/D |
| Instancias administradas de SQL | Compatible | N/D |
| Azure Functions | Compatible | Se admite solo para determinadas SKU de Azure Functions. |

> [!NOTE]
> Además de las opciones enumeradas anteriormente, en el caso de las cuentas de Azure Storage protegidas por red, los clientes pueden aprovechar el hecho de que Azure Cognitive Search es un [servicio de Microsoft de confianza](https://docs.microsoft.com/azure/storage/common/storage-network-security#trusted-microsoft-services). Esto significa que un servicio de búsqueda específico puede omitir las restricciones de red virtual o IP sobre la cuenta de almacenamiento y puede acceder a los datos que esta contiene si tiene habilitado el control de acceso basado en rol adecuado. Se puede encontrar más información en la [guía de procedimientos](search-indexer-howto-access-trusted-service-exception.md). Esta opción puede usarse en lugar de la ruta de restricciones de IP; en cualquier caso, la cuenta de almacenamiento o el servicio de búsqueda no se puedan mover a otra región.

A la hora de elegir qué mecanismo de acceso seguro debe utilizar un indexador, tenga en cuenta las siguientes restricciones:

- Los [puntos de conexión de servicio](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) no se admitirán en ningún recurso de Azure.
- No se puede aprovisionar un servicio de búsqueda en una red virtual específica: Azure Cognitive Search no ofrece esta funcionalidad.
- Cuando los indexadores usan puntos de conexión privados (salientes) para acceder a los recursos, se pueden aplicar [cargos de vínculo privado](https://azure.microsoft.com/pricing/details/search/) adicionales.

## <a name="indexer-execution-environment"></a>Entorno de ejecución de los indexadores

Los indexadores de Azure Cognitive Search son capaces de extraer contenido de orígenes de datos de forma eficaz, agregar enriquecimientos al contenido extraído y, opcionalmente, generar proyecciones antes de escribir los resultados en el índice de búsqueda. En función del número de responsabilidades asignadas a un indexador, puede ejecutarse en dos entornos:

- Un entorno privado para un servicio de búsqueda específico. Los indexadores que se ejecutan en estos entornos comparten recursos con otras cargas de trabajo (por ejemplo, otra carga de trabajo de indexación o consulta iniciada por el cliente). Normalmente, solo los indexadores que no requieren muchos recursos (por ejemplo, no usan un conjunto de habilidades) se ejecutan en este entorno.
- Un entorno multiinquilino que hospeda los indexadores que consumen muchos recursos, como los que tienen un conjunto de habilidades. Este tipo de indexadores se ejecutan en este entorno para ofrecer un rendimiento óptimo y garantizar que los recursos del servicio de búsqueda estén disponibles para otras cargas de trabajo. Azure Cognitive Search administra y protege este entorno multiinquilino, sin costo adicional para el cliente.

Con cada ejecución de un indexador dado, Azure Cognitive Search determina el mejor entorno en el que ejecutarlo.

## <a name="granting-access-to-indexer-ip-ranges"></a>Concesión de acceso a los intervalos IP del indexador

Si el recurso al que intenta acceder el indexador está restringido a solo un determinado conjunto de intervalos IP, deberá expandir el conjunto para incluir los posibles intervalos IP desde los que se puede originar una solicitud del indexador. Como se indicó anteriormente, existen dos entornos posibles en los que se ejecutan los indexadores y de los que pueden originarse solicitudes de acceso. Tendrá que agregar las direcciones IP de __ambos__ entornos para que el acceso del indexador funcione.

- Para obtener la dirección IP del entorno privado específico del servicio de búsqueda, use `nslookup` (o `ping`) con el nombre de dominio completo (FQDN) del servicio de búsqueda. El nombre de dominio completo de un servicio de búsqueda en la nube pública sería, por ejemplo, `<service-name>.search.windows.net`. Esta información está disponible en Azure Portal.
- Las direcciones IP de los entornos multiinquilino están disponibles a través de la etiqueta de servicio `AzureCognitiveSearch`. Las [etiquetas de servicio de Azure](https://docs.microsoft.com/azure/virtual-network/service-tags-overview) tienen un intervalo de direcciones IP publicado para cada servicio, que está disponible a través de una [API de detección (versión preliminar)](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#use-the-service-tag-discovery-api-public-preview) o un [archivo JSON descargable](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#discover-service-tags-by-using-downloadable-json-files). En cualquier caso, los intervalos IP se desglosan por región: solo puede elegir aquellos asignados para la región en la que se aprovisiona su servicio de búsqueda.

En el caso de determinados orígenes de datos, la propia etiqueta de servicio se puede usar directamente en lugar de enumerar la lista de intervalos IP (la dirección IP del servicio de búsqueda se debe seguir usando explícitamente). Estos orígenes de datos restringen el acceso por medio de la configuración de una [regla de grupo de seguridad de red](https://docs.microsoft.com/azure/virtual-network/security-overview), que admite de forma nativa la adición de una etiqueta de servicio, a diferencia de las reglas de IP como las que ofrecen Azure Storage, CosmosDB, Azure SQL, etc. Los orígenes de datos que admiten la posibilidad de utilizar la etiqueta de servicio `AzureCognitiveSearch` directamente además de la dirección IP del servicio de búsqueda son:

- [SQL Server en máquinas virtuales IaaS](https://docs.microsoft.com/azure/search/search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers#restrict-access-to-the-azure-cognitive-search)

- [Instancias administradas de SQL](https://docs.microsoft.com/azure/search/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers#verify-nsg-rules)

Puede encontrar información detallada en la [guía de procedimientos](search-indexer-howto-access-ip-restricted.md).

## <a name="granting-access-via-private-endpoints"></a>Concesión de acceso a través de puntos de conexión privados

Los indexadores pueden utilizar [puntos de conexión privados](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) para acceder a los recursos que están bloqueados para la selección de redes virtuales o que no tienen habilitado el acceso público.
Esta funcionalidad solo está disponible para los servicios de pago, con límites en el número de puntos de conexión privados que se crean. Los detalles sobre los límites se documentan en la [página sobre los límites de Azure Search](search-limits-quotas-capacity.md).

### <a name="step-1-create-a-private-endpoint-to-the-secure-resource"></a>Paso 1: Creación de un punto de conexión privado al recurso seguro

Los clientes deben llamar a la operación de administración de búsqueda, [Creación o actualización de la API](https://docs.microsoft.com/rest/api/searchmanagement/sharedprivatelinkresources/createorupdate) de *recurso compartido de Private Link* con el fin de crear una conexión de punto de conexión privado a su recurso seguro (por ejemplo, una cuenta de almacenamiento). El tráfico que atraviesa esta conexión de punto de conexión privado (saliente) solo se originará en la red virtual que se encuentre en el entorno de ejecución "privado" del indexador específico del servicio de búsqueda.

Azure Cognitive Search validará que los llamadores de esta API tengan permisos para aprobar las solicitudes de conexión de punto de conexión privado al recurso seguro. Por ejemplo, si solicita una conexión de punto de conexión privado a una cuenta de almacenamiento a la que no tiene acceso, se rechazará esta llamada.

### <a name="step-2-approve-the-private-endpoint-connection"></a>Paso 2: Aprobación de la conexión del punto de conexión privado

Cuando se complete la operación (asincrónica) que crea un recurso de vínculo privado compartido, se creará una conexión de punto de conexión privado en un estado "pendiente". Todavía no fluye el tráfico por la conexión.
Se espera entonces que el cliente encuentre esta solicitud en su recurso seguro y la apruebe. Normalmente, esta operación se puede hacer a través del portal o la [API REST](https://docs.microsoft.com/rest/api/virtualnetwork/privatelinkservices/updateprivateendpointconnection).

### <a name="step-3-force-indexers-to-run-in-the-private-environment"></a>Paso 3: Forzar la ejecución de los indexadores en el entorno "privado"

Un punto de conexión privado aprobado permite llamadas salientes del servicio de búsqueda a un recurso que tenga algún tipo de restricciones de acceso de nivel de red (por ejemplo, un origen de datos de la cuenta de almacenamiento que está configurado para que solo se pueda acceder a él desde determinadas redes virtuales).
Esto significa que cualquier indexador que pueda llegar a un origen de datos así a través del punto de conexión privado se ejecutará correctamente.
Si el punto de conexión privado no está aprobado, o si el indexador no utiliza la conexión de punto de conexión privado, la ejecución del indexador terminará en `transientFailure`.

Para permitir que los indexadores accedan a los recursos a través de conexiones de punto de conexión privado, es obligatorio establecer el valor de `executionEnvironment` del indexador en `"Private"` a fin de garantizar que todas las ejecuciones del indexador puedan usar el punto de conexión privado. El motivo es que los puntos de conexión privados se aprovisionan dentro del entorno privado específico del servicio de búsqueda.

```json
    {
      "name" : "myindexer",
      ... other indexer properties
      "parameters" : {
          ... other parameters
          "configuration" : {
            ... other configuration properties
            "executionEnvironment": "Private"
          }
        }
    }
```

Estos pasos se describen con más detalle en la [guía de procedimientos](search-indexer-howto-access-private.md).
Una vez que tenga un punto de conexión privado aprobado a un recurso, los indexadores establecidos como *privados* intentarán acceder a través de la conexión de punto de conexión privado.

### <a name="limits"></a>Límites

Para garantizar un rendimiento y una estabilidad óptimos del servicio de búsqueda, se imponen restricciones (por SKU de servicio de búsqueda) sobre las dimensiones siguientes:

- Los tipos de indexadores que se pueden establecer como *privados*.
- El número de recursos de vínculo privado compartidos que se pueden crear.
- El número de tipos de recursos distintos para los que se pueden crear recursos de vínculo privado compartidos.

Estos límites se documentan en los [límites de servicio](search-limits-quotas-capacity.md).
