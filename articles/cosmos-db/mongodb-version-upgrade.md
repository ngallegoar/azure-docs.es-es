---
title: Actualización de la versión de Mongo de la cuenta de API de Azure Cosmos DB para MongoDB
description: Procedimiento para actualizar sin problemas la versión del protocolo de conexión de MongoDB para las cuentas de API de Azure Cosmos DB para MongoDB existentes
author: jasonwhowell
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: guide
ms.date: 09/22/2020
ms.author: jasonh
ms.openlocfilehash: c6369be39d0a964f07c64083e3269bb1c0c49c7f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91409670"
---
# <a name="upgrade-the-mongodb-wire-protocol-version-of-your-azure-cosmos-dbs-api-for-mongodb-account"></a>Actualización de la versión del protocolo de conexión de MongoDB de la cuenta de API de Azure Cosmos DB para MongoDB

En este artículo se describe cómo actualizar la versión del protocolo de conexión de la cuenta de API de Azure Cosmos DB para MongoDB. Después de actualizar la versión del protocolo de conexión, puede usar la funcionalidad más reciente de la API de Azure Cosmos DB para MongoDB. El proceso de actualización no interrumpe la disponibilidad de la cuenta y no consume RU/s ni reduce la capacidad de la base de datos en ningún momento. Este proceso no afectará a los índices o datos existentes.

>[!Note]
> En este momento, solo las cuentas aptas que usan la versión 3.2 del servidor se pueden actualizar a la versión 3.6. Si la cuenta no muestra la opción de actualización, [abra una incidencia de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="upgrading-from-version-32-to-36"></a>Actualización de la versión 3.2 a la 3.6

### <a name="benefits-of-upgrading-to-version-36"></a>Ventajas de la actualización a la versión 3.6

A continuación se describen las nuevas características incluidas en la versión 3.6:
- Rendimiento y estabilidad mejorada
- Compatibilidad con los nuevos comandos de base de datos
- Compatibilidad con la canalización de agregación de forma predeterminada y nuevas fases de agregación
- Compatibilidad con flujos de cambios
- Compatibilidad con índices compuestos
- Compatibilidad entre particiones para las operaciones siguientes: actualización, eliminación, recuento y ordenación
- Rendimiento mejorado para las siguientes operaciones de agregado: $count, $skip, $limit y $group
- Ahora se admite la indexación con caracteres comodín

### <a name="changes-from-version-32"></a>Cambios con respecto a la versión 3.2

- **Se han quitado los errores RequestRateIsLarge**. Las solicitudes de la aplicación cliente ya no devolverán errores 16500. En su lugar, las solicitudes se reanudarán hasta que finalicen o cumplan el tiempo de espera.
- El tiempo de espera de cada solicitud se establece en 60 segundos.
- Las colecciones de MongoDB creadas con la nueva versión del protocolo de conexión solo tendrán la propiedad `_id` indexada de forma predeterminada.

### <a name="action-required"></a>Acción requerida

Para la actualización a la versión 3.6, el sufijo del punto de conexión de la cuenta de base de datos se actualizará al formato siguiente:

```
<your_database_account_name>.mongo.cosmos.azure.com
```

Tendrá que reemplazar el punto de conexión existente en las aplicaciones y los controladores que se conectan con esta cuenta de base de datos. **Solo las conexiones que usan el nuevo punto de conexión tendrán acceso a las características de la versión 3.6 de MongoDB**. El punto de conexión anterior debe tener el sufijo `.documents.azure.com`.

>[!Note]
> Es posible que este punto de conexión tenga ligeras diferencias si la cuenta se ha creado en una nube de Azure soberana, gubernamental o restringida.

### <a name="how-to-upgrade"></a>Procedimiento de actualización

1. Primero, vaya a Azure Portal y navegue hasta la hoja Información general de la cuenta API de Azure Cosmos DB para MongoDB. Compruebe que la versión del servidor es `3.2`. 

    :::image type="content" source="./media/mongodb-version-upgrade/1.png" alt-text="Azure Portal con la información general de la cuenta de MongoDB" border="false":::

2. En las opciones de la izquierda, seleccione la hoja `Features`. Esto revelará las características de nivel de cuenta que están disponibles para la cuenta de base de datos.

    :::image type="content" source="./media/mongodb-version-upgrade/2.png" alt-text="Azure Portal con la información general de la cuenta de MongoDB" border="false":::

3. Haga clic en la fila `Upgrade to Mongo server version 3.6`. Si no ve esta opción, es posible que la cuenta no sea válida para esta actualización. En ese caso, abra una [incidencia de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

    :::image type="content" source="./media/mongodb-version-upgrade/3.png" alt-text="Azure Portal con la información general de la cuenta de MongoDB" border="false":::

4. Revise la información que se muestra sobre esta actualización específica. Tenga en cuenta que la actualización solo se completará hasta que las aplicaciones usen el punto de conexión actualizado, como se destaca en esta sección. Haga clic en `Enable` cuando esté listo para iniciar el proceso.

    :::image type="content" source="./media/mongodb-version-upgrade/4.png" alt-text="Azure Portal con la información general de la cuenta de MongoDB" border="false":::

5. Después de iniciar el proceso, en el menú `Features` se mostrará el estado de la actualización. El estado pasará de `Pending` a `In Progress` y luego a `Upgraded`. Este proceso no afectará a la funcionalidad u operaciones existentes de la cuenta de base de datos.

    :::image type="content" source="./media/mongodb-version-upgrade/5.png" alt-text="Azure Portal con la información general de la cuenta de MongoDB" border="false":::

6. Una vez que se haya completado la actualización, el estado se mostrará como `Upgraded`. Haga clic para obtener más información sobre los pasos y las acciones que debe realizar para finalizar el proceso. Póngase en contacto con el [soporte técnico](https://azure.microsoft.com/en-us/support/create-ticket/) si se produce alguna incidencia al procesar la solicitud.

    :::image type="content" source="./media/mongodb-version-upgrade/6.png" alt-text="Azure Portal con la información general de la cuenta de MongoDB" border="false":::

7. **Para empezar a usar la versión actualizada de la cuenta de base de datos**, vuelva a la hoja `Overview` y copie la nueva cadena de conexión para usarla en la aplicación. Las aplicaciones comenzarán a usar la versión actualizada en cuanto se conecten al nuevo punto de conexión. Las conexiones existentes no se interrumpirán y se pueden actualizar como prefiera. Para garantizar una experiencia coherente, todas las aplicaciones deben usar el nuevo punto de conexión.

    :::image type="content" source="./media/mongodb-version-upgrade/7.png" alt-text="Azure Portal con la información general de la cuenta de MongoDB" border="false":::

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre las [características admitidas y no admitidas de la versión 3.6 de MongoDB](mongodb-feature-support-36.md).
- Para más información, consulte las [características de la versión 3.6 de Mongo](https://devblogs.microsoft.com/cosmosdb/azure-cosmos-dbs-api-for-mongodb-now-supports-server-version-3-6/)
