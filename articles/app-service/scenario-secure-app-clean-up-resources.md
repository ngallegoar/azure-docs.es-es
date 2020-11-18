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
ms.openlocfilehash: ab91ea7aa6e621dabc5cac83fe818dbf175214b6
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94428393"
---
# <a name="tutorial-clean-up-resources"></a>Tutorial: Limpieza de recursos

Si ha completado todos los pasos de este tutorial de varias partes, habrá creado una aplicación de App Service, un plan de hospedaje de App Service y una cuenta de almacenamiento en un grupo de recursos.  También habrá creado un registro de aplicaciones en Azure AD.  Cuando ya no lo necesite, elimine estos recursos y el registro de aplicaciones para que no continúe acumulando gastos.  

En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> * Eliminar los recursos de Azure creados mientras se sigue el tutorial

## <a name="delete-the-resource-group"></a>Eliminar el grupo de recursos
En [Azure Portal](https://portal.azure.com), seleccione **Grupos de recursos** en el menú del portal y seleccione el grupo de recursos que contenga su instancia de App Service y el plan de esta.

Seleccione **Eliminar grupo de recursos** para eliminar el grupo de recursos y todos los recursos.

Eliminación de un grupo de recursos

La ejecución de este comando puede llevar varios minutos.

## <a name="delete-the-app-registration"></a>Eliminación del registro de aplicaciones
En el menú del portal, seleccione **Azure Active Directory**, después **Registros de aplicaciones** y, finalmente, la aplicación que ha creado.
:::image type="content" alt-text="Seleccionar registro de aplicaciones" source="./media/scenario-secure-app-clean-up-resources/select-app-registration.png":::

En la información general del registro de aplicaciones, seleccione **Eliminar**.
:::image type="content" alt-text="Eliminar registro de aplicaciones" source="./media/scenario-secure-app-clean-up-resources/delete-app-registration.png":::

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
>
> * Eliminar los recursos de Azure creados mientras se sigue el tutorial

Aprenda a conectar una [aplicación de .NET Core](tutorial-dotnetcore-sqldb-app.md), [Python](tutorial-python-postgresql-app.md), [Java](tutorial-java-spring-cosmosdb.md) o [Node.js](tutorial-nodejs-mongodb-app.md) a una base de datos.