---
title: 'Problemas conocidos: Migración de MongoDB a Azure Cosmos DB'
titleSuffix: Azure Database Migration Service
description: Información acerca de las limitaciones y problemas conocidos de migración con las migraciones de MongoDB a Azure Cosmos DB mediante Azure Database Migration Service.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 02/27/2020
ms.openlocfilehash: 194da036260a78b27748dfc7f755212ab4f30b1e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78256027"
---
# <a name="known-issuesmigration-limitations-with-migrations-from-mongodb-to-azure-cosmos-dbs-api-for-mongodb"></a>Problemas conocidos/limitaciones de migración con las migraciones desde MongoDB a la API de Azure Cosmos DB para MongoDB

En las secciones siguientes se describen los problemas conocidos y las limitaciones asociadas a las migraciones de MongoDB a la API de Cosmos DB para MongoDB.

## <a name="migration-fails-as-a-result-of-using-the-incorrect-ssl-cert"></a>Se produce un error en la migración como resultado del uso de un certificado SSL incorrecto

* **Síntoma**: Este problema se hace patente cuando un usuario no puede conectarse al servidor de origen de MongoDB. A pesar de que todos los puertos de firewall estén abiertos, el usuario sigue sin poder conectarse.

| Causa         | Solución |
| ------------- | ------------- |
| El uso de un certificado autofirmado en Azure Database Migration Service puede dar lugar a errores en la migración debido al certificado SSL incorrecto. El mensaje de error puede incluir el siguiente texto "The remote certificate is invalid according to the validation procedure" (De acuerdo al procedimiento de validación el certificado remoto no es válido). | Use un certificado original de una entidad de certificación (CA).  Los certificados autofirmados se suelen usar solo en pruebas internas. Al instalar un certificado original desde una entidad de certificación, puede usar SSL en Azure Database Migration Service sin problema (las conexiones a Cosmos DB usan SSL a través de Mongo API).<br><br> |

## <a name="unable-to-get-the-list-of-databases-to-map-in-dms"></a>No se puede obtener la lista bases de datos para asignarlas en DMS

* **Síntoma**: No se puede obtener la lista de bases de datos en la hoja **Configuración de base de datos** al usar el modo **Datos de Azure Storage** en la hoja **Seleccionar origen**.

| Causa         | Solución |
| ------------- | ------------- |
| La cadena de conexión de la cuenta de almacenamiento no tiene información de SAS y, por tanto, no se puede autenticar. | Cree la SAS en el contenedor de blobs en Explorador de Storage y use la dirección URL con la información de la SAS de contenedor como cadena de conexión de detalles de origen.<br><br> |

## <a name="using-an-unsupported-version-of-the-database"></a>Uso de una versión no compatible de la base de datos

* **Síntoma**: Se produce un error en la migración.

| Causa         | Solución |
| ------------- | ------------- |
| Está intentando realizar una migración a Azure Cosmos DB desde una versión no compatible de MongoDB. | A medida que se publican nuevas versiones de MongoDB, se van probando para garantizar la compatibilidad con Azure Database Migration Service, y el servicio se actualiza periódicamente para aceptar las últimas versiones. Si hay una necesidad inmediata de migración, como solución alternativa puede exportar las bases de datos o recopilaciones a Azure Storage, y apuntar el origen al volcado de memoria resultante. Cree la SAS en el contenedor de blobs en Explorador de Storage, y luego, use la dirección URL con la información de la SAS de contenedor como cadena de conexión de detalles de origen.<br><br> |

## <a name="next-steps"></a>Pasos siguientes

* Consulte el tutorial [Migración de MongoDB a la API de Azure Cosmos DB para MongoDB en línea mediante DMS](tutorial-mongodb-cosmos-db-online.md).
* Consulte el tutorial [Migración de MongoDB a la API de Azure Cosmos DB para MongoDB sin conexión mediante DMS](tutorial-mongodb-cosmos-db.md).