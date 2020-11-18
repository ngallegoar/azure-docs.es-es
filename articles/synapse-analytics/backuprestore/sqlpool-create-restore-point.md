---
title: Creación de un punto de restauración definido por el usuario para un grupo de SQL dedicado
description: Cómo crear un punto de restauración para el grupo de SQL dedicado.
services: synapse-analytics
author: joannapea
manager: igorstan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 10/29/2020
ms.author: joanpo
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: c0835fb48d00fe5277732f34fd6b0de1448f6a78
ms.sourcegitcommit: 46c5ffd69fa7bc71102737d1fab4338ca782b6f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "94332021"
---
# <a name="user-defined-restore-points"></a>Puntos de restauración definidos por el usuario

En este artículo aprenderá a crear un punto de restauración definido por el usuario para un grupo de SQL dedicado en Azure Synapse Analytics mediante Azure Portal.

## <a name="create-user-defined-restore-points-through-the-azure-portal"></a>Creación de puntos de restauración definidos por el usuario mediante Azure Portal

Los puntos de restauración definidos por el usuario también se pueden crear mediante Azure Portal.

1. Inicie sesión en su cuenta de [Azure Portal](https://portal.azure.com/).

2. Vaya al grupo de SQL dedicado para el que quiere crear un punto de restauración.

3. Seleccione **Información general** en el panel izquierdo y **+ New restore point** (Nuevo punto de restauración). Si el botón Nuevo punto de restauración no está habilitado, asegúrese de que el grupo de SQL dedicado no está en pausa.

    ![Nuevo punto de restauración](../media/sql-pools/create-sqlpool-restore-point-01.png)

4. Especifique el nombre del punto de restauración definido por el usuario y haga clic en **Aplicar**. Los puntos de restauración definidos por el usuario tienen un período de retención predeterminado de siete días.

    ![Nombre de punto de restauración](../media/sql-pools/create-sqlpool-restore-point-02.png)

## <a name="next-steps"></a>Pasos siguientes

- [Restauración de un grupo de SQL dedicado](restore-sql-pool.md)

