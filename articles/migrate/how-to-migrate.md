---
title: Incorporación de herramientas de migración en Azure Migrate
description: Aprenda a agregar herramientas de migración en Azure Migrate.
author: anvar-ms
ms.author: anvar
ms.manager: bsiva
ms.topic: how-to
ms.date: 11/23/2020
ms.openlocfilehash: 97051e97ec9868f6941b579241e16e62fdd2162b
ms.sourcegitcommit: ea551dad8d870ddcc0fee4423026f51bf4532e19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2020
ms.locfileid: "96751791"
---
# <a name="add-migration-tools"></a>Incorporación de herramientas de migración

En este artículo se describe cómo agregar herramientas de migración en [Azure Migrate](./migrate-services-overview.md).

- Si quiere agregar una herramienta de migración y todavía no ha configurado un proyecto de Azure Migrate, consulte este [artículo](create-manage-projects.md).
- Si ha agregado una herramienta de ISV para la migración, [siga los pasos](prepare-isv-movere.md) de preparación para trabajar con la herramienta.

## <a name="select-a-migration-scenario"></a>Selección de un escenario de migración

1. En el proyecto de Azure Migrate, haga clic en **Información general**.
2. Seleccione el escenario de migración que quiere usar:

    - Para migrar máquinas y cargas de trabajo a Azure, seleccione **Evaluar y migrar servidores**.
    - Para migrar bases de datos locales, seleccione **Evaluar y migrar bases de datos**.
    - Para migrar aplicaciones web locales, seleccione **Explorar más** > **Aplicaciones web**.
    - Para migrar datos a Azure mediante Data Box, seleccione **Explorar más** > **Data Box**.

    ![Opciones para seleccionar un escenario de migración](./media/how-to-migrate/migrate-scenario.png)


## <a name="select-a-server-migration-tool"></a>Selección de una herramienta de migración de servidores

1. Agregue una herramienta:

    - Si creó un proyecto de Azure Migrate a través de la opción **Evaluar y migrar servidores** en el portal, la herramienta Azure Migrate Server Migration se agrega automáticamente al proyecto. Para agregar más herramientas de migración, en **Servidores**, junto a **Herramientas de migración**, seleccione **Agregar más herramientas**.
    
         ![Botón para agregar herramientas de migración adicionales](./media/how-to-migrate/add-migration-tools.png)

    - Si creó un proyecto con una opción diferente y aún no tiene ninguna herramienta de migración, en **Servidores** > **Herramientas de migración**, seleccione **Haga clic aquí**.

    ![Botón para agregar las primeras herramientas de migración](./media/how-to-migrate/no-migration-tool.png)

2. En **Azure Migrate** > **Agregar herramientas**, seleccione las herramientas que quiera agregar. A continuación, seleccione **Agregar herramienta**.

    ![Selección de una herramienta de evaluación de la lista](./media/how-to-migrate/select-migration-tool.png)


## <a name="select-a-database-migration-tool"></a>Selección de una herramienta de migración de bases de datos

Si creó un proyecto de Azure Migrate a través de la opción **Evaluar y migrar bases de datos** en el portal, la herramienta Database Migration se agrega automáticamente al proyecto. 

1. Si la herramienta Database Migration no se encuentra en el proyecto, en **Bases de datos** > **Herramientas de evaluación**, seleccione **Haga clic aquí**.
    
    ![Adición de herramientas de migración de base de datos](./media/how-to-migrate/no-database-migration-tool.png)


2. En **Azure Migrate** > **Agregar herramientas**, seleccione la herramienta Database Migration. A continuación, seleccione **Agregar herramienta**.

    ![Selección de la herramienta de migración de bases de datos de la lista](./media/how-to-migrate/select-database-migration-tool.png)

    

## <a name="select-a-web-app-migration-tool"></a>Selección de una herramienta de migración de aplicaciones web

Si creó un proyecto de Azure Migrate a través de la opción **Explorar más** > **Web Apps** en el portal, la herramienta Web App Migration se agrega automáticamente al proyecto. 

1. Si la herramienta Web App Migration no está en el proyecto, en **Web Apps** > **Herramientas de evaluación**, seleccione **Haga clic aquí**.

    ![Adición de herramientas de migración de aplicaciones web](./media/how-to-migrate/no-web-app-migration-tool.png)
 

2. En **Azure Migrate** > **Agregar herramientas**, seleccione la herramienta Web App Migration. A continuación, seleccione **Agregar herramienta**.

    ![Selección de herramientas de evaluación de aplicaciones web de la lista](./media/how-to-migrate/select-web-app-migration-tool.png)


## <a name="order-an-azure-data-box"></a>Solicitud de una instancia de Azure Data Box

Para migrar grandes cantidades de datos a Azure, puede solicitar una instancia de Azure Data Box para la transferencia de datos sin conexión.

1. En **Información general**, seleccione **Explorar más**.
2. En **Explorar más**, seleccione **Data Box**.
3. En **Introducción a Data Box**, seleccione la suscripción y el grupo de recursos que quiere usar para solicitar una instancia de Data Box.
4. El **Tipo de transferencia** es una importación a Azure. Especifique el país en el que residen los datos y la región de Azure a la que quiere transferir los datos. 
5. Haga clic en **Aplicar** para guardar la configuración.

## <a name="next-steps"></a>Pasos siguientes

Pruebe la migración con la herramienta Azure Migrate Server Migration para máquinas virtuales de [Hyper-V](tutorial-migrate-hyper-v.md) o [VMware](tutorial-migrate-vmware.md).
