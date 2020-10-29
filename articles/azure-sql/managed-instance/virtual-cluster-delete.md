---
title: Eliminación de una subred después de eliminar una instancia administrada de SQL Managed Instance
description: Aprenda a eliminar una red virtual de Azure después de eliminar una instancia administrada de Azure SQL Managed Instance.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: seo-lt-2019, sqldbrb=1
ms.devlang: ''
ms.topic: how-to
author: danimir
ms.author: danil
ms.reviewer: douglas, sstein
ms.date: 06/26/2019
ms.openlocfilehash: 4ed8f6dc90debddd17282f8f96962ffd78055030
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92791671"
---
# <a name="delete-a-subnet-after-deleting-a-managed-instance-of-sql-managed-instance"></a>Eliminación de una subred después de eliminar una instancia administrada de SQL Managed Instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

En este artículo se proporcionan instrucciones sobre cómo eliminar manualmente una subred después de eliminar la última instancia administrada de Azure SQL Managed Instance que reside en ella.

Las instancias administradas se implementan en [clústeres virtuales](connectivity-architecture-overview.md#virtual-cluster-connectivity-architecture). Cada clúster virtual está asociado a una subred. El clúster virtual persiste intencionadamente durante 12 horas después de la eliminación de la última instancia, para que pueda crear más rápidamente instancias administradas en la misma subred. No hay ningún cargo por mantener un clúster virtual vacío. Durante este período, no se puede eliminar la subred asociada con el clúster virtual.

Si no desea esperar 12 horas y prefiere eliminar el clúster virtual y su subred más rápidamente, puede hacerlo de forma manual. Elimine manualmente el clúster virtual mediante Azure Portal o la API de clústeres virtuales.

> [!IMPORTANT]
> - Para que la eliminación se realice correctamente, el clúster virtual no debe contener ninguna instancia administrada. 
> - La eliminación de un clúster virtual es una operación de ejecución prolongada que requiere aproximadamente 1,5 horas (consulte [Operaciones de administración de SQL Managed Instance](./sql-managed-instance-paas-overview.md#management-operations) para actualizar el tiempo de eliminación del clúster virtual). El clúster virtual seguirá visible en el portal hasta que se complete este proceso.

## <a name="delete-a-virtual-cluster-from-the-azure-portal"></a>Eliminación de un clúster virtual desde Azure Portal

Para eliminar un clúster virtual mediante Azure Portal, busque los recursos del clúster virtual.

![Captura de pantalla de Azure Portal con el cuadro de búsqueda resaltado](./media/virtual-cluster-delete/virtual-clusters-search.png)

Una vez que localice el clúster virtual que desea eliminar, seleccione este recurso y seleccione **Eliminar** . Se le pedirá que confirme la eliminación del clúster virtual.

![Captura de pantalla del panel de clústeres virtuales de Azure Portal con la opción Eliminar resaltada](./media/virtual-cluster-delete/virtual-clusters-delete.png)

Las notificaciones de Azure Portal le mostrarán una confirmación de que la solicitud para eliminar el clúster virtual se ha enviado correctamente. La operación de eliminación en sí durará aproximadamente 1,5 horas, durante las cuales el clúster virtual seguirá visible en el portal. Una vez completado el proceso, el clúster virtual dejará de estar visible y se liberará la subred asociada a él para su reutilización.

> [!TIP]
> Si no se muestran instancias administradas en el clúster virtual y no puede eliminar el clúster virtual, asegúrese de que no tiene una implementación de instancia en curso. Esto incluye las implementaciones iniciadas y canceladas que siguen en curso. Esto se debe a que estas operaciones aún utilizan el clúster virtual, lo que bloquea su eliminación. Revise la pestaña **Implementaciones** del grupo de recursos en el que se implementó la instancia para ver si hay implementaciones en curso. En este caso, espere a que la implementación finalice, elimine la instancia administrada y, a continuación, elimine el clúster virtual.

## <a name="delete-a-virtual-cluster-by-using-the-api"></a>Eliminación de un clúster virtual mediante la API

Para eliminar un clúster virtual mediante la API, utilice los parámetros de URI que se especifican en el [método de eliminación de clústeres virtuales](/rest/api/sql/virtualclusters/delete).

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información general, consulte [¿Qué es Azure SQL Managed Instance?](sql-managed-instance-paas-overview.md).
- Obtenga información sobre la [arquitectura de conectividad en Instancia administrada de SQL](connectivity-architecture-overview.md).
- Aprenda a [modificar una red virtual existente para Instancia administrada de SQL](vnet-existing-add-subnet.md).
- Para obtener un tutorial en el que se muestra cómo crear una red virtual y una instancia administrada, y restaurar una base de datos a partir de una copia de seguridad de base de datos, vea [Creación de una instancia administrada](instance-create-quickstart.md).
- Para problemas de DNS, consulte [Configuración de un DNS personalizado](custom-dns-configure.md).