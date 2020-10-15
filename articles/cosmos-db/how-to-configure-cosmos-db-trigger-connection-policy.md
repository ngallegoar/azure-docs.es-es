---
title: Desencadenador de Azure Functions para la directiva de conexión de Cosmos DB
description: Aprenda a configurar la directiva de conexión que utiliza el desencadenador de Azure Functions para Cosmos DB
author: ealsur
ms.service: cosmos-db
ms.topic: how-to
ms.date: 07/17/2019
ms.author: maquaran
ms.openlocfilehash: df30a0ddca58e6d7bd74184fa7287df6818cfc37
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84117141"
---
# <a name="how-to-configure-the-connection-policy-used-by-azure-functions-trigger-for-cosmos-db"></a>Configuración de la directiva de conexión que utiliza el desencadenador de Azure Functions para Cosmos DB

En este artículo se describe cómo se puede configurar la directiva de conexión cuando se usa el desencadenador de Azure Functions para Cosmos DB para conectarse a la cuenta de Azure Cosmos.

## <a name="why-is-the-connection-policy-important"></a>¿Por qué es importante la directiva de conexión?

Existen dos modos de conexión: modo directo y modo de puerta de enlace. Para más información acerca de estos modos de conexión, consulte el artículo acerca de las [sugerencias para obtener mayor rendimiento](./performance-tips.md#networking). De forma predeterminada, el modo de **puerta de enlace** se utiliza para establecer todas las conexiones en el desencadenador de Azure Functions para Cosmos DB. Sin embargo, es posible que no sea la mejor opción para escenarios en los que prime el rendimiento.

## <a name="changing-the-connection-mode-and-protocol"></a>Cambio del modo y protocolo de conexión

Hay dos valores de configuración clave disponibles para configurar la directiva de conexión de cliente: el **modo de conexión** y el **protocolo de conexión**. Tanto el modo de conexión como el protocolo predeterminados que usa el desencadenador de Azure Functions para Cosmos DB se pueden cambiar, al igual que todos los [enlaces de Azure Cosmos DB](../azure-functions/functions-bindings-cosmosdb-v2-output.md). Para cambiar la configuración predeterminada, es preciso localizar el archivo `host.json` en el proyecto de Azure Functions o en la aplicación de Azure Functions y agregar el siguiente [valor extra](../azure-functions/functions-bindings-cosmosdb-v2-output.md#hostjson-settings):

```js
{
  "cosmosDB": {
    "connectionMode": "Direct",
    "protocol": "Tcp"
  }
}
```

Donde `connectionMode` debe tener el modo de conexión deseado (directa o puerta de enlace) y `protocol` el protocolo de conexión deseado (Tcp o Https). 

Si el proyecto de Azure Functions funciona con el runtime de Azure Functions V1, la configuración tiene una pequeña diferencia en el nombre, debe usar `documentDB` en lugar de `cosmosDB`:

```js
{
  "documentDB": {
    "connectionMode": "Direct",
    "protocol": "Tcp"
  }
}
```

> [!NOTE]
> Cuando se trabaja con el plan de hospedaje Plan de consumo de Azure Functions, cada instancia tiene un límite de conexiones de socket que puede mantener. Cuando se trabaja con el modo directo/TCP, por diseño se crean más conexiones y se puede alcanzar el [límite del Plan de consumo](../azure-functions/manage-connections.md#connection-limit), en cuyo caso se puede utilizar el modo de puerta de enlace, o bien ejecutar Azure Functions en [modo App Service](../azure-functions/functions-scale.md#app-service-plan).

## <a name="next-steps"></a>Pasos siguientes

* [Límites de conexiones en Azure Functions](../azure-functions/manage-connections.md#connection-limit)
* [Sugerencias para mejorar el rendimiento de Azure Cosmos DB](./performance-tips.md)
* [Ejemplos de código](https://github.com/ealsur/serverless-recipes/tree/master/connectionmode)
