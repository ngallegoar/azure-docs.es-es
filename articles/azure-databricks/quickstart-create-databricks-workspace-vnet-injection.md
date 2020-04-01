---
title: 'Inicio rápido: Creación de un área de trabajo de Azure Databricks en su red virtual propia'
description: En este artículo se describe cómo implementar Azure Databricks en la red virtual.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: conceptual
ms.date: 03/23/2020
ms.openlocfilehash: 63dd1b4d9396d340dd17a7afb92ff9c38a2b38b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80132670"
---
# <a name="quickstart-create-an-azure-databricks-workspace-in-your-own-virtual-network"></a>Inicio rápido: Creación de un área de trabajo de Azure Databricks en su red virtual propia

La implementación predeterminada de Azure Databricks crea una nueva red virtual administrada por Databricks. En este inicio rápido se muestra cómo crear un área de trabajo de Azure Databricks en una red virtual propia. También creará un clúster de Apache Spark dentro de esa área de trabajo. 

Para más información sobre el razón por la que puede optar por crear un área de trabajo de Azure Databricks en su propia red virtual, consulte [Implementación de Azure Databricks en Azure Virtual Network (inyección de VNet)](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/databricks/).

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en [Azure Portal](https://portal.azure.com/).

> [!Note]
> Este tutorial no puede llevarse a cabo mediante una **suscripción de evaluación gratuita de Azure**.
> Si tiene una cuenta gratuita, vaya a su perfil y cambiar la suscripción a **pago por uso**. Para más información consulte el sitio de [cuentas gratuitas de Azure](https://azure.microsoft.com/free/). Después, [quite el límite de gasto](https://docs.microsoft.com/azure/billing/billing-spending-limit#why-you-might-want-to-remove-the-spending-limit) y [solicite un aumento de la cuota](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request) para las vCPU de su región. Cuando crea su área de trabajo de Azure Databricks, puede seleccionar el plan de tarifa de la **Trial (Premium - 14-Days Free DBUs)** para que el área de trabajo acceda a las DBU Premium de Azure Databricks gratis durante 14 días.

## <a name="create-a-virtual-network"></a>Creación de una red virtual

1. En el menú de Azure Portal, seleccione **Crear un recurso**. A continuación, seleccione **Redes > Red virtual**.

    ![Creación de una red virtual en Azure Portal](./media/quickstart-create-databricks-workspace-vnet-injection/create-virtual-network-portal.png)

2. En **Creación de una red virtual**, aplique las opciones siguientes: 

    |Configuración|Valor sugerido|Descripción|
    |-------|---------------|-----------|
    |Subscription|\<Su suscripción\>|Seleccione la suscripción de Azure que quiera usar.|
    |Resource group|databricks-quickstart|Seleccione **Crear nuevo** y escriba un nuevo nombre de grupo de recursos para la cuenta.|
    |Nombre|databricks-quickstart|Seleccione un nombre para la red virtual.|
    |Region|\<Seleccione la región más cercana a los usuarios\>|Seleccione la ubicación geográfica donde puede hospedar la red virtual. Use la ubicación más cercana a los usuarios.|

    ![Fundamentos de una red virtual en Azure Portal](./media/quickstart-create-databricks-workspace-vnet-injection/create-virtual-network.png)

3. Seleccione **Siguiente: Direcciones IP >** y aplique la configuración siguiente. Después, seleccione **Revisar y crear**.
    
    |Configuración|Valor sugerido|Descripción|
    |-------|---------------|-----------|
    |Espacio de direcciones IPv4|10.2.0.0/16|El intervalo de direcciones de la red virtual en notación CIDR. El intervalo de CIDR debe estar comprendido entre /16 y /24.|
    |Nombre de subred|default|Seleccione un nombre para la subred predeterminada en la máquina virtual.|
    |Intervalo de direcciones de subred|10.2.0.0/24|El intervalo de direcciones de la subred en notación CIDR. Debe incluirse en el espacio de direcciones de la red virtual. El intervalo de direcciones de una subred que está en uso no se puede modificar.|

    ![Establecimiento de configuraciones de direcciones IP para una red virtual en Azure Portal](./media/quickstart-create-databricks-workspace-vnet-injection/create-virtual-network-ip-config.png)

4. En la pestaña **Revisar y crear**, seleccione **Crear** para implementar la red virtual. Una vez completada la implementación, vaya a la red virtual y seleccione **Espacio de direcciones** bajo **Configuración**. En el cuadro que dice *Agregar otro intervalo de direcciones*, inserte `10.179.0.0/16` y seleccione **Guardar**.

    ![Espacios de direcciones de red virtual de Azure](./media/quickstart-create-databricks-workspace-vnet-injection/add-address-space.png)

## <a name="create-an-azure-databricks-workspace"></a>Creación de un área de trabajo de Azure Databricks

1. En el menú de Azure Portal, seleccione **Crear un recurso**. A continuación, seleccione **Analytics > Databricks**.

    ![Creación de un área de trabajo de Azure Databricks en Azure Portal](./media/quickstart-create-databricks-workspace-vnet-injection/create-databricks-workspace-portal.png)

2. En **Servicio Azure Databricks**, proporcione los valores siguientes:

    |Configuración|Valor sugerido|Descripción|
    |-------|---------------|-----------|
    |Nombre del área de trabajo|databricks-quickstart|Seleccione un nombre para el área de trabajo de Azure Databricks.|
    |Subscription|\<Su suscripción\>|Seleccione la suscripción de Azure que quiera usar.|
    |Resource group|databricks-quickstart|Seleccione el mismo grupo de recursos que usó para la red virtual.|
    |Location|\<Seleccione la región más cercana a los usuarios\>|Seleccione la misma ubicación que la de la red virtual.|
    |Nivel de precios|Elija entre Standard o Premium.|Para más información sobre estos planes de tarifa, consulte la [página de precios de Databricks](https://azure.microsoft.com/pricing/details/databricks/).|

    ![Creación de un área de trabajo de Azure Databricks básica](./media/quickstart-create-databricks-workspace-vnet-injection/create-databricks-workspace.png)

3. Una vez que haya terminado de especificar la configuración en la página **Basics** (Datos básicos), seleccione **Siguiente: Redes >** y aplique la configuración siguiente:

    |Configuración|Valor sugerido|Descripción|
    |-------|---------------|-----------|
    |Implementar área de trabajo de Azure Databricks en una red virtual (Vnet) propia|Sí|Este parámetro le permite implementar un área de trabajo de Azure Databricks en la red virtual.|
    |Virtual Network|databricks-quickstart|Seleccione la red virtual que creó en el paso anterior.|
    |Nombre de la subred pública|public-subnet|Use el nombre de la subred pública predeterminado.|
    |Intervalo de CIDR de la subred pública|10.179.64.0/18|Use un intervalo de CIDR hasta /26, inclusive.|
    |Nombre de la subred privada|private-subnet|Use el nombre de la subred privada predeterminado.|
    |Intervalo de CIDR de la subred privada|10.179.0.0/18|Use un intervalo de CIDR hasta /26, inclusive.|

    ![Incorporación de información de red de área virtual a un área de trabajo de Azure Databricks en Azure Portal](./media/quickstart-create-databricks-workspace-vnet-injection/create-databricks-workspace-vnet-config.png)

3. Una vez completada la implementación, navegue hasta el recurso de Azure Databricks. Observe que el emparejamiento de redes virtuales está deshabilitado. Observe también el grupo de recursos y el grupo de recursos administrados en la página de información general. 

    ![Información general de Azure Databricks en Azure Portal](./media/quickstart-create-databricks-workspace-vnet-injection/databricks-overview-portal.png)

    El grupo de recursos administrados no se puede modificar y no se usa para crear máquinas virtuales. Solo puede crear máquinas virtuales en el grupo de recursos que administra.

    ![Grupo de recursos administrados de Azure Databricks](./media/quickstart-create-databricks-workspace-vnet-injection/managed-resource-group.png)

    Cuando se produce un error en una implementación del área de trabajo, esta todavía se crea en un estado de error. Elimine el área de trabajo con errores y cree una nueva que resuelva los errores de implementación. Al eliminar el área de trabajo con errores, también se eliminan el grupo de recursos administrados y los recursos implementados correctamente.

## <a name="create-a-cluster"></a>Crear un clúster

> [!NOTE]
> Para usar una cuenta gratuita para crear el clúster de Azure Databricks, antes de crear el clúster, vaya a su perfil y cambie su suscripción a **pago por uso**. Para más información consulte el sitio de [cuentas gratuitas de Azure](https://azure.microsoft.com/free/).

1. Vuelva al servicio de Azure Databricks y seleccione **Launch Workspace** (Iniciar área de trabajo) en la página de **información general**.

2. Seleccione **Clústeres** >  **+ Crear clúster**. A continuación, cree un nombre de clúster, como *databricks-quickstart-cluster*, y acepte el resto de valores predeterminados. Seleccione **Create cluster** (Crear clúster).

    ![Creación de un clúster de Azure Databricks](./media/quickstart-create-databricks-workspace-vnet-injection/create-cluster.png)

3. Una vez que se esté ejecutando el clúster, vuelva al grupo de recursos administrados de Azure Portal. Observe las nuevas máquinas virtuales, discos, dirección IP e interfaces de red. Se crea una interfaz de red en cada una de las subredes públicas y privadas con direcciones IP.  

    ![Grupo de recursos administrados de Azure Databricks tras la creación del clúster](./media/quickstart-create-databricks-workspace-vnet-injection/managed-resource-group2.png)

4. Vuelva al área de trabajo Azure Databricks y seleccione el clúster que creó. A continuación, navegue hasta la pestaña **Executors** (Ejecutores) de la página de la **interfaz de usuario de Spark**. Tenga en cuenta que las direcciones para el controlador y los ejecutores están en el intervalo de subred privada. En este ejemplo, el controlador es 10.179.0.6 y los ejecutores son 10.179.0.4 y 10.179.0.5. Sus direcciones IP podrían ser diferentes.

    ![Ejecutores de la IU de Spark de Azure Databricks](./media/quickstart-create-databricks-workspace-vnet-injection/databricks-sparkui-executors.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado el artículo, puede finalizar el clúster. Para ello, desde el área de trabajo de Azure Databricks, en el panel izquierdo, seleccione **Clusters** (Clústeres). En el clúster que desea finalizar, mueva el cursor sobre el botón de puntos suspensivos en la columna **Actions** (Acciones) y seleccione el icono **Terminate** (Finalizar). Esto detiene el clúster.

Si no finaliza manualmente el clúster, este se detendrá automáticamente si seleccionó la casilla **Terminate after \_\_ minutes of inactivity** (Finalizar después de \_\_ minutos de inactividad) al crear el clúster. En tal caso, el clúster se detiene automáticamente si ha estado inactivo durante el tiempo especificado.

Si no desea volver a usar el clúster, puede eliminar el grupo de recursos que creó en Azure Portal.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha creado un clúster de Spark en Azure Databricks que ha implementado en una red virtual. Avance al siguiente artículo para obtener información sobre cómo consultar un contenedor de Docker de Linux de SQL Server en la red virtual mediante JDBC desde un cuaderno de Azure Databricks.  

> [!div class="nextstepaction"]
>[Query a SQL Server Linux Docker container in a virtual network from an Azure Databricks notebook](vnet-injection-sql-server.md) (Consulta de un contenedor de Docker de Linux para SQL Server en una red virtual mediante un cuaderno de Azure Databricks)
