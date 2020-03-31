---
title: Almacenamiento, administración y conservación de datos de aplicaciones en la nube con Visual Studio App Center y servicios de Azure
description: Obtenga más información sobre servicios como Visual Studio App Center, que permiten almacenar, administrar y conservar datos de aplicaciones móviles en la nube.
author: codemillmatt
ms.assetid: 12344321-0123-4678-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: 166847325fa9094136f1c2a143f1751420f05f66
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240907"
---
# <a name="store-sync-and-query-mobile-application-data-from-the-cloud"></a>Almacenamiento, sincronización y consulta de datos de aplicaciones móviles desde la nube
Con independencia del tipo de aplicación que cree, es probable que genere y procese datos. Los usuarios de la aplicación tienen expectativas altas. Quieren que la aplicación funcione de forma rápida y sin problemas, en todas las circunstancias. La mayoría de las aplicaciones también funcionan en varios dispositivos. Puede tener acceso a la aplicación desde un dispositivo móvil o de escritorio. Es posible que varios usuarios usen la aplicación al mismo tiempo y que compartan datos con la expectativa de obtener acceso instantáneo y en tiempo real a ellos.

Los usuarios de la aplicación no siempre tendrán conectividad a Internet. Las aplicaciones están diseñadas para que funcionen con o sin conexión a Internet y eso es lo que se espera de ellas. Los desarrolladores deben elegir la solución adecuada para almacenar sus datos en la nube y sincronizarlos con esta con el fin de proporcionar al cliente una excelente experiencia con su aplicación.

Microsoft proporciona una gran variedad de servicios que eliminan la necesidad de poner en marcha los servidores, de elegir la base de datos o de preocuparse por el escalado o la seguridad para ofrecer la mejor experiencia posible. Estos servicios proporcionan una excelente experiencia de desarrollador que permite almacenar datos de aplicaciones en la nube mediante el uso de API de SQL o NoSQL. También puede sincronizar datos en todos los dispositivos y permitir que la aplicación funcione con o sin conexión de red para ayudar a crear aplicaciones escalables y robustas.

Use los servicios siguientes para administrar y almacenar datos de aplicaciones móviles en la nube.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center Data](/appcenter/data/) es un servicio de administración de datos. Permite a las aplicaciones administrar, conservar y sincronizar los datos en la nube entre distintos dispositivos y plataformas, tanto en escenarios en línea como sin conexión. Basado en Azure Cosmos DB, App Center Data se escala a medida que la base de usuarios y el número de aplicaciones crecen. Garantiza una baja latencia, alta disponibilidad y alta escalabilidad para todos los datos.

**Características principales**
- Aprovisionamiento sencillo en una nueva base de datos de Azure Cosmos DB o en una base de datos de Azure Cosmos DB existente desde el portal de Visual Studio App Center.
- Compatibilidad con bases de datos NoSQL para almacenar, sincronizar y consultar los datos de las aplicaciones fácilmente.
- Basado en Azure Cosmos DB, este servicio hereda la mayoría de las características clave que ofrece Azure Cosmos DB y puede escalarse globalmente para satisfacer sus necesidades empresariales.
- Funcionalidades de sincronización en línea y sin conexión para sincronizar datos entre dispositivos.
- SDK de cliente para dispositivos móviles que puede usar para administrar fácilmente los datos privados de las aplicaciones.
- Compatibilidad con las plataformas iOS, Android, Xamarin y React Native.

**Referencias**
- [Registro con Visual Studio App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [Introducción a App Center Data](/appcenter/data/getting-started)

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) es un servicio de base de datos multimodelo distribuido globalmente. Se puede usar para crear una aplicación escalable a nivel global. Con Azure Cosmos DB, puede escalar de forma elástica e independiente el rendimiento y el almacenamiento en cualquier número de regiones de Azure de todo el mundo. Puede aprovechar el acceso rápido a datos de un solo en milisegundos mediante sus superficies de API favoritas. Estas superficies incluyen SQL, MongoDB, Cassandra, Tables o Gremlin. Azure Cosmos DB proporciona de forma exclusiva completos Acuerdos de Nivel de Servicio (SLA) para el rendimiento, la latencia, la disponibilidad y la coherencia.

