---
title: 'Tutorial: Migración de MongoDB sin conexión a la API de Azure Cosmos DB para MongoDB'
titleSuffix: Azure Database Migration Service
description: Aprenda a migrar desde el entorno local de MongoDB a la API de Azure Cosmos DB para MongoDB sin conexión mediante Azure Database Migration Service.
services: dms
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: tutorial
ms.date: 01/08/2020
ms.openlocfilehash: 4f3b201d35781d6d33eead0b0a21d38fbb897097
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94966826"
---
# <a name="tutorial-migrate-mongodb-to-azure-cosmos-dbs-api-for-mongodb-offline-using-dms"></a>Tutorial: Migración de MongoDB a la API de Azure Cosmos DB para MongoDB sin conexión mediante DMS

Puede usar Azure Database Migration Service para realizar una migración sin conexión (de una sola vez) de las bases de datos desde una instancia local o en la nube de MongoDB a la API de Azure Cosmos DB para MongoDB.

En este tutorial, aprenderá a:
> [!div class="checklist"]
>
> * Crear una instancia de Azure Database Migration Service.
> * Crear un proyecto de migración mediante Azure Database Migration Service.
> * Ejecutar la migración.
> * Supervisar la migración

En este tutorial, migrará un conjunto de datos de MongoDB hospedado en una máquina virtual de Azure a la API de Azure Cosmos DB para MongoDB mediante Azure Database Migration Service. Si no tiene un origen de MongoDB ya configurado, vea el artículo [Instalación y configuración de MongoDB en una máquina virtual Windows en Azure](../virtual-machines/windows/install-mongodb.md).

## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial, necesita:

