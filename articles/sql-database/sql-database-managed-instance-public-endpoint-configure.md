---
title: 'Configuración de un punto de conexión público: instancia administrada'
description: Obtenga información sobre cómo configurar un punto de conexión público para una instancia administrada
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto, carlrab
ms.date: 05/07/2019
ms.openlocfilehash: 1acd7d6a3b203997e3acd8d7959b1572e09845f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79228604"
---
# <a name="configure-public-endpoint-in-azure-sql-database-managed-instance"></a>Configurar un punto de conexión público en una instancia administrada de Azure SQL Database

El punto de conexión público para una [instancia administrada](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index) habilita el acceso a datos de su instancia administrada desde fuera de la [red virtual](../virtual-network/virtual-networks-overview.md). Puede acceder a su instancia administrada desde servicios de Azure de varios inquilinos, como Power BI, Azure App Service o una red local. Al usar el punto de conexión público en una instancia administrada, no es necesario que use una VPN, lo que puede contribuir a evitar problemas de rendimiento de la VPN.

En este artículo, aprenderá a:

> [!div class="checklist"]
> - Habilitar un punto de conexión público para su instancia administrada en Azure Portal.
> - Habilitar un punto de conexión público para su instancia administrada usando PowerShell.
> - Configurar un grupo de seguridad de red en la instancia administrada para permitir el tráfico al punto de conexión público de la instancia administrada.
> - Obtener la cadena de conexión del punto de conexión público en la instancia administrada.

## <a name="permissions"></a>Permisos

Debido a la confidencialidad de los datos de una instancia administrada, la configuración para habilitar el punto de conexión público de la instancia administrada requiere un proceso de dos pasos. Esta medida de seguridad se ajusta a la separación de tareas (SoD):

