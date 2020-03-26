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
ms.openlocfilehash: 64c900e01496dad0d5f6a96d8d790ae0c2a8c95a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "78927352"
---
Ahora puede usar consultas en Data Explorer para recuperar y filtrar los datos.

1. En la parte superior de la pestaña **Elementos** de Data Explorer, examine la consulta predeterminada `SELECT * FROM c`. Esta consulta recupera y muestra todos los documentos de la colección por orden de identificador. 
   
   ![La consulta predeterminada en el Explorador de datos es `SELECT * FROM c`](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-query.png)
   
1. Para cambiar la consulta, seleccione **Editar filtro**, reemplace la consulta predeterminada por `ORDER BY c._ts DESC`y, después, seleccione **Aplicar filtro**.
   
   ![Cambio de la consulta predeterminada agregando ORDER BY c._ts DESC y haciendo clic en Aplicar filtro](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edit-query.png)

   Esta consulta modificada muestra los documentos en orden descendente según su marca de tiempo, por lo que ahora el segundo documento aparece en primer lugar. 
   
   ![Consulta cambiada a ORDER BY c._ts DESC y haciendo clic en Aplicar filtro](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edited-query.png)

Si está familiarizado con la sintaxis de SQL, puede especificar cualquiera de las [consultas SQL](../articles/cosmos-db/sql-api-sql-query.md) admitidas en el cuadro de predicado de consulta. También puede usar Data Explorer para crear procedimientos almacenados, UDF y desencadenadores para la lógica de negocios del servidor. 

Data Explorer proporciona un acceso sencillo en Azure Portal a todas las características de acceso a datos mediante programación integrado que está disponible en las API. Use también el portal para escalar el rendimiento, obtener las claves y cadenas de conexión, y revisar las métricas y los contratos de nivel de servicio de la cuenta de Azure Cosmos DB. 