* [Realizar los pasos previos a la migración](../cosmos-db/mongodb-pre-migration.md) como son estimar el rendimiento, elegir una clave de partición y seleccionar la directiva de indexación.
* [Crear una cuenta para la API de Azure Cosmos DB para MongoDB](https://ms.portal.azure.com/#create/Microsoft.DocumentDB).
* Cree una instancia de Microsoft Azure Virtual Network para Azure Database Migration Service con el modelo de implementación de Azure Resource Manager, que proporciona conectividad de sitio a sitio a los servidores de origen local mediante [ExpressRoute](../expressroute/expressroute-introduction.md) o [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md). Para más información sobre la creación de una red virtual, consulte la documentación de [Virtual Network](../virtual-network/index.yml)y, especialmente, los artículos de inicio rápido con detalles paso a paso.

    > [!NOTE]
    > Durante la configuración de la red virtual, si usa ExpressRoute con emparejamiento de red a Microsoft, agregue los siguientes [puntos de conexión](../virtual-network/virtual-network-service-endpoints-overview.md) de servicio a la subred en la que se aprovisionará el servicio:
    >
    > * Punto de conexión de base de datos de destino (por ejemplo, punto de conexión de SQL, punto de conexión de Cosmos DB, etc.)
    > * Punto de conexión de Storage
    > * Punto de conexión de Service Bus
    >
    > Esta configuración es necesaria porque Azure Database Migration Service no tiene conexión a Internet.

* Asegúrese de que las reglas del grupo de seguridad de red (NSG) de la red virtual no bloquean los siguientes puertos de comunicación: 53, 443, 445, 9354 y 10000-20000. Para más información sobre el filtrado del tráfico con grupos de seguridad de red para redes virtuales, vea el artículo [Filtrado del tráfico de red con grupos de seguridad de red](../virtual-network/virtual-network-vnet-plan-design-arm.md).
* Abra el Firewall de Windows para permitir que Azure Database Migration Service acceda al servidor de MongoDB de origen que, de manera predeterminada, es el puerto TCP 27017.
* Cuando se usa un dispositivo de firewall frente a las bases de datos de origen, puede que sea necesario agregar reglas de firewall para permitir que Azure Database Migration Service acceda a las bases de datos de origen para realizar la migración.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registro del proveedor de recursos Microsoft.DataMigration

1. Inicie sesión en Azure Portal, seleccione **Todos los servicios** y, después, **Suscripciones**.

   ![Mostrar suscripciones en el portal](media/tutorial-mongodb-to-cosmosdb/portal-select-subscription1.png)

2. Seleccione la suscripción en la que quiere crear la instancia de Azure Database Migration Service y después seleccione **Proveedores de recursos**.

    ![Mostrar los proveedores de recursos](media/tutorial-mongodb-to-cosmosdb/portal-select-resource-provider.png)

3. Busque la migración y después, a la derecha de **Microsoft.DataMigration**, seleccione **Registrar**.

    ![Registro del proveedor de recursos](media/tutorial-mongodb-to-cosmosdb/portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Creación de una instancia

1. En Azure Portal, seleccione + **Crear un recurso**, busque Azure Database Migration Service y, después, seleccione **Azure Database Migration Service** en la lista desplegable.

    ![Azure Marketplace](media/tutorial-mongodb-to-cosmosdb/portal-marketplace.png)

2. En la pantalla **Azure Database Migration Service**, seleccione **Crear**.

    ![Creación de una instancia de Azure Database Migration Service](media/tutorial-mongodb-to-cosmosdb/dms-create1.png)
  
3. En la pantalla **Crear el servicio de migración**, especifique un nombre para el servicio, la suscripción y un grupo de recursos nuevo o existente.

4. Seleccione la ubicación en la que quiere crear la instancia de Azure Database Migration Service. 

5. Seleccione una red virtual existente o cree una nueva.

    La red virtual proporciona a Azure Database Migration Service acceso a la instancia de MongoDB de origen y a la cuenta de Azure Cosmos DB de destino.

    Para más información sobre cómo crear una red virtual en Azure Portal, consulte el artículo [Creación de una red virtual con Azure Portal](../virtual-network/quick-create-portal.md).

6. Seleccione un plan de tarifa.

    Para más información sobre los costos y planes de tarifa, vea la [página de precios](https://aka.ms/dms-pricing).

    ![Configuración de la instancia de Azure Database Migration Service](media/tutorial-mongodb-to-cosmosdb/dms-settings2.png)

7. Seleccione **Crear** para crear el servicio.

## <a name="create-a-migration-project"></a>Creación de un proyecto de migración

Después de crear el servicio, búsquelo en Azure Portal, ábralo y cree un proyecto de migración.

1. En Azure Portal, seleccione **Todos los servicios**, busque Azure Database Migration Service y, luego, elija **Azure Database Migration Services**.

      ![Búsqueda de todas las instancias de Azure Database Migration Service](media/tutorial-mongodb-to-cosmosdb/dms-search.png)

2. En la pantalla **Azure Database Migration Services**, busque el nombre de la instancia de Azure Database Migration Service que creó y, después, seleccione la instancia.

3. Seleccione **+ New Migration Project** (+ Nuevo proyecto de migración).

4. En la pantalla **Nuevo proyecto de migración**, especifique un nombre para el proyecto. En el cuadro de texto **Source server type** (Tipo de servidor de origen), seleccione **MongoDB**, en el cuadro de texto **Target server type** (Tipo de servidor de destino), seleccione **CosmosDB (API de MongoDB)** y, por último, en **Elegir tipo de actividad**, seleccione **Migración de datos sin conexión**. 

    ![Creación de un proyecto de Database Migration Service](media/tutorial-mongodb-to-cosmosdb/dms-create-project.png)

5. Seleccione **Crear y ejecutar una actividad** para crear el proyecto y ejecutar la actividad de migración.

## <a name="specify-source-details"></a>Especificación de los detalles de origen

1. En la pantalla **Detalles del origen**, especifique los detalles de conexión del servidor de MongoDB de origen.

   > [!IMPORTANT]
   > Azure Database Migration Service no admite Azure Cosmos DB como origen.

    Hay tres modos de conexión a un origen:
   * **Modo estándar**, que acepta un nombre de dominio completo o una dirección IP, número de puerto y las credenciales de conexión.
   * **Modo de cadena de conexión**, que acepta una cadena de conexión de MongoDB, como se describe en el artículo [Connection String URI Format](https://docs.mongodb.com/manual/reference/connection-string/) (Formato de identificador URI de cadena de conexión).
   * **Datos de Azure Storage**, que acepta un dirección URL de SAS del contenedor de blobs. Seleccione **El blob contiene volcados BSON** si el contenedor de blobs tiene volcados BSON producidos por la [herramienta bsondump](https://docs.mongodb.com/manual/reference/program/bsondump/) de MongoDB y anule su selección si el contenedor contiene archivos JSON.

     Si selecciona esta opción, asegúrese de que la cadena de conexión de la cuenta de almacenamiento aparece en el formato:

     ```
     https://blobnameurl/container?SASKEY
     ```

     Esta cadena de conexión de SAS del contenedor de blobs se puede encontrar en el Explorador de Azure Storage. La creación de SAS para el contenedor implicado le proporcionará la dirección URL con el formato solicitado anterior.
     
     Además, según la información del tipo de volcado de memoria de Azure Storage, tenga en cuenta el siguiente detalle.

     * En el caso de los volcados de BSON, los datos del contenedor de blobs deben estar en formato bsondump, con el fin de que dichos archivos se coloquen en carpetas que se llamen igual que las bases de datos que contienen en el formato colección.bson. A los archivos de metadatos (si hubiera) se les deben asignar el nombre con el formato *colección*.metadata.json.

     * En el caso de los volcados de JSON, los archivos del contenedor de blobs deben colocarse en carpetas que se llamen igual que las bases de datos que contienen. Dentro de cada una de estas carpetas, los archivos de datos se deben colocar en una subcarpeta denominada "data" y se le debe asignar el nombre con el formato *colección*.json. Los archivos de metadatos (si hubiera) se deben colocar en una subcarpeta denominada "metadata" y se le debe asignar el nombre con el mismo formato, *colección*.json. Los archivos de metadatos deben estar en el mismo formato que los que genera la herramienta bsondump de MongoDB.

    > [!IMPORTANT]
    > No se recomienda usar un certificado autofirmado en el servidor de Mongo. Sin embargo, si lo usa, conecte con el servidor con el **modo de cadena de conexión** y asegúrese de que la cadena de conexión es "".
    >
    >```
    >&sslVerifyCertificate=false
    >```

   También puede usar la dirección IP en los casos en que no sea posible la resolución de nombres de DNS.

   ![Especificación de los detalles de origen](media/tutorial-mongodb-to-cosmosdb/dms-specify-source.png)

2. Seleccione **Guardar**.

## <a name="specify-target-details"></a>Especificación de los detalles de destino

1. En la pantalla **Detalles del destino de la migración**, especifique los detalles de conexión de la cuenta de Azure Cosmos DB de destino, que es la cuenta de la API de Azure Cosmos DB para MongoDB aprovisionada previamente a la que se van a migrar los datos de MongoDB.

    ![Especificación de los detalles de destino](media/tutorial-mongodb-to-cosmosdb/dms-specify-target.png)

2. Seleccione **Guardar**.

## <a name="map-to-target-databases"></a>Asignación a bases de datos de destino

1. En la pantalla **Map to target databases** (Asignar a bases de datos de destino), asigne la base de datos de origen y de destino para la migración.

    Si la base de datos de destino contiene el mismo nombre de base de datos que la de origen, Azure Database Migration Service selecciona la base de datos de destino de forma predeterminada.

    Si la cadena **Crear** aparece junto al nombre de la base de datos, indica que Azure Database Migration Service no encontró la base de datos de destino, y el servicio creará la base de datos automáticamente.

    En este momento de la migración, puede [aprovisionar rendimiento](../cosmos-db/set-throughput.md). En Cosmos DB, puede aprovisionar rendimiento a nivel de base de datos o individualmente para cada colección. El rendimiento se mide en [unidades de solicitud](../cosmos-db/request-units.md) (RU). Obtenga más información sobre los [precios de Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/).

    ![Asignación a bases de datos de destino](media/tutorial-mongodb-to-cosmosdb/dms-map-target-databases.png)

2. Seleccione **Guardar**.
3. En la pantalla **Configuración de colecciones**, expanda la lista de colecciones y, a continuación, revise la lista de las colecciones que se van a migrar.

    Azure Database Migration Service selecciona automáticamente todas las colecciones existentes en la instancia de MongoDB de origen que no existen en la cuenta de Azure Cosmos DB de destino. Si quiere volver a migrar las colecciones que ya incluyen datos, necesita seleccionar explícitamente las colecciones de esta hoja.

    Puede especificar la cantidad de unidades de solicitud que desea que las colecciones usen. Azure Database Migration Service sugiere valores predeterminados inteligentes en función del tamaño de la colección.

    > [!NOTE]
    > Realice la migración de base de datos y la colección en paralelo mediante múltiples instancias de Azure Database Migration Service, si es necesario, para acelerar la ejecución.

    También puede especificar una clave de partición para aprovechar las ventajas de la [creación de particiones en Azure Cosmos DB](../cosmos-db/partitioning-overview.md) para una escalabilidad óptima. No olvide revisar los [procedimientos recomendados para seleccionar una clave de partición](../cosmos-db/partitioning-overview.md#choose-partitionkey).

    ![Selección de tablas de colecciones](media/tutorial-mongodb-to-cosmosdb/dms-collection-setting.png)

4. Seleccione **Guardar**.

5. En la pantalla **Migration summary** (Resumen de migración), en el cuadro de texto **Nombre de actividad**, especifique un nombre para la actividad de migración.

    ![Resumen de migración](media/tutorial-mongodb-to-cosmosdb/dms-migration-summary.png)

## <a name="run-the-migration"></a>Ejecución de la migración

* Seleccione **Ejecutar migración**.

    Aparecerá la ventana de actividad de migración y el **estado** de la actividad será **Sin iniciar**.

    ![Estado de la actividad](media/tutorial-mongodb-to-cosmosdb/dms-activity-status.png)

## <a name="monitor-the-migration"></a>Supervisión de la migración

* En la pantalla de la actividad de migración, seleccione **Actualizar** para actualizar la vista hasta que el valor de **Estado** de la migración sea **Completado**.

   > [!NOTE]
   > Puede seleccionar la actividad para obtener los detalles de las métricas de migración a nivel de base de datos y de colección.

    ![Estado de actividad completado](media/tutorial-mongodb-to-cosmosdb/dms-activity-completed.png)

## <a name="verify-data-in-cosmos-db"></a>Verificación de datos en Cosmos DB

* Después de finalizar la migración, podrá consultar la cuenta de Azure Cosmos DB para comprobar que todas las colecciones se han migrado correctamente.

    ![Captura de pantalla que muestra dónde consultar la cuenta de Azure Cosmos DB para comprobar que todas las colecciones se han migrado correctamente.](media/tutorial-mongodb-to-cosmosdb/dms-cosmosdb-data-explorer.png)

## <a name="post-migration-optimization"></a>Optimización posterior a la migración

Después de migrar los datos almacenados en la base de datos de MongoDB a la API de Azure Cosmos DB para MongoDB, puede conectarse a Azure Cosmos DB y administrar los datos. También puede realizar otros pasos de optimización posteriores a la migración, como optimizar la directiva de indexación, actualizar el nivel de coherencia predeterminado o configurar la distribución global de la cuenta de Azure Cosmos DB. Para más información, consulte el artículo [Optimización posterior a la migración](../cosmos-db/mongodb-post-migration.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Información sobre el servicio Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)

## <a name="next-steps"></a>Pasos siguientes

* Revise las directrices de migración en la [Guía de migración de bases de datos](https://datamigration.microsoft.com/) para consultar escenarios adicionales.