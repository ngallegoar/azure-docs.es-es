---
title: Indexadores que acceden a recursos seguros a través de puntos de conexión privados
titleSuffix: Azure Cognitive Search
description: En esta guía de procedimientos se describe la configuración de puntos de conexión privados para que los indexadores se comuniquen con recursos seguros.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/07/2020
ms.openlocfilehash: e9944e95dd452cd00e63280ad8002141591f521e
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2020
ms.locfileid: "90971422"
---
# <a name="accessing-secure-resources-via-private-endpoints"></a>Acceso a recursos seguros a través de puntos de conexión privados

Los recursos de Azure (como las cuentas de almacenamiento que se usan como orígenes de datos) se pueden configurar de modo que solo se pueda acceder a ellos desde una lista específica de redes virtuales. También se pueden configurar para que no se permita el acceso desde ninguna "red pública".
Los clientes pueden solicitar a Azure Cognitive Search que cree una [conexión de punto de conexión privado](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) (de salida) para acceder de forma segura a los datos de estos orígenes de datos a través de indexadores.

## <a name="shared-private-link-resources-management-apis"></a>API de administración de recursos de vínculo privado compartido

Los puntos de conexión privados que se crean en Azure Cognitive Search a petición de un cliente para acceder a recursos "seguros" se conocen como *recursos de vínculo privado compartido*. El cliente "comparte" el acceso a un recurso (por ejemplo, una cuenta de almacenamiento) que se ha incorporado al [servicio Azure Private Link](https://azure.microsoft.com/services/private-link/).

Azure Cognitive Search ofrece, a través de la API de administración de búsqueda, la capacidad de [crear o actualizar recursos de vínculo privado compartido](https://docs.microsoft.com/rest/api/searchmanagement/sharedprivatelinkresources/createorupdate). Usará esta API junto con otras API de administración de *recursos de vínculo privado compartido* para configurar el acceso a un recurso seguro desde un indexador de Azure Cognitive Search.

Las conexiones de punto de conexión privado a algunos recursos solo se pueden crear a través de la versión preliminar de la API de administración de búsqueda (`2020-08-01-Preview`), que se indica con la etiqueta "versión preliminar" en la tabla siguiente. Los recursos sin etiqueta "versión preliminar" se pueden crear tanto a través de la API de vista previa como de la API con disponibilidad general (`2020-08-01`).

A continuación se muestra la lista de recursos de Azure a los que se pueden crear puntos de conexión privados de salida desde Azure Cognitive Search. Para crear un recurso de vínculo privado compartido, el valor de `groupId` de la tabla siguiente debe usarse exactamente así (con distinción de mayúsculas y minúsculas) en la API.

| Recurso de Azure | Identificador de grupo |
| --- | --- |
| Azure Storage: Blob (o) ADLS Gen2 | `blob`|
| Azure Storage: Tablas | `table`|
| Azure Cosmos DB: SQL API | `Sql`|
| Azure SQL Database | `sqlServer`|
| Azure Database for MySQL (versión preliminar) | `mysqlServer`|
| Azure Key Vault | `vault` |
| Azure Functions (versión preliminar) | `sites` |

La lista de recursos de Azure para los que se admiten las conexiones de punto de conexión privado salientes también se puede consultar a través de la [lista de API compatibles](https://docs.microsoft.com/rest/api/searchmanagement/privatelinkresources/listsupported).

Para los fines de esta guía, se usa una combinación de [ARMClient](https://github.com/projectkudu/ARMClient) y [Postman](https://www.postman.com/) para mostrar las llamadas a API REST.

> [!NOTE]
> A lo largo de esta guía, supondremos que el nombre del servicio de búsqueda es __contoso-search__, que existe en el grupo de recursos __contoso__ de una suscripción con el identificador __00000000-0000-0000-0000-000000000000__. El identificador de recurso de este servicio de búsqueda será `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search`.

En el resto de la guía se mostrará cómo se puede configurar el servicio __contoso-search__ para que sus indexadores puedan acceder a los datos de la cuenta de almacenamiento segura `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Storage/storageAccounts/contoso-storage`.

## <a name="securing-your-storage-account"></a>Protección de la cuenta de almacenamiento

Puede configurar la cuenta de almacenamiento para [permitir el acceso solo desde subredes específicas](https://docs.microsoft.com/azure/storage/common/storage-network-security#grant-access-from-a-virtual-network). Si en Azure Portal activa esta opción y deja el conjunto vacío, significa que no se permite el tráfico desde ninguna red virtual.

   ![Acceso de redes virtuales](media\search-indexer-howto-secure-access\storage-firewall-noaccess.png "Acceso de redes virtuales")

> [!NOTE]
> Puede usarse el [enfoque de servicio de Microsoft de confianza](https://docs.microsoft.com/azure/storage/common/storage-network-security#trusted-microsoft-services) para omitir las restricciones de red virtual o IP en dicha cuenta de almacenamiento y permitir que el servicio de búsqueda acceda a los datos de la cuenta de almacenamiento tal y como se describe en la [guía de procedimientos](search-indexer-howto-access-trusted-service-exception.md). Sin embargo, cuando se usa este enfoque, la comunicación entre Azure Cognitive Search y la cuenta de almacenamiento se produce mediante la dirección IP pública de la cuenta de almacenamiento, a través de la red troncal de Microsoft segura.

## <a name="step-1-create-a-shared-private-link-resource-to-the-storage-account"></a>Paso 1: Crear un recurso de vínculo privado compartido a la cuenta de almacenamiento

Realice la siguiente llamada API para solicitar a Azure Cognitive Search que cree una conexión de punto de conexión privado de salida con la cuenta de almacenamiento.

`armclient PUT https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search/sharedPrivateLinkResources/blob-pe?api-version=2020-08-01 create-pe.json`

El contenido del archivo `create-pe.json` (que representa el cuerpo de la solicitud a la API) es el siguiente:

```json
{
      "name": "blob-pe",
      "properties": {
        "privateLinkResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Storage/storageAccounts/contoso-storage",
        "groupId": "blob",
        "requestMessage": "please approve"
      }
}
```

Se devuelve una respuesta `202 Accepted` en caso de éxito. El proceso de creación de un punto de conexión privado de salida es una operación de larga duración (asincrónica), que implica la implementación de los siguientes recursos:

1. Un punto de conexión privado asignado con una dirección IP privada con un estado `"Pending"`. La dirección IP privada se obtiene del espacio de direcciones asignado a la red virtual del entorno de ejecución del indexador privado específico del servicio de búsqueda. Tras la aprobación del punto de conexión privado, cualquier comunicación desde Azure Cognitive Search a la cuenta de almacenamiento se origina desde la dirección IP privada y un canal seguro de vínculo privado.
2. Una zona DNS privada para el tipo de recurso, que se basa en el valor de `groupId`. Esto garantiza que cualquier búsqueda DNS en el recurso privado usa la dirección IP asociada con el punto de conexión privado.

Asegúrese de especificar el valor de `groupId` correcto para el tipo de recurso para el que va a crear el punto de conexión privado. Cualquier discrepancia producirá un mensaje de respuesta incorrecta.

Al igual que todas las operaciones asincrónicas de Azure, la llamada a `PUT` devuelve un valor de encabezado `Azure-AsyncOperation` que tendrá el aspecto siguiente:

`"Azure-AsyncOperation": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search/sharedPrivateLinkResources/blob-pe/operationStatuses/08586060559526078782?api-version=2020-08-01"`

Este URI se puede sondear periódicamente para obtener el estado de la operación. Se recomienda esperar hasta que el estado de la operación de recurso de vínculo privado compartido haya alcanzado un estado terminal (es decir, `succeeded`) antes de continuar.

`armclient GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search/sharedPrivateLinkResources/blob-pe/operationStatuses/08586060559526078782?api-version=2020-08-01"`

```json
{
    "status": "running" | "succeeded" | "failed"
}
```

## <a name="step-2a-approve-the-private-endpoint-connection-for-the-storage-account"></a>Paso 2a: Aprobar la conexión del punto de conexión privado para la cuenta de almacenamiento

> [!NOTE]
> En esta sección se usa Azure Portal para recorrer el flujo de aprobación de un punto de conexión privado al almacenamiento. También se puede usar en su lugar la [API REST](https://docs.microsoft.com/rest/api/storagerp/privateendpointconnections) disponible a través del proveedor de recursos de almacenamiento.
>
> Otros proveedores como CosmosDB, Azure SQL Server, etc., también ofrecen API de proveedor de recursos similares para administrar conexiones de punto de conexión privado.

Vaya a la pestaña "**Conexiones de punto de conexión privado**" de la cuenta de almacenamiento en Azure Portal. Debe haber una solicitud para una conexión de punto de conexión privado con el mensaje de solicitud de la llamada API anterior (una vez que la operación asincrónica ha finalizado __correctamente__).

   ![Aprobación del punto de conexión privado](media\search-indexer-howto-secure-access\storage-privateendpoint-approval.png "Aprobación del punto de conexión privado")

Seleccione el punto de conexión privado que creó Azure Cognitive Search (use la columna "Punto de conexión privado" para identificar la conexión de punto de conexión privado por el nombre especificado en la API anterior) y elija "Aprobar", con un mensaje adecuado (el mensaje no es significativo). Asegúrese de que la conexión de punto de conexión privado aparece de la siguiente manera (el estado puede tardar entre 1 o 2 minutos en actualizarse en el portal).

![Punto de conexión privado aprobado](media\search-indexer-howto-secure-access\storage-privateendpoint-after-approval.png "Punto de conexión privado aprobado")

Cuando se aprueba la solicitud de conexión de punto de conexión privado, el tráfico *es capaz* de fluir a través del punto de conexión privado. Una vez que se aprueba el punto de conexión privado, Azure Cognitive Search creará las asignaciones de zona DNS necesarias en la zona DNS que se crea para él.

## <a name="step-2b-query-the-status-of-the-shared-private-link-resource"></a>Paso 2b: Consultar el estado del recurso de vínculo privado compartido

 Para confirmar que el recurso de vínculo privado compartido se ha actualizado después de la aprobación, obtenga su estado mediante [GET API](https://docs.microsoft.com/rest/api/searchmanagement/sharedprivatelinkresources/get).

`armclient GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search/sharedPrivateLinkResources/blob-pe?api-version=2020-08-01`

Si el valor `properties.provisioningState` del recurso es `Succeeded` y el valor de `properties.status` es `Approved`, significa que el recurso de vínculo privado compartido es funcional y los indexadores se pueden configurar para que se comuniquen a través del punto de conexión privado.

```json
{
      "name": "blob-pe",
      "properties": {
        "privateLinkResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Storage/storageAccounts/contoso-storage",
        "groupId": "blob",
        "requestMessage": "please approve",
        "status": "Approved",
        "resourceRegion": null,
        "provisioningState": "Succeeded"
      }
}

```

## <a name="step-3-configure-indexer-to-run-in-the-private-environment"></a>Paso 3: Configurar el indexador para que se ejecute en el entorno privado

> [!NOTE]
> Este paso se puede realizar incluso antes de que se apruebe la conexión del punto de conexión privado. Hasta que se apruebe la conexión del punto de conexión privado, cualquier indexador que intente comunicarse con un recurso seguro (como la cuenta de almacenamiento) terminará con un estado de error transitorio. Los nuevos indexadores no se crearán. En cuanto se apruebe la conexión del punto de conexión privado, los indexadores podrán acceder a la cuenta de almacenamiento privada.

1. [Cree un origen de datos](https://docs.microsoft.com/rest/api/searchservice/create-data-source) que apunte a la cuenta de almacenamiento segura y a un contenedor adecuado dentro de la cuenta de almacenamiento. A continuación se muestra esta solicitud realizada a través de Postman.
![Create Data Source](media\search-indexer-howto-secure-access\create-ds.png "Creación del origen de datos") (Creación de un origen de datos)

2. De forma similar [cree un índice](https://docs.microsoft.com/rest/api/searchservice/create-index) y, opcionalmente, [cree un conjunto de aptitudes](https://docs.microsoft.com/rest/api/searchservice/create-skillset) mediante la API REST.

3. [Cree un indexador](https://docs.microsoft.com/rest/api/searchservice/create-indexer) que apunte al origen de datos, el índice y el conjunto de aptitudes creados anteriormente. Además, fuerce el indexador para que se ejecute en el entorno de ejecución privado; para ello, establezca la propiedad de configuración del indexador `executionEnvironment` en `"Private"`.
![Crear indexador](media\search-indexer-howto-secure-access\create-idr.png "Creación del indexador")

El indexador debe crearse correctamente y debe progresar, indexando contenido de la cuenta de almacenamiento a través de la conexión del punto de conexión privado. El estado del indexador se puede supervisar a través de la [API de estado del indexador](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status).

> [!NOTE]
> Si ya tiene indexadores, simplemente puede actualizarlos a través de [PUT API](https://docs.microsoft.com/rest/api/searchservice/create-indexer) para establecer el valor de `executionEnvironment` en `"Private"`.

## <a name="troubleshooting-issues"></a>Solución de problemas

- Si se produce un error al crear un indexador con un mensaje donde se indica que las credenciales del origen de datos no son válidas, significa que el punto de conexión privado no tiene el estado *Aprobado* o que no es funcional.
Obtenga el estado del recurso de vínculo privado compartido mediante [GET API](https://docs.microsoft.com/rest/api/searchmanagement/sharedprivatelinkresources/get). Si se ha *aprobado*, compruebe el valor `properties.provisioningState` del recurso. Si es `Incomplete`, significa que algunas de las dependencias subyacentes del recurso no se pudieron aprovisionar; emitir de nuevo la solicitud `PUT` para "volver a crear" el recurso de vínculo privado compartido debería corregir el problema. Es posible que sea necesaria una nueva aprobación; compruebe el estado del recurso una vez más para comprobarlo.
- Si el indexador se crea sin establecer su valor de `executionEnvironment`, el proceso podría finalizar correctamente, pero su historial de ejecución mostrará que sus ejecuciones no son correctas. Debe [actualizar el indexador](https://docs.microsoft.com/rest/api/searchservice/update-indexer) para especificar el entorno de ejecución.
- Si el indexador se crea sin establecer el valor de `executionEnvironment` y se ejecuta correctamente, significa que Azure Cognitive Search ha decidido que su entorno de ejecución es el entorno "privado" específico del servicio de búsqueda. Sin embargo, esto puede cambiar en función de una serie de factores (recursos consumidos por el indexador, la carga en el servicio de búsqueda, etc.) y se puede producir un error más adelante; por tanto, se recomienda establecer el valor de `executionEnvironment` como `"Private"` para asegurarse de que no se producirá ningún error en el futuro.
- [Cuotas y límites](search-limits-quotas-capacity.md) determinan el número de recursos de vínculo privado compartido que se pueden crear y que pueden depender de la SKU del servicio de búsqueda.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre los puntos de conexión privados:

- [¿Qué son los puntos de conexión privados?](https://docs.microsoft.com/azure/private-link/private-endpoint-overview)
- [Configuraciones de DNS necesarias para los puntos de conexión privados](https://docs.microsoft.com/azure/private-link/private-endpoint-dns)