**Características principales**
- Admite un amplio abanico de API, como API SQL (Core), API Cassandra, API de MongoDB, API Gremlin y API Table.
- La distribución global llave en mano replica los datos dondequiera que estén los usuarios. Los usuarios pueden interactuar con una réplica de los datos más cercanos.
- Sin administración de esquemas o índices, ya que el motor de base de datos es completamente independiente del esquema.
- Presencia en todas las regiones, ya que Cosmos DB está disponible en todas las regiones de Azure del mundo, incluidas las más de 54 regiones de la nube pública.
- Varias opciones de coherencia definidas con precisión, ya que el protocolo de replicación de varios maestros de Cosmos DB se ha diseñado cuidadosamente para ofrecer cinco opciones de coherencia bien definidas. Las cinco opciones son fuerte, obsolescencia limitada, sesión, prefijo coherente y eventual.
- Disponibilidad del 99,999 % tanto en lectura como en escritura.
- Invocación mediante programación (o a través de Azure Portal) de la conmutación por error regional de la cuenta de Azure Cosmos DB para tener la seguridad de que la aplicación está diseñada para resistir un desastre regional.
- Baja latencia garantizada en el percentil 99, en todo el mundo.

**Referencias**
- [Azure Portal](https://portal.azure.com) 
- [Documentación sobre Azure Cosmos DB](/azure/cosmos-db/introduction)

## <a name="azure-sql-database"></a>Azure SQL Database
 [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) es un servicio administrado de base de datos relacional de uso general. Puede usarlo para crear una capa de almacenamiento de datos de gran rendimiento y disponibilidad para las aplicaciones y las soluciones de la nube de Azure.

**Características principales**
- **Modelos y herramientas de bases de datos elásticas:** Con una base de datos elástica, los desarrolladores pueden agrupar los recursos entre un grupo de bases de datos para el escalado. Para gestionar estos recursos de forma administrativa, envíe un script como trabajo. A continuación, la base de datos SQL usa el script en las bases de datos.
- **Alto rendimiento:** las aplicaciones de alto rendimiento pueden aprovechar la versión más reciente. Ofrece un 25 % más de capacidad de base de datos prémium.
- **Copias de seguridad, replicación y alta disponibilidad:** La replicación integrada y un Acuerdo de Nivel de Servicio respaldado por Microsoft en el nivel de base de datos permiten la continuidad y la protección de las aplicaciones frente a eventos catastróficos. La replicación geográfica activa le permite configurar la conmutación por error y el autoservicio de restauración, lo que proporciona un control total sobre la recuperación de errores. La restauración de datos desde copias de seguridad está disponible hasta 35 días.
- **Mantenimiento casi inexistente:** El software automático forma parte del servicio. Las réplicas del sistema integradas permiten protección de datos inherente, tiempo de actividad de la base de datos y estabilidad del sistema. Las réplicas del sistema se mueven automáticamente a los nuevos equipos. Se aprovisionan sobre la marcha a medida que se produce un error en las antiguas.
- **Seguridad:** Azure SQL Database ofrece un conjunto de características de seguridad que le ayudan a cumplir las directivas de cumplimiento impuestas por la organización o el sector.
    - La auditoría proporciona a los desarrolladores la posibilidad de realizar tareas relacionadas con el cumplimiento y obtener información sobre las actividades.
    - Los desarrolladores y los responsables de TI también pueden implementar directivas en el nivel de base de datos para limitar el acceso a datos confidenciales con seguridad de nivel de fila, enmascaramiento de datos dinámico y cifrado de datos transparente en Azure SQL Database.
    - Azure SQL Database está verificado por importantes auditores de la nube como parte del proceso de certificaciones de conformidad y aprobaciones clave de Azure, como el acuerdo de asociación comercial de HIPAA, ISO/IEC 27001:2005, FedRAMP y las cláusulas según el modelo de la UE.

**Referencias**
- [Azure Portal](https://portal.azure.com) 
- [Documentación de Azure SQL Database](/azure/sql-database/) 
