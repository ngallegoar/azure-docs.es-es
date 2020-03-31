---
title: Incorporación de herramientas de migración en Azure Migrate
description: Aprenda a agregar herramientas de migración en Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.manager: carmonm
ms.topic: article
ms.date: 11/18/2019
ms.author: raynew
ms.openlocfilehash: 6bcb65912627356215769ccc8380ee1a7404f6f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74185956"
---
# <a name="add-migration-tools"></a>Incorporación de herramientas de migración

En este artículo se describe cómo agregar herramientas de migración en [Azure Migrate](migrate-overview.md).

Azure Migrate proporciona un centro de herramientas para la valoración y la migración a Azure. Incluye herramientas nativas, herramientas proporcionadas por otros servicios de Azure y ofertas de fabricantes de software independientes (ISV).

Si quiere agregar una herramienta de migración y todavía no ha configurado un proyecto de Azure Migrate, consulte este [artículo](how-to-add-tool-first-time.md).



## <a name="selecting-an-isv-tool"></a>Seleccionar una herramienta de ISV

Si elige una [herramienta de ISV](migrate-services-overview.md#isv-integration) para la migración, empiece por obtener una licencia o registrarse para una evaluación gratuita, de acuerdo con la directiva de ISV. En cada herramienta, existe una opción para conectarse a Azure Migrate. Implemente la herramienta y siga sus instrucciones y documentación para conectar el área de trabajo de la herramienta con Azure Migrate. 

## <a name="select-a-migration-scenario"></a>Selección de un escenario de migración

1. En el proyecto de Azure Migrate, haga clic en **Información general**.
2. Seleccione el escenario de migración que quiere usar:

    - Para migrar máquinas y cargas de trabajo a Azure, seleccione **Evaluar y migrar servidores**.
    - Para migrar máquinas SQL locales, seleccione **Evaluar y migrar bases de datos**.
    - Para migrar aplicaciones web locales, seleccione **Evaluar y migrar aplicaciones web**.
    - Para migrar grandes cantidades de datos locales a Azure en el modo sin conexión, seleccione **Solicitar una instancia de Data Box**.

    ![Escenario de valoración](./media/how-to-migrate/assess-scenario.png)

## <a name="select-a-server-migration-tool"></a>Selección de una herramienta de migración de servidores

1. Haga clic en **Evaluar y migrar servidores**.
2. En **Azure Migrate - Servidores**, si aún no ha agregado una herramienta de migración, vaya a **Herramientas de migración** y seleccione **Click here to add a migration tool** (Haga clic aquí para agregar una herramienta de migración). Si ya ha agregado herramientas, en **¿Quiere agregar más herramientas de migración?** , seleccione **Cambiar**.

    > [!NOTE]
    > Si necesita desplazarse a otro proyecto, en **Azure Migrate - Servidores**, junto a **¿Quiere ver los detalles de un proyecto de migración diferente?** , seleccione **Haga clic aquí**.

3. En **Azure Migrate**, seleccione la herramienta de migración que quiere usar.
    - Si usa Azure Migrate Server Migration, puede configurar y ejecutar las migraciones directamente en el proyecto de Azure Migrate.
    - Si usa una herramienta de valoración de terceros, vaya al vínculo proporcionado para el ISV y ejecute la migración de acuerdo con las instrucciones que se proporcionan.

## <a name="select-a-database-migration-tool"></a>Selección de una herramienta de migración de bases de datos

1. Haga clic en **Evaluar y migrar bases de datos**.
2. En **Bases de datos**, haga clic en **Agregar herramientas**.
3. En Agregar una herramienta > **Seleccione una herramienta de migración**, elija la herramienta que quiere usar para migrar la base de datos.

## <a name="select-a-web-app-migration-tool"></a>Selección de una herramienta de migración de aplicaciones web

1. Haga clic en **Evaluar y migrar aplicaciones web**.
2. Siga el vínculo a la herramienta de migración para Azure App Service. Use la herramienta de migración para:

    - **Evaluar aplicaciones web en línea**: puede evaluar y migrar las aplicaciones con una dirección URL pública en línea mediante Migration Assistant de Azure App Service.
    - **.NET/PHP**: para las aplicaciones .NET y PHP internas, puede descargar y ejecutar Migration Assistant.

## <a name="order-an-azure-data-box"></a>Solicitud de una instancia de Azure Data Box

Para migrar grandes cantidades de datos a Azure, puede solicitar una instancia de Azure Data Box para la transferencia de datos sin conexión.

1. Haga clic en **Solicitar una instancia de Data Box**.
2. En **Seleccione su instancia de Azure Data Box**, especifique su suscripción. 
3. La transferencia será una importación a Azure. Especifique el origen de datos y la región de Azure de destino para los datos.

## <a name="next-steps"></a>Pasos siguientes

Pruebe la migración con Azure Migrate Server Migration para máquinas virtuales de [Hyper-V](tutorial-migrate-hyper-v.md) o [VMware](tutorial-migrate-vmware.md).
