---
title: archivo de inclusión
description: archivo de inclusión
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/05/2019
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 609b62312329b3a8f9f16d15458a0a47f5eb377f
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "85115221"
---
Ahora puede usar consultas en Data Explorer para recuperar y filtrar los datos.

1. En la parte superior de la pestaña **Elementos** de Data Explorer, examine la consulta predeterminada `SELECT * FROM c`. Esta consulta recupera y muestra todos los documentos del contenedor ordenados por identificador. 
   
   :::image type="content" source="./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-query.png" alt-text="La consulta predeterminada del Explorador de datos es SELECT * FROM c":::
   
1. Para cambiar la consulta, seleccione **Editar filtro**, reemplace la consulta predeterminada por `ORDER BY c._ts DESC`y, después, seleccione **Aplicar filtro**.
   
   :::image type="content" source="./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edit-query.png" alt-text="La consulta predeterminada del Explorador de datos es SELECT * FROM c":::

   Esta consulta modificada muestra los documentos en orden descendente según su marca de tiempo, por lo que ahora el segundo documento aparece en primer lugar. 
   
   :::image type="content" source="./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edited-query.png" alt-text="La consulta predeterminada del Explorador de datos es SELECT * FROM c":::

Si está familiarizado con la sintaxis de SQL, puede especificar cualquiera de las [consultas SQL](../articles/cosmos-db/sql-api-sql-query.md) admitidas en el cuadro de predicado de consulta. También puede usar Data Explorer para crear procedimientos almacenados, UDF y desencadenadores para la lógica de negocios del servidor. 

Data Explorer proporciona un acceso sencillo en Azure Portal a todas las características de acceso a datos mediante programación integrado que está disponible en las API. Use también el portal para escalar el rendimiento, obtener las claves y cadenas de conexión, y revisar las métricas y los contratos de nivel de servicio de la cuenta de Azure Cosmos DB. 

