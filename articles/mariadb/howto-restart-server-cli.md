---
title: Reinicio de un servidor mediante la CLI de Azure en Azure Database for MariaDB
description: En este artículo se describe cómo reiniciar un servidor de Azure Database for MariaDB mediante la CLI de Azure.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: how-to
ms.date: 3/18/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 5ccbd0790692f065cc313b5f95d5944ad3ff716b
ms.sourcegitcommit: 5831eebdecaa68c3e006069b3a00f724bea0875a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94515428"
---
# <a name="restart-azure-database-for-mariadb-server-using-the-azure-cli"></a>Reinicio de un servidor de Azure Database for MariaDB mediante la CLI de Azure
En este tema se describe cómo reiniciar un servidor de Azure Database for MariaDB. Es posible que deba reiniciar el servidor por motivos de mantenimiento, lo que causa una breve interrupción del servicio mientras el servidor realiza la operación.

Si el servicio está ocupado, se bloqueará el reinicio del servidor. Por ejemplo, el servicio puede estar procesando una operación solicitada anteriormente, como el escalado de núcleos virtuales.

El tiempo necesario para completar un reinicio depende el proceso de recuperación de MariaDB. Para reducir el tiempo de reinicio, se recomienda que minimice la cantidad de actividades que se ejecutan en el servidor antes del reinicio.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía de procedimientos:

- Necesita un [servidor de Azure Database for MariaDB](quickstart-create-mariadb-server-database-using-azure-cli.md).
 
[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

- En este artículo se necesita la versión 2.0 o posterior de la CLI de Azure. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.


## <a name="restart-the-server"></a>Reinicio del servidor

Reinicie el servidor con el siguiente comando:

```azurecli-interactive
az mariadb server restart --name mydemoserver --resource-group myresourcegroup
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre [cómo establecer parámetros en Azure Database for MariaDB](howto-configure-server-parameters-cli.md)
