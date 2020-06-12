---
title: Plantilla de ARM de punto de conexión privado de Azure
description: Información acerca de Azure Private Link
services: private-link
author: mblanco77
ms.service: private-link
ms.topic: article
ms.date: 05/26/2020
ms.author: allensu
ms.openlocfilehash: a91415e7e3d91c2950cc4df2235c3d58df284cc0
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2020
ms.locfileid: "84235983"
---
# <a name="create-a-private-endpoint---resource-manager-template"></a>Creación de un punto de conexión privado: plantilla de Resource Manager

En esta guía de inicio rápido, usará una plantilla de Resource Manager para crear un punto de conexión privado.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

También se puede completar este inicio rápido mediante [Azure Portal](create-private-endpoint-portal.md), [Azure PowerShell](create-private-endpoint-powershell.md) o la [CLI de Azure](create-private-endpoint-cli.md).

## <a name="prerequisites"></a>Requisitos previos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-an-private-endpoint"></a>Creación de un punto de conexión privado

Esta plantilla crea un punto de conexión privado para un servidor de Azure SQL.

### <a name="review-the-template"></a>Revisión de la plantilla

La plantilla usada en este inicio rápido forma parte de las [plantillas de inicio rápido de Azure](https://github.com/Azure/azure-quickstart-templates/blob/master/101-private-endpoint-sql/azuredeploy.json).

:::code language="json" source="~/quickstart-templates/101-private-endpoint-sql/azuredeploy.json" range="001-295" highlight="131-156":::

En la plantilla se definen varios recursos de Azure:

- [**Microsoft.Network/privateEndpoints**](/azure/templates/microsoft.network/privateendpoints): punto de conexión privado para acceder de forma privada a Azure SQL Server.
- [**Microsoft.Network/privateDnsZones**](/azure/templates/microsoft.network/privatednszones): se usa para resolver la dirección IP del punto de conexión privado.
- [**Microsoft.Network/privateDnsZones/virtualNetworkLinks**](/azure/templates/microsoft.network/privatednszones/virtualnetworklinks)
- [**Microsoft.Network/privateEndpoints/privateDnsZoneGroups**](/azure/templates/microsoft.network/privateendpoints/privateDnsZoneGroups): permite asociar el punto de conexión privado con una zona DNS privada.
- [**Microsoft.Sql/servers**](/azure/templates/microsoft.sql/servers): Azure SQL Server con la base de datos de ejemplo.
- [**Microsoft.Sql/servers/databases**](/azure/templates/microsoft.sql/servers/databases): base de datos de ejemplo.
- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks): instancia de Virtual Network donde se implementa el punto de conexión privado.
- [**Microsoft.Network/publicIpAddresses**](/azure/templates/microsoft.network/publicIpAddresses): dirección IP pública para acceder a la máquina virtual.
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines): máquina virtual para probar la conexión privada con un punto de conexión privado a Azure SQL Server.
- [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces): interfaz de red para la máquina virtual.

### <a name="deploy-the-template"></a>Implementación de la plantilla

Implementación de la plantilla de Resource Manager en Azure:

1. Seleccione **Implementar en Azure** para iniciar sesión en Azure y abrir la plantilla. La plantilla crea el punto de conexión privado, la instancia de Azure SQL Server, la infraestructura de red y las máquinas virtuales que se van a validar.

   [![Implementación en Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-private-endpoint-sql%2Fazuredeploy.json)

2. Elija su grupo de recursos o cree uno.
3. Escriba las credenciales de inicio de sesión y contraseña del administrador de SQL.
4. Escriba el nombre de usuario y la contraseña del administrador de la máquina virtual.
5. Seleccione **Acepto los términos y condiciones indicadas anteriormente** y, después, seleccione **Comprar**. La implementación puede tardar 20 minutos o más en completarse.

## <a name="validate-the-deployment"></a>Validación de la implementación

> [!NOTE]
> La plantilla de ARM genera un nombre único para el recurso myVm<b>{uniqueid}</b> de la máquina virtual y para el recurso sqlserver<b>{uniqueid}</b> de Azure SQL Server. Reemplace <b>{uniqueid}</b> por el valor generado.

### <a name="connect-to-a-vm-from-the-internet"></a>Conexión a una máquina virtual desde Internet

Conéctese a la VM _myVm{uniqueid}_ desde Internet de la siguiente manera:

1. En la barra de búsqueda del portal, escriba _myVm{uniqueid}_ .

2. Seleccione el botón **Conectar**. Después de seleccionar el botón **Conectar**, se abre **Conectar a máquina virtual**.

3. Seleccione **Descargar archivo RDP**. Azure crea un archivo de Protocolo de Escritorio remoto ( _.rdp_) y lo descarga en su equipo.

4. Abra el archivo .rdp\* descargado.

   a. Cuando se le pida, seleccione **Conectar**.

   b. Escriba el nombre de usuario y la contraseña que especificó al crear la VM.

      > [!NOTE]
      > Es posible que tenga que seleccionar **Más opciones** > **Usar otra cuenta** para especificar las credenciales que escribió al crear la máquina virtual.

5. Seleccione **Aceptar**.

6. Puede recibir una advertencia de certificado durante el proceso de inicio de sesión. Si recibe una advertencia de certificado, seleccione **Sí** o **Continuar**.

7. Una vez que aparezca el escritorio de la máquina virtual, minimícelo para volver a su escritorio local.

### <a name="access-sql-database-server-privately-from-the-vm"></a>Acceso al servidor de SQL Database de forma privada desde la máquina virtual

En esta sección, se conectará al servidor de SQL Database desde la máquina virtual mediante el punto de conexión privado.

1.  En el Escritorio remoto de _myVm{uniqueid}_ , abra PowerShell.
2.  Escriba nslookup sqlserver{uniqueid}.database.windows.net . Recibirá un mensaje similar al siguiente:

    ```
      Server:  UnKnown
      Address:  168.63.129.16
      Non-authoritative answer:
      Name:    sqlserver.privatelink.database.windows.net
      Address:  10.0.0.5
      Aliases:  sqlserver.database.windows.net
    ```

3.  Instalación de SQL Server Management Studio
4.  En Conectar con el servidor, escriba o seleccione esta información: Tipo de servidor: Seleccione Motor de base de datos.
    Nombre del servidor: Seleccione sqlserver{uniqueid}.database.windows.net. Nombre de usuario: escriba un nombre de usuario proporcionado durante la creación.
    Contraseña: escriba una contraseña proporcionada durante la creación.
    Recordar contraseña: seleccione Sí.

5.  Seleccione **Conectar**.
6.  Examine las **Bases de datos** en el menú izquierdo.
7.  (Opcionalmente) Cree o consulte la información de _sample-db_.
8.  Cierre la conexión de Escritorio remoto a _myVm{uniqueid}_ .

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite los recursos que ha creado con el punto de conexión privado, elimine el grupo de recursos. Esta acción quita el punto de conexión privado y todos los recursos relacionados.

Para eliminar el grupo de recursos, llame al cmdlet `Remove-AzResourceGroup`:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [Azure Private Link](private-link-overview.md).