- El administrador de la instancia administrada debe habilitar el punto de conexión público en la instancia administrada. Dicho administrador se puede encontrar en la página **Introducción** del recurso de la instancia administrada de SQL.
- Un administrador de red debe permitir el tráfico usando un grupo de seguridad de red. Para más información, vea los [permisos para el grupo de seguridad de red](../virtual-network/manage-network-security-group.md#permissions).

## <a name="enabling-public-endpoint-for-a-managed-instance-in-the-azure-portal"></a>Habilitar un punto de conexión público para una instancia administrada en Azure Portal

1. Inicie Azure Portal en <https://portal.azure.com/.>.
1. Abra el grupo de recursos con la instancia administrada y seleccione la **instancia administrada de SQL** en la que quiera configurar el punto de conexión público.
1. En la configuración de **seguridad**, seleccione la pestaña **Red virtual**.
1. En la página de configuración de la red virtual, seleccione **Habilitar** y después el icono **Guardar** para actualizar la configuración.

![mi-vnet-config.png](media/sql-database-managed-instance-public-endpoint-configure/mi-vnet-config.png)

## <a name="enabling-public-endpoint-for-a-managed-instance-using-powershell"></a>Habilitar un punto de conexión público para una instancia administrada usando PowerShell

### <a name="enable-public-endpoint"></a>Habilitar el punto de conexión público

Ejecute los comandos de PowerShell siguientes. Reemplace **subscription-id** por su identificador de suscripción. Reemplace también **rg-name** por el grupo de recursos de la instancia administrada y **mi-name** por el nombre de la instancia administrada.

```powershell
Install-Module -Name Az

Import-Module Az.Accounts
Import-Module Az.Sql

Connect-AzAccount

# Use your subscription ID in place of subscription-id below

Select-AzSubscription -SubscriptionId {subscription-id}

# Replace rg-name with the resource group for your managed instance, and replace mi-name with the name of your managed instance

$mi = Get-AzSqlInstance -ResourceGroupName {rg-name} -Name {mi-name}

$mi = $mi | Set-AzSqlInstance -PublicDataEndpointEnabled $true -force
```

### <a name="disable-public-endpoint"></a>Deshabilitar el punto de conexión público

Para deshabilitar el punto de conexión público con PowerShell, deberá ejecutar el comando siguiente (además, no olvide cerrar el NSG para el puerto de entrada 3342 si lo tiene configurado):

```powershell
Set-AzSqlInstance -PublicDataEndpointEnabled $false -force
```

## <a name="allow-public-endpoint-traffic-on-the-network-security-group"></a>Permitir el tráfico del punto de conexión público en el grupo de seguridad de red

1. Si todavía tiene abierta la página de configuración de la instancia administrada, vaya a la pestaña **Introducción**. En caso contrario, vuelva a su recurso **Instancia administrada de SQL**. Seleccione el vínculo **Red virtual/subred**, que le llevará a la página de configuración de la red virtual.

    ![mi-overview.png](media/sql-database-managed-instance-public-endpoint-configure/mi-overview.png)

1. Seleccione la pestaña **Subredes** en el panel de configuración izquierdo de la red virtual y anote el **GRUPO DE SEGURIDAD** de su instancia administrada.

    ![mi-vnet-subnet.png](media/sql-database-managed-instance-public-endpoint-configure/mi-vnet-subnet.png)

1. Vuelva al grupo de recursos que contiene la instancia administrada. Debería ver el nombre del **grupo de seguridad de red** anotado anteriormente. Seleccione el nombre que va a introducir en la página de configuración del grupo de seguridad de red.

1. Seleccione la pestaña **Reglas de seguridad de entrada** y **agregue** una regla que tenga mayor prioridad que la regla **deny_all_inbound** con la siguiente configuración: </br> </br>

    |Configuración  |Valor sugerido  |Descripción  |
    |---------|---------|---------|
    |**Origen**     |Cualquier dirección IP o etiqueta de servicio         |<ul><li>En servicios de Azure como Power BI, seleccione la etiqueta de servicio en la nube de Azure.</li> <li>En su equipo o máquina virtual de Azure, use la dirección IP de NAT.</li></ul> |
    |**Intervalos de puertos de origen**     |*         |Deje esta opción en * (cualquiera), puesto que los puertos de origen suelen asignarse de forma dinámica y, por lo tanto, son impredecibles. |
    |**Destino**     |Any         |Deje la opción Destino en Cualquiera para permitir el tráfico en la subred de la instancia administrada. |
    |**Intervalos de puertos de destino**     |3342         |Establezca el puerto de destino en 3342, que es el punto de conexión TDS público de la instancia administrada. |
    |**Protocolo**     |TCP         |La instancia administrada utiliza el protocolo TCP para TDS. |
    |**Acción**     |Allow         |Permite el tráfico entrante en la instancia administrada mediante el punto de conexión público. |
    |**Prioridad**     |1300         |Asegúrese de que esta regla tiene más prioridad que la regla **deny_all_inbound**. |

    ![mi-nsg-rules.png](media/sql-database-managed-instance-public-endpoint-configure/mi-nsg-rules.png)

    > [!NOTE]
    > El puerto 3342 se usa para las conexiones del punto de conexión público a la instancia administrada y no se puede cambiar en este momento.

## <a name="obtaining-the-managed-instance-public-endpoint-connection-string"></a>Obtener la cadena de conexión del punto de conexión público en la instancia administrada

1. Vaya a la página de configuración de la instancia administrada de SQL que se ha habilitado para el punto de conexión público. Seleccione la pestaña **Cadenas de conexión** en **Configuración**.
1. El nombre de host del punto de conexión público debe tener el formato <mi_name>.**public**.<dns_zone>.database.windows.net y el puerto usado para la conexión debe ser 3342.

    ![mi-public-endpoint-conn-string.png](media/sql-database-managed-instance-public-endpoint-configure/mi-public-endpoint-conn-string.png)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [utilizar una instancia administrada de Azure SQL Database de forma segura con puntos de conexión públicos](sql-database-managed-instance-public-endpoint-securely.md).