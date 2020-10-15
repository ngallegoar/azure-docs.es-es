---
title: Conexión de Qlik Sense a Azure Cosmos DB y visualización de los datos
description: En este artículo se describen los pasos necesarios para conectar Azure Cosmos DB a Qlik Sense y para visualizar los datos.
ms.service: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.topic: how-to
ms.date: 05/23/2019
ms.reviewer: sngun
ms.openlocfilehash: 986db85ddbf741dc459625dbd763b9cbbba1c429
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85263354"
---
# <a name="connect-qlik-sense-to-azure-cosmos-db-and-visualize-your-data"></a>Conexión de Qlik Sense a Azure Cosmos DB y visualización de los datos

Qlik Sense es una herramienta de visualización de datos que combina datos de orígenes diferentes en una sola vista. Qlik Sense indexa todas las posibles relaciones de los datos para poder obtener conocimiento inmediato de los datos. Puede visualizar los datos de Azure Cosmos DB con el uso de Qlik Sense. En este artículo se describen los pasos necesarios para conectar Azure Cosmos DB a Qlik Sense y para visualizar los datos. 

> [!NOTE]
> La conexión de Qlik Sense con Azure Cosmos DB actualmente es compatible solamente con las cuentas de SQL API de Azure Cosmos DB para las cuentas de MongoDB.

Puede conectar Qlik Sense a Azure Cosmos DB con:

* API de SQL de Cosmos DB mediante el conector ODBC.

* API de Azure Cosmos DB para MongoDB con el uso del conector de MongoDB para Qlik Sense (actualmente en versión preliminar).

* API de Azure Cosmos DB para MongoDB y API de SQL con el uso del conector de la API de REST en Qlik Sense.

* API de Mongo DB de Cosmos DB con el uso del conector gRPC para Qlik Core.
En este artículo se describen los detalles de la conexión a la API de SQL de Cosmos DB mediante el conector ODBC.

En este artículo se describen los detalles de la conexión a la API de SQL de Cosmos DB mediante el conector ODBC.

## <a name="prerequisites"></a>Requisitos previos

Antes de seguir las instrucciones del presente artículo, asegúrese de tener los siguientes recursos:

* Descargue [Qlik Sense Desktop](https://www.qlik.com/us/try-or-buy/download-qlik-sense) o configure Qlik Sense en Azure mediante la [instalación del elemento de Marketplace de Qlik Sense](https://azuremarketplace.microsoft.com/marketplace/apps/qlik.qlik-sense).

* Descargue los [datos del videojuego](https://www.kaggle.com/gregorut/videogamesales); estos datos de ejemplo están en formato CSV. Almacenará estos datos en una cuenta de Cosmos DB y los visualizará en Qlik Sense.

* Cree una cuenta de API de SQL de Azure Cosmos DB mediante los pasos descritos en la sección de [creación de una cuenta](create-sql-api-dotnet.md#create-account) del artículo de inicio rápido.

* [Crear una base de datos y una colección](create-sql-api-java.md#add-a-container): puede establecer el valor de rendimiento de la colección en 1000 RU/s. 

* Cargue los datos de ventas del videojuego de ejemplo en la cuenta de Cosmos DB. Puede importar los datos mediante el uso de la herramienta de migración de datos de Azure Cosmos DB; puede hacer una importación [secuencial](import-data.md#SQLSeqTarget) o una [importación en bloque](import-data.md#SQLBulkTarget) de los datos. La importación de los datos en la cuenta de Cosmos DB tarda aproximadamente de 3 a 5 minutos.

* Descargue, instale y configure el controlador ODBC mediante los pasos descritos en el artículo [Conexión a Azure Cosmos DB con el controlador ODBC](odbc-driver.md). Los datos del videojuego son un conjunto de datos simple y no tiene que modificar el esquema; simplemente use el esquema de asignación de colección predeterminado.

## <a name="connect-qlik-sense-to-cosmos-db"></a>Conexión de Qlik Sense a Cosmos DB

1. Abra Qlik Sense y seleccione **Crear nueva aplicación**. Proporcione un nombre a la aplicación y luego seleccione **Crear**.

   :::image type="content" source="./media/visualize-qlik-sense/create-new-qlik-sense-app.png" alt-text="Crear una aplicación de Qlik Sense":::

2. Una vez creada correctamente la aplicación, seleccione **Abrir aplicación** y elija **Add data from files and other sources** (Agregar datos de archivos y otros orígenes). 

3. En los orígenes de datos, seleccione **ODBC** para abrir la ventana de configuración de la nueva conexión. 

4. Cambie a **DSN de usuario** y elija la conexión de ODBC que creó anteriormente. Proporcione un nombre para la conexión y seleccione **Crear**. 

   :::image type="content" source="./media/visualize-qlik-sense/create-new-connection.png" alt-text="Crear una aplicación de Qlik Sense":::

5. Una vez creada la conexión, puede elegir la base de datos, la colección donde se encuentran los datos del videojuego y después obtener la vista previa.

   :::image type="content" source="./media/visualize-qlik-sense/choose-database-and-collection.png" alt-text="Crear una aplicación de Qlik Sense"::: 

6. A continuación, seleccione **Agregar datos** para cargar los datos a Qlik Sense. Después de cargar los datos a Qlik Sense, puede generar información y realizar análisis de los datos. Puede usar esta información o crear su propia aplicación de exploración de las ventas de videojuegos. En la imagen siguiente se muestra: 

   :::image type="content" source="./media/visualize-qlik-sense/visualize-data.png" alt-text="Crear una aplicación de Qlik Sense":::

### <a name="limitations-when-connecting-with-odbc"></a>Limitaciones al conectarse con ODBC 

Cosmos DB es una base de datos distribuida sin esquema con controladores modelados en torno a las necesidades del desarrollador. El controlador ODBC requiere una base de datos con esquema para deducir las columnas, sus tipos de datos y otras propiedades. La consulta SQL normal o la sintaxis DML con funcionalidad relacional no son aplicables a la API de SQL de Cosmos DB porque la API de SQL no es ANSI SQL. Por este motivo, las instrucciones SQL emitidas con el controlador ODBC se traducen en sintaxis SQL específica de Cosmos DB que no tiene equivalentes para todas las construcciones. Para evitar estos problemas de traducción, debe aplicar un esquema al configurar la conexión ODBC. En el artículo [Conexión con el controlador ODBC](odbc-driver.md) se proporcionan sugerencias y métodos para ayudar a configurar el esquema. Asegúrese de crear esta asignación para cada base de datos o colección dentro de la cuenta de Cosmos DB.

## <a name="next-steps"></a>Pasos siguientes

Si usa una herramienta de visualización diferente, como Power BI, puede conectarse a ella siguiendo las instrucciones en el documento siguiente:

* [Visualizar datos de Cosmos DB mediante el conector de Power BI](powerbi-visualize.md)
