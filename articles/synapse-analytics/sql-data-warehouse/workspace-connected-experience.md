---
title: Habilitación de las características del área de trabajo de Synapse en un grupo de SQL dedicado (anteriormente, SQL DW)
description: En este documento se describe cómo un cliente puede acceder a la instancia independiente existente de SQL DW y utilizarla en el área de trabajo.
services: synapse-analytics
author: antvgski
manager: igorstan
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 11/23/2020
ms.author: anvang
ms.reviewer: jrasnick
ms.openlocfilehash: 5efb1df378df323585bc0ca1094451cdb095fe4e
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96499788"
---
# <a name="enabling-synapse-workspace-features-on-an-existing-dedicated-sql-pool-formerly-sql-dw"></a>Habilitación de las características del área de trabajo de Synapse en un grupo existente de SQL dedicado (anteriormente, SQL DW)

Ahora, todos los clientes de SQL Data Warehouse pueden acceder a una instancia existente de un grupo de SQL dedicado (anteriormente, SQL DW) y utilizarla mediante Synapse Studio y el área de trabajo, sin afectar a la automatización, las conexiones ni las herramientas. En este artículo se explica cómo un cliente existente de Azure Synapse Analytics puede crear y ampliar su solución existente de Analytics con las nuevas funcionalidades repletas de características que ahora están disponibles en el área de trabajo de Synapse y Studio.   

## <a name="experience"></a>Experiencia
 
Ahora que el área de trabajo de Synapse se encuentra disponible con carácter general, existe una nueva funcionalidad en la sección de información general del portal de DW que le permite crear un área de trabajo de Synapse para las instancias existentes del grupo de SQL dedicado (anteriormente, SQL DW). Esta nueva funcionalidad le permitirá conectar el servidor lógico que hospeda las instancias de almacenamiento de datos existentes a una nueva área de trabajo de Synapse. La conexión garantiza que se pueda acceder a todos los almacenamientos de datos hospedados en ese servidor desde el área de trabajo y Studio, y que se puedan utilizar junto con los servicios de asociados de Synapse (grupo de SQL sin servidor, grupo de Apache Spark y ADF). Puede empezar a acceder a los recursos y utilizarlos en cuanto se hayan completado los pasos de aprovisionamiento y después de que se haya establecido la conexión al área de trabajo recién creada.  
:::image type="content" source="media/workspace-connected-overview/workspace-connected-dw-portal-overview-pre-create.png" alt-text="Área de trabajo conectada de Synapse":::

## <a name="using-synapse-workspace-and-studio-features-to-access-and-use-a-dedicated-sql-pool-formerly-sql-dw"></a>Uso del área de trabajo de Synapse y Studio para acceder a un grupo de SQL dedicado (anteriormente, SQL DW) y utilizarlo
 
La siguiente información se aplicará al utilizar una instancia dedicada de SQL DW (anteriormente, SQL DW) con las características del área de trabajo de Synapse habilitadas: 
- **Funcionalidades de SQL**: todas las funcionalidades de SQL se conservarán con el servidor SQL lógico una vez que se haya habilitado la característica del área de trabajo de Synapse. El acceso al servidor mediante el proveedor de recursos de SQL seguirá siendo posible una vez que se haya habilitado el área de trabajo. Todas las funciones de administración se pueden iniciar a través del área de trabajo, y la operación se llevará a cabo en el servidor SQL lógico que hospeda los grupos de SQL. Ninguna automatización, herramienta o conexión existente se interrumpirá cuando se habilite un área de trabajo.  
- **Movimiento de recursos**: iniciar un movimiento de recursos en un servidor con la característica del área de trabajo de Synapse habilitada romperá el vínculo entre el servidor y el área de trabajo, y ya no podrá acceder a las instancias existentes del grupo de SQL dedicado (anteriormente, SQL DW) desde el área de trabajo. Para garantizar que se mantenga la conexión, se recomienda que ambos recursos permanezcan en la misma suscripción y en el mismo grupo de recursos. 
- **Supervisión**: las solicitudes de SQL enviadas mediante Synapse Studio en un área de trabajo habilitada para un grupo de SQL dedicado (anteriormente, SQL DW) se pueden ver en el centro de supervisión. Para todas las demás actividades de supervisión, puede ir a la supervisión de grupos de SQL dedicados de Azure Portal (anteriormente, SQL DW). 
- **Controles de seguridad** y **acceso**: como se indicó anteriormente, todas las funciones de administración de las instancias de SQL Server y de los grupos de SQL dedicados (anteriormente, SQL DW) seguirán residiendo en el servidor SQL lógico. Estas funciones incluyen la administración de reglas de firewall, la configuración del administrador de Azure AD del servidor y todo el control de acceso de los datos en el grupo de SQL dedicado (anteriormente, SQL DW). Se deben completar los pasos siguientes para asegurarse de que se puede acceder al grupo de SQL dedicado (anteriormente, SQL DW) y que se puede utilizar mediante el área de trabajo de Synapse. Las pertenencias a roles del área de trabajo no conceden a los usuarios permisos para los datos de las instancias del grupo de SQL dedicado (anteriormente, SQL DW). Siga las directivas de [autenticación de SQL](sql-data-warehouse-authentication.md) habituales para asegurarse de que los usuarios puedan acceder a las instancias del grupo de SQL dedicado (anteriormente, SQL DW) en el servidor lógico. 

    ```sql
    CREATE USER [<workspace managed identity] FROM EXTERNAL PROVIDER 
    GRANT CONTROL ON DATABASE:: <dedicated SQL pool name> TO [<workspace managed identity>
    ```

    > [!NOTE] 
    > En Synapse Studio en el área de trabajo conectada se mostrarán los nombres de los grupos dedicados basados en los permisos que el usuario tiene en Azure. No se podrá acceder a los grupos si el usuario no tiene permisos en los grupos de SQL. 

- **Seguridad de red**: si el área de trabajo de Synapse que ha habilitado en el grupo de SQL dedicado existente (anteriormente, SQL DW) está habilitada para la protección de infiltración de datos. Cree una conexión de punto de conexión privado administrada desde el área de trabajo hasta el servidor SQL lógico. Apruebe la solicitud de conexión de punto de conexión privado para permitir las comunicaciones entre el servidor y el área de trabajo.
- Grupos de SQL de **Studio** en el **centro de datos**: un área de trabajo habilitada para un grupo de SQL dedicado (anteriormente, SQL DW) se identifica mediante la información sobre herramientas del centro de datos. 
- **Creación de un grupo de SQL dedicado (anteriormente, SQL DW)** : se pueden crear grupos de SQL dedicados mediante el área de trabajo de Synapse y Studio una vez habilitada la característica del área de trabajo, y el aprovisionamiento de un nuevo grupo se realizará en el servidor SQL lógico. Los nuevos recursos aparecerán en el portal y en Studio cuando se complete el aprovisionamiento.      

## <a name="next-steps"></a>Pasos siguientes
Habilitación de las [características del área de trabajo de Synapse](workspace-connected-create.md) en el grupo existente de SQL dedicado (anteriormente, SQL DW)
