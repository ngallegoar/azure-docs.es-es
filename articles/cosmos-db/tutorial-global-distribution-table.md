---
title: Tutorial de distribución global de Azure Cosmos DB para Table API
description: Obtenga información sobre cómo funciona la distribución global en cuentas de Table API de Azure Cosmos DB y cómo configurar la lista de regiones preferida.
author: sakash279
ms.author: akshanka
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: tutorial
ms.date: 01/30/2020
ms.reviewer: sngun
ms.openlocfilehash: 627086bdb13acdd29821af399f90fee8deaae432
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "76900178"
---
# <a name="set-up-azure-cosmos-db-global-distribution-using-the-table-api"></a>Configuración de la distribución global de Azure Cosmos DB con Table API

En este artículo se tratan las tareas siguientes: 

> [!div class="checklist"]
> * Configuración de la distribución global con Azure Portal
> * Configuración de la distribución global con [Table API](table-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-table-api"></a>Conexión a una región de preferencia con Table API

Para aprovechar las ventajas de la [distribución global](distribute-data-globally.md), las aplicaciones cliente deben especificar la ubicación actual en la que se ejecuta la aplicación. Esto se hace mediante el establecimiento de la propiedad `CosmosExecutorConfiguration.CurrentRegion`. La propiedad `CurrentRegion` debe contener una sola ubicación. Cada instancia de cliente puede especificar su propia región para lecturas de baja latencia. Se debe asignar un nombre a la región con su [nombre para mostrar](https://msdn.microsoft.com/library/azure/gg441293.aspx), como "Oeste de EE. UU.". 

El SDK de Table API de Azure Cosmos DB elige automáticamente el mejor punto de conexión para comunicarse en función de la configuración de la cuenta y la disponibilidad regional actual. Prioriza la región más cercana para proporcionar una mejor latencia a los clientes. Después de establecer la propiedad `CurrentRegion` actual, las solicitudes de lectura y escritura se dirigen de la siguiente manera:

* **Solicitudes de lectura:** Todas las solicitudes de lectura se envían al valor de `CurrentRegion` configurado. En función de la proximidad, el SDK selecciona automáticamente una región con replicación geográfica de reserva para lograr la alta disponibilidad.

* **Solicitudes de escritura:** El SDK envía automáticamente todas las solicitudes de escritura a la región de escritura actual. En una cuenta de varios maestros, la región actual también atenderá las solicitudes de escritura. En función de la proximidad, el SDK selecciona automáticamente una región con replicación geográfica de reserva para lograr la alta disponibilidad.

Si no se especifica la propiedad `CurrentRegion`, el SDK usa la región de escritura actual para todas las operaciones.

Por ejemplo, una cuenta de Azure Cosmos se encuentra en las regiones "Oeste de EE. UU." y "Este de EE. UU.". "Oeste de EE. UU." es la región de escritura y la aplicación está presente en "Este de EE. UU.". Si no se configura la propiedad CurrentRegion, todas las solicitudes de lectura y escritura siempre se dirigen a la región "Oeste de EE. UU.". Si se configura la propiedad CurrentRegion, todas las solicitudes de lectura se sirven desde la región "Este de EE. UU.".

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha hecho lo siguiente:

> [!div class="checklist"]
> * Configuración de la distribución global con Azure Portal
> * Configurar la distribución global mediante Table API de Azure Cosmos DB

