---
title: 'Tutorial: Limpieza de recursos | Azure'
description: En este tutorial aprenderá a limpiar los recursos de Azure asignados al crear la aplicación web.
services: storage, app-service-web
author: rwike77
manager: CelesteDG
ms.service: app-service-web
ms.topic: tutorial
ms.workload: identity
ms.date: 10/27/2020
ms.author: ryanwi
ms.reviewer: stsoneff
ms.custom: azureday1
ms.openlocfilehash: bf03fe9d920298d49e79a9a0febf7e09e94eb6ff
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2020
ms.locfileid: "96905526"
---
# <a name="tutorial-clean-up-resources"></a>Tutorial: Limpieza de recursos

Si ha completado todos los pasos de este tutorial de varias partes, habrá creado una aplicación de App Service, un plan de hospedaje de App Service y una cuenta de almacenamiento en un grupo de recursos. También habrá creado un registro de aplicaciones en Azure Active Directory. Cuando ya no lo necesite, elimine estos recursos y el registro de aplicaciones para que no continúe acumulando gastos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> * Eliminar los recursos de Azure creados mientras seguía el tutorial.

## <a name="delete-the-resource-group"></a>Eliminar el grupo de recursos

En [Azure Portal](https://portal.azure.com), seleccione **Grupos de recursos** en el menú del portal y seleccione el grupo de recursos que contenga su instancia de App Service y el plan de esta.

Seleccione **Eliminar grupo de recursos** para eliminar el grupo de recursos y todos los recursos.

:::image type="content" alt-text="Captura de pantalla que muestra la eliminación del grupo de recursos." source="./media/scenario-secure-app-clean-up-resources/delete-resource-group.png":::

Este comando puede tardar varios minutos en ejecutarse.

## <a name="delete-the-app-registration"></a>Eliminación del registro de aplicaciones

En el menú del portal, seleccione **Azure Active Directory** > **Registros de aplicaciones**. A continuación, seleccione la aplicación que ha creado.
:::image type="content" alt-text="Captura de pantalla que muestra la selección del registro de aplicaciones." source="./media/scenario-secure-app-clean-up-resources/select-app-registration.png":::

En la información general del registro de aplicaciones, seleccione **Eliminar**.
:::image type="content" alt-text="Captura de pantalla que muestra la eliminación del registro de aplicaciones." source="./media/scenario-secure-app-clean-up-resources/delete-app-registration.png":::

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a:

> [!div class="checklist"]
>
> * Eliminar los recursos de Azure creados mientras seguía el tutorial.

Aprenda a conectar una [aplicación de .NET Core](tutorial-dotnetcore-sqldb-app.md), [Python](tutorial-python-postgresql-app.md), [Java](tutorial-java-spring-cosmosdb.md) o [Node.js](tutorial-nodejs-mongodb-app.md) a una base de datos.