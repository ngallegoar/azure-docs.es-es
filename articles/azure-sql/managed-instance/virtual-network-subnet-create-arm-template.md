---
title: Creación de una red virtual
titleSuffix: Azure SQL Managed Instance
description: En este artículo se describe cómo crear una red virtual configurada para admitir la implementación de Instancia administrada de Azure SQL.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 09/12/2019
ms.openlocfilehash: 923f8b447b1103284b2c999a981826ef19a1c7d8
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84039266"
---
# <a name="create-a-virtual-network-for-azure-sql-managed-instance"></a>Creación de una red virtual para Instancia administrada de Azure SQL
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

En este artículo se explica cómo crear una red virtual y una subred válidas en las que puede implementar una Instancia administrada de Azure SQL.

Instancia administrada de Azure SQL debe implementarse dentro de una [red virtual](../../virtual-network/virtual-networks-overview.md) de Azure. Esta implementación permite los siguientes escenarios:

- Protección de una dirección IP privada
- Conexión a Instancia administrada de SQL directamente desde una red local
- Conexión de Instancia administrada de SQL con un servidor vinculado o con otro almacén de datos local
- Conexión de Instancia administrada de SQL a los recursos de Azure  

> [!NOTE]
> Debería [determinar el tamaño de la subred para la Instancia administrada de SQL](vnet-subnet-determine-size.md) antes de implementar la primera instancia. No se puede cambiar el tamaño de la subred después de colocar los recursos en su interior.
>
> Si piensa usar una red virtual existente, tiene que modificar la configuración de red para dar cabida a Instancia administrada de SQL. Para más información, consulte [Modificación de una red virtual existente para Instancia administrada de SQL](vnet-existing-add-subnet.md).
>
> Después de crear una Instancia administrada de SQL, no se admite el traslado de dicha Instancia administrada de SQL o la red virtual a otro grupo de recursos o suscripción.  Tampoco se admite el traslado de la Instancia administrada de SQL a otra subred.
>

## <a name="create-a-virtual-network"></a>Creación de una red virtual

La manera más fácil de crear y configurar una red virtual es usar una plantilla de implementación de Azure Resource Manager.

1. Inicie sesión en Azure Portal.

2. Seleccione el botón **Implementar en Azure**:

   <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener" data-linktype="external"> <img src="https://azuredeploy.net/deploybutton.png" data-linktype="external"> </a>

   Este botón abre un formulario que puede usar para configurar el entorno de red, donde puede implementar la Instancia administrada de SQL.

   > [!Note]
   > Esta plantilla de Azure Resource Manager implementará una red virtual con dos subredes. Una subred, llamada **ManagedInstances**, está reservada para Instancia administrada de SQL y tiene una tabla de rutas configuradas previamente. La otra subred, denominada **Default**, se usa para otros recursos que deberían acceder a Instancia administrada de SQL (por ejemplo, Azure Virtual Machines).

3. Configure el entorno de red. En el formulario siguiente, puede configurar parámetros de su entorno de red:

   ![Plantilla de Resource Manager para configurar la red de Azure](./media/virtual-network-subnet-create-arm-template/create-mi-network-arm.png)

   Puede cambiar los nombres de la red virtual y las subredes, y ajustar los intervalos IP asociados a sus recursos de red. Una vez que presione el botón **Comprar**, se creará el formulario y configurará el entorno. Si no necesita dos subredes, puede eliminar la predeterminada.

## <a name="next-steps"></a>Pasos siguientes

- Para información general, consulte [¿Qué es Instancia administrada de SQL?](sql-managed-instance-paas-overview.md)
- Obtenga información sobre la [arquitectura de conectividad en Instancia administrada de SQL](connectivity-architecture-overview.md).
- Aprenda a [modificar una red virtual existente para Instancia administrada de SQL](vnet-existing-add-subnet.md).
- Para obtener un tutorial que muestre cómo crear una red virtual, crear una Instancia administrada de SQL y restaurar una base de datos desde una copia de seguridad de base de datos, consulte [Creación de una Instancia administrada de Azure SQL](instance-create-quickstart.md).
- Para ver información relativa a los distintos problemas de DNS, consulte [Configuración de un DNS personalizado](custom-dns-configure.md).
