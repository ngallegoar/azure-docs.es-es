---
title: 'Azure App Service: Sincronización de la configuración de red'
description: En este artículo se describe cómo sincronizar la configuración de red para el plan de hospedaje de Azure App Service.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 12/13/2018
ms.openlocfilehash: 0065b5c7ddf8e19897f76aeb49300ec7a6809b30
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83772376"
---
# <a name="sync-networking-configuration-for-azure-app-service-hosting-plan"></a>Sincronización de la configuración de red para el plan de hospedaje de Azure App Service

Puede suceder que aunque haya [integrado la aplicación con una red de Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md) no pueda establecer conexión con Instancia administrada. Puede intentar actualizar la configuración de red para el plan de servicio que tenga.

## <a name="sync-network-configuration-for-app-service-hosting-plan"></a>Sincronización de la configuración de red para el plan de hospedaje de App Service

Para ello, sigue estos pasos:  

1. Vaya al plan de App Service de las aplicaciones web.

   ![plan de App Service](./media/sql-database-managed-instance-sync-networking/app-service-plan.png)

2. Haga clic en **Redes** y, a continuación, en **Haga clic aquí para administrar**.

   ![administración del plan de servicio](./media/sql-database-managed-instance-sync-networking/manage-plan.png)

3. Seleccione **Red virtual** y haga clic en **Sincronizar red**.

   ![sincronización de red](./media/sql-database-managed-instance-sync-networking/sync.png)

4. Espere hasta que se realice la sincronización.
  
   ![sincronización acabada](./media/sql-database-managed-instance-sync-networking/sync-done.png)

Ya está preparado para intentar volver a establecer la conexión con su instancia administrada.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre cómo configurar la red virtual para Instancia administrada, consulte [Arquitectura de conectividad de la red virtual de la Instancia administrada](sql-database-managed-instance-connectivity-architecture.md) y [Configuración de una red virtual existente](sql-database-managed-instance-configure-vnet-subnet.md).
