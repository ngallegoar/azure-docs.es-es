---
title: Introducción a Cassandra API de Azure Cosmos DB
description: Aprenda a usar Azure Cosmos DB para migrar mediante "lift-and-shift" las aplicaciones existentes y compilar nuevas aplicaciones mediante los controladores de Cassandra y CQL
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: overview
ms.date: 05/21/2019
ms.openlocfilehash: 075b55b5a798924b55ef9d901e4d2e9ecfc9dc1e
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "77597582"
---
# <a name="introduction-to-the-azure-cosmos-db-cassandra-api"></a>Introducción a Cassandra API de Azure Cosmos DB

Cassandra API de Azure Cosmos DB se puede usar como almacén de datos para aplicaciones escritas para [Apache Cassandra](https://cassandra.apache.org). Esto significa que si se usan [controladores de Apache](https://cassandra.apache.org/doc/latest/getting_started/drivers.html?highlight=driver) existentes compatibles con CQLv4, la aplicación de Cassandra existente se puede comunicar con Cassandra API de Azure Cosmos DB. En muchos casos puede pasar de usar Apache Cassandra a emplear Cassandra API de Azure Cosmos DB con solo cambiar una cadena de conexión. 

Cassandra API permite interactuar con los datos almacenados en Azure Cosmos DB mediante Cassandra Query Language (CQL), herramientas basadas en Cassandra (como cqlsh) y controladores cliente de Cassandra con los que ya está familiarizado.

## <a name="what-is-the-benefit-of-using-apache-cassandra-api-for-azure-cosmos-db"></a>¿Cuál es la ventaja de usar la API Apache Cassandra para Azure Cosmos DB?

**Ninguna administración de operaciones**: como servicio en la nube completamente administrado, Cassandra API de Azure Cosmos DB acaba con la sobrecarga de administrar y supervisar innumerables configuraciones en el sistema operativo, JVM y los archivos yaml y sus interacciones. Azure Cosmos DB proporciona supervisión de rendimiento, latencia, almacenamiento, disponibilidad y alertas configurables.

**Administración de rendimiento**: Azure Cosmos DB proporciona lecturas y escrituras de baja latencia garantizadas en el percentil 99, respaldadas por el Acuerdo de Nivel de Servicio. Los usuarios no tienen que preocuparse por la sobrecarga operativa de garantizar un alto rendimiento y lecturas y escrituras de baja latencia. Esto significa que los usuarios no tienen que programar la compactación, administrar marcadores de exclusión ni configurar filtros de bloom ni réplicas manualmente. Azure Cosmos DB acaba con la sobrecarga de tener que administrar estos problemas y permite centrarse en la lógica de la aplicación.

**Posibilidad de usar código y herramientas existentes**: Azure Cosmos DB proporciona compatibilidad de nivel de protocolo de conexión con SDK y herramientas existentes de Cassandra. Esta compatibilidad garantiza que pueda usar el código base existente con Cassandra API de Azure Cosmos DB con cambios triviales.

**Elasticidad de almacenamiento y rendimiento**: Azure Cosmos DB proporciona un rendimiento garantizado en todas las regiones y puede escalar el rendimiento aprovisionado con operaciones de Azure Portal, PowerShell o la CLI. Se puede escalar de forma elástica el almacenamiento y el rendimiento de las tablas según sea necesario con el rendimiento predecible.

**Disponibilidad y distribución globales**: Azure Cosmos DB ofrece la posibilidad de distribuir los datos globalmente en todas las regiones de Azure y de proporcionarlos localmente a la vez que garantiza un acceso a datos de baja latencia y alta disponibilidad. Azure Cosmos DB proporciona una alta disponibilidad del 99,99 % dentro de una región y una disponibilidad de lectura y escritura del 99,999 % en varias regiones sin sobrecarga de operaciones. Obtenga más información en el artículo [Distribución de datos global](distribute-data-globally.md). 

**Opción de coherencia**: Azure Cosmos DB proporciona la opción de cinco niveles de coherencia bien definidos para lograr un equilibrio óptimo entre la coherencia y el rendimiento. Estos niveles de coherencia son Alta, Obsolescencia limitada, Sesión, Prefijo coherente y Posible. Estos niveles de coherencia bien definidos, prácticos e intuitivos permiten a los desarrolladores lograr un equilibrio preciso entre la coherencia, la disponibilidad y la latencia. Obtenga más información en el artículo [Niveles de coherencia](consistency-levels.md). 

**Nivel empresarial**: Azure Cosmos DB proporciona [certificaciones de cumplimiento normativo](https://www.microsoft.com/trustcenter) para garantizar que los usuarios puedan usar la plataforma de forma segura. Azure Cosmos DB también proporciona cifrado en reposo y en movimiento, firewall de IP y registros de auditoría para actividades del plano de control.

## <a name="next-steps"></a>Pasos siguientes

* Puede empezar rápidamente a compilar las siguientes aplicaciones específicas de lenguaje para crear y administrar datos de Cassandra API:
  - [Aplicación Node.js](create-cassandra-nodejs.md)
  - [Aplicación .NET](create-cassandra-dotnet.md)
  - [Aplicación Python](create-cassandra-python.md)

* Introducción a la [creación de una cuenta de Cassandra API, una base de datos y una tabla](create-cassandra-api-account-java.md) mediante una aplicación de Java.

* [Carga de datos de ejemplo en la tabla de Cassandra API](cassandra-api-load-data.md) mediante el uso de una aplicación de Java.

* [Consulta de datos de la cuenta de Cassandra API](cassandra-api-query-data.md) mediante el uso de una aplicación de Java.

* Para obtener más información sobre las características de Apache Cassandra admitidas por Cassandra API de Azure Cosmos DB, vea el artículo [Cassandra support](cassandra-support.md) (Compatibilidad de Cassandra).

* Lea las [Preguntas más frecuentes](faq.md#cassandra).
