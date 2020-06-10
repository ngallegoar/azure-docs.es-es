---
title: Plantilla de Resource Manager del servicio Azure Private Link
description: Plantilla de Resource Manager del servicio Azure Private Link
services: private-link
author: mblanco77
ms.service: private-link
ms.topic: article
ms.date: 05/29/2020
ms.author: allensu
ms.openlocfilehash: 93a66057ddb0034f7ac9ac62578292ca38f2d2fe
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2020
ms.locfileid: "84236414"
---
# <a name="create-a-private-link-service---resource-manager-template"></a>Creación de un servicio Private Link: plantilla de Resource Manager

En este inicio rápido, usará una plantilla de Resource Manager para crear un servicio Private Link.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

También se puede completar este inicio rápido mediante [Azure Portal](create-private-link-service-portal.md), [Azure PowerShell](create-private-link-service-powershell.md) o la [CLI de Azure](create-private-link-service-cli.md).

## <a name="prerequisites"></a>Requisitos previos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-a-private-link-service"></a>Creación de un servicio Private Link

Esta plantilla crea un servicio Private Link.

### <a name="review-the-template"></a>Revisión de la plantilla

La plantilla usada en este inicio rápido forma parte de las [plantillas de inicio rápido de Azure](https://github.com/Azure/azure-quickstart-templates/blob/master/101-privatelink-service/azuredeploy.json).

:::code language="json" source="~/quickstart-templates/101-privatelink-service/azuredeploy.json" range="001-432" highlight="263-289":::

En la plantilla se definen varios recursos de Azure:

- [**Microsoft.Network/privateLinkServices**](/azure/templates/microsoft.network/privateLinkServices): servicio Private Link para exponer el servicio de forma privada
- [**Microsoft.Network/privateendpoints**](/azure/templates/microsoft.network/privateendpoints): punto de conexión privado para acceder de forma privada al servicio
- [**Microsoft.Network/loadBalancers**](/azure/templates/microsoft.network/loadBalancers): equilibrador de carga que expone las máquinas virtuales en las que se hospeda el servicio
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines): dos máquinas virtuales, una en la que se hospeda el servicio y otra para probar la conexión al punto de conexión privado
- [**Microsoft.Compute/virtualMachines/extensions**](/azure/templates/Microsoft.Compute/virtualMachines/extensions): extensión que instala el servidor web
- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks): una para cada máquina virtual
- [**Microsoft.Network/publicIpAddresses**](/azure/templates/microsoft.network/publicIpAddresses): dos direcciones IP públicas, una para cada máquina virtual
- [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces): dos interfaces de red, una para cada máquina virtual

### <a name="deploy-the-template"></a>Implementación de la plantilla

Implementación de la plantilla de Resource Manager en Azure:

1. Seleccione **Implementar en Azure** para iniciar sesión en Azure y abrir la plantilla. La plantilla crea una máquina virtual, un equilibrador de carga estándar, un servicio Private Link, un punto de conexión privado, redes y una máquina virtual para validar.

   [![Implementación en Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-privatelink-service%2Fazuredeploy.json)

2. Seleccione el grupo de recursos o cree uno.
3. Escriba el nombre de usuario y la contraseña del administrador de la máquina virtual.
4. Seleccione **Acepto los términos y condiciones indicadas anteriormente** y, después, seleccione **Comprar**.

## <a name="validate-the-deployment"></a>Validación de la implementación

> [!NOTE]
> La plantilla de Resource Manager genera un nombre único para el recurso myConsumerVm<b>{uniqueid}</b> de la máquina virtual; reemplace <b>{uniqueid}</b> por el valor generado.

### <a name="connect-to-a-vm-from-the-internet"></a>Conexión a una máquina virtual desde Internet

Conéctese a la máquina virtual _myConsumerVm{uniqueid}_ desde Internet de esta forma:

1.  En la barra de búsqueda del portal, escriba _myConsumerVm{uniqueid}_ .

2.  Seleccione el botón **Conectar**. Después de seleccionar el botón **Conectar**, se abre **Conectar a máquina virtual**.

3.  Seleccione **Descargar archivo RDP**. Azure crea un archivo de Protocolo de Escritorio remoto ( _.rdp_) y lo descarga en su equipo.

4.  Abra el archivo .rdp\* descargado.

    a. Cuando se le pida, seleccione **Conectar**.

    b. Escriba el nombre de usuario y la contraseña que especificó al crear la VM.
    
    > [!NOTE]
    > Es posible que tenga que seleccionar **Más opciones** > **Usar otra cuenta** para especificar las credenciales que escribió al crear la máquina virtual.

5.  Seleccione **Aceptar**.

6.  Puede recibir una advertencia de certificado durante el proceso de inicio de sesión. Si recibe una advertencia de certificado, seleccione **Sí** o **Continuar**.

7.  Una vez que aparezca el escritorio de la máquina virtual, minimícelo para volver a su escritorio local.

### <a name="access-the-http-service-privately-from-the-vm"></a>Acceso al servicio http de forma privada desde la máquina virtual

En esta sección, se conectará al servicio http desde la máquina virtual mediante el punto de conexión privado.

1.  En el Escritorio remoto de _myConsumerVm{uniqueid}_
2.  Abra un explorador y escriba la dirección del punto de conexión privado http://10.0.0.5/
3.  Verá la página predeterminada de IIS.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite los recursos que ha creado con el servicio Private Link, elimine el grupo de recursos. Esta acción quita el servicio Private Link y todos los recursos relacionados.

Para eliminar el grupo de recursos, llame al cmdlet `Remove-AzResourceGroup`:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [Azure Private Link](private-link-overview.md).
