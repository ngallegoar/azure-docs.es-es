---
title: Conexión a Azure Synapse Link para Azure Cosmos DB
description: Conexión de una instancia de Azure Cosmos DB a un área de trabajo de Synapse con Azure Synapse Link
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/21/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: d72d1feda4c267dfa3d5c0dea6928f1b7541b26d
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83599015"
---
# <a name="connect-to-azure-synapse-link-for-azure-cosmos-db"></a>Conexión a Azure Synapse Link para Azure Cosmos DB

En este artículo se describe cómo acceder a una base de datos de Azure Cosmos DB desde Azure Synapse Analytics Studio con Azure Synapse Link.

## <a name="prerequisites"></a>Prerrequisitos

Antes de conectar una base de datos de Azure Cosmos DB al área de trabajo, necesitará lo siguiente:

* Una base de datos de Azure Cosmos DB existente o deberá seguir [este inicio rápido](https://docs.microsoft.com/azure/cosmos-db/how-to-manage-database-account) para crear una nueva cuenta.
* Una área de trabajo de Synapse existente o deberá seguir [este inicio rápido](https://docs.microsoft.com/azure/synapse-analytics/quickstart-create-workspace) para crear una nueva. 

## <a name="enable-azure-cosmos-db-analytical-store"></a>Habilitación del almacén analítico de Azure Cosmos DB

Para ejecutar análisis a gran escala en Azure Cosmos DB sin afectar al rendimiento operativo, le recomendamos que habilite Synapse Link para Azure Cosmos DB. Synapse Link aporta la funcionalidad de HTAP a un contenedor y compatibilidad integrada en Azure Synapse.

## <a name="navigate-to-synapse-studio"></a>Navegación a Synapse Studio

En el área de trabajo de Synapse, seleccione **Iniciar Synapse Studio**. En la página principal de Synapse Studio, seleccione **Datos y se le dirigirá al **Data Object Explorer** (Explorador de objetos de datos).

## <a name="connect-an-azure-cosmos-db-database-to-a-synapse-workspace"></a>Conexión de una base de datos de Azure Cosmos DB a un área de trabajo de Synapse

La conexión de una base de datos de Azure Cosmos DB se realiza como un servicio vinculado. Un servicio vinculado de Azure Cosmos DB permite a los usuarios examinar y explorar datos, y leer y escribir contenido de Apache Spark para Azure Synapse Analytics o SQL en Azure Cosmos DB.

En Data Object Explorer (Explorador de objetos de datos), puede conectarse directamente a una base de datos de Azure Cosmos DB siguiendo estos pasos:

1. Seleccione el icono ***+*** cerca de la opción Datos.
2. Seleccione **Connect to external data** (Conectar a datos externos).
3. Seleccione la API a la que quiere conectarse. API de SQL o API para MongoDB
4. Seleccione ***Continuar***
5. Asigne un nombre al servicio vinculado. El nombre se mostrará en el Explorador de objetos y lo usarán los tiempos de ejecución de Synapse para conectarse a la base de datos y a los contenedores. Le recomendamos que use un nombre descriptivo.
6. Seleccione el **nombre de la cuenta de Azure Cosmos DB** y el **nombre de la base de datos**.
7. (Opcional) Si no se especifica ninguna región, las operaciones de tiempo de ejecución de Synapse se enrutarán hacia la región más cercana en la que esté habilitado el almacén analítico. Sin embargo, puede establecer manualmente en qué región quiere que los usuarios accedan al almacén analítico de Azure Cosmos DB. Seleccione **Propiedades de conexión adicionales** y, a continuación, **Nuevo**. En **Nombre de propiedad**, escriba ***PreferredRegions*** y establezca el **valor** en la región que quiera (por ejemplo: WestUS2; no hay ningún espacio entre las palabras y el número).
8. Seleccione ***Crear***

Las bases de datos de Azure Cosmos DB están visibles en la pestaña **Vinculado** de la sección Azure Cosmos DB. Con Azure Cosmos DB, puede diferenciar un contenedor habilitado para HTAP de un contenedor solo de OLTP mediante los iconos siguientes:

**Contenedor solo de OLTP**:

![Contenedor de OLTP](../media/quickstart-connect-synapse-link-cosmosdb/oltp-container.png)

**Contenedor habilitado para HTAP**:

![Contenedor de HTAP](../media/quickstart-connect-synapse-link-cosmosdb/htap-container.png)

## <a name="quickly-interact-with-code-generated-actions"></a>Interacción rápida con las acciones generadas por el código

Al hacer clic con el botón derecho en un contenedor, tendrá una lista de gestos que desencadenarán un tiempo de ejecución de Spark o SQL. La escritura en un contenedor se realizará a través del almacén transaccional de Azure Cosmos DB y consumirá unidades de solicitud.  

## <a name="next-steps"></a>Pasos siguientes

* [Información sobre compatibilidad entre Synapse y Azure Cosmos DB](./concept-synapse-link-cosmos-db-support.md)
* [Información sobre cómo realizar consultas en el almacén analítico con Spark](./how-to-query-analytical-store-spark.md)