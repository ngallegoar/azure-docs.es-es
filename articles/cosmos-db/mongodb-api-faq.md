---
title: Preguntas más frecuentes sobre la API de Azure Cosmos DB para MongoDB
description: Obtenga respuestas a las preguntas más frecuentes sobre la API de Azure Cosmos DB para MongoDB.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 04/28/2020
ms.author: sngun
ms.openlocfilehash: 4c93aea5a37fb286b49dabfb97522bf40c8f5429
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93333178"
---
# <a name="frequently-asked-questions-about-the-azure-cosmos-dbs-api-for-mongodb"></a>Preguntas más frecuentes sobre la API de Azure Cosmos DB para MongoDB
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

La API de Azure Cosmos DB para MongoDB es una capa de compatibilidad de protocolo de transferencia que permite a las aplicaciones comunicarse de manera sencilla y transparente con el motor de base de datos nativo de Azure Cosmos mediante los controladores y los SDK para MongoDB admitidos por la comunidad. Los desarrolladores ahora pueden usar cadenas de herramienta de MongoDB existentes y aptitudes para compilar aplicaciones que aprovechan las ventajas de Azure Cosmos DB. Los desarrolladores se benefician de las capacidades únicas de Azure Cosmos DB, que incluyen distribución global con replicación de escritura en varias regiones, indexación automática, mantenimiento de copias de seguridad, contratos de nivel de servicio con respaldo financiero, etc.

## <a name="how-do-i-connect-to-my-database"></a>¿Cómo me conecto a mi base de datos?

La forma más rápida de conectarse a una base de datos de Cosmos con la API de Azure Cosmos DB para MongoDB es a través de [Azure Portal](https://portal.azure.com). Vaya a la cuenta y luego, en el menú de navegación izquierdo, haga clic en **Inicio rápido**. Inicio rápido es la mejor manera de obtener fragmentos de código para conectarse a su base de datos.

Azure Cosmos DB aplica estándares y requisitos de seguridad estrictos. Las cuentas de Azure Cosmos DB requieren autenticación y comunicación segura mediante TLS, por lo que debe usar TLS v1.2.

Para más información, consulte [Conectar una aplicación de MongoDB a Azure Cosmos DB](connect-mongodb-account.md).

## <a name="error-codes-while-using-azure-cosmos-dbs-api-for-mongodb"></a>¿Se muestran códigos de error al usar la API de Azure Cosmos DB para MongoDB?

Además de los códigos de error comunes de MongoDB, la API de Azure Cosmos DB para MongoDB tiene sus propios códigos de error:

| Error               | Código  | Descripción  | Solución  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | El número total de unidades de solicitud consumidas es mayor que la tasa de unidades de solicitud aprovisionadas para el contenedor y se ha limitado. | Considere la posibilidad de escalar el rendimiento asignado a un contenedor o un conjunto de contenedores desde Azure Portal o reintentarlo de nuevo. |
| ExceededMemoryLimit | 16501 | Como se trata de un servicio de varios inquilinos, la operación ha superado la asignación de memoria del cliente. | Reduzca el ámbito de la operación a través de criterios de consulta más restrictivos o póngase en contacto con soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). <br><br> Ejemplo: `db.getCollection('users').aggregate([{$match: {name: "Andy"}}, {$sort: {age: -1}}]))` |

## <a name="supported-drivers"></a>Controladores admitidos

### <a name="is-the-simba-driver-for-mongodb-supported-for-use-with-azure-cosmos-dbs-api-for-mongodb"></a>¿Se puede usar el controlador Simba para la API de Azure Cosmos DB para MongoDB?

Sí, puede usar el controlador ODBC para Mongo de Simba con la API de Azure Cosmos DB para MongoDB.

## <a name="next-steps"></a>Pasos siguientes

* [Compilación de una aplicación web .NET mediante la API de Azure Cosmos DB para MongoDB](create-mongodb-dotnet.md)
* [Creación de una aplicación de consola con Java y la API MongoDB en Azure Cosmos DB](create-mongodb-java.md)
