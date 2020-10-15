---
title: 'Tutorial: Acceso a la nube privada'
description: Aprenda a acceder a una nube privada de Azure VMware Solution.
ms.topic: tutorial
ms.date: 09/21/2020
ms.openlocfilehash: b7f21dbba5256712607241bb1258962fc22418fb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91578432"
---
# <a name="tutorial-access-an-azure-vmware-solution-private-cloud"></a>Tutorial: Acceso a una nube privada de Azure VMware Solution

Azure VMware Solution no permite administrar la nube privada con la instancia local de vCenter. Tendrá que realizar una configuración y conexión adicionales a una instancia local de vCenter mediante un jumpbox. 

En este tutorial, creará un jumpbox en el grupo de recursos que creó en el [tutorial anterior](tutorial-configure-networking.md) e iniciará sesión en vCenter. El jumpbox es una máquina virtual (VM) de Windows en la misma red virtual que creó.  Proporciona acceso a vCenter y NSX Manager. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una máquina virtual Windows para conectarse a vCenter
> * Iniciar sesión en vCenter desde la máquina virtual

## <a name="create-a-new-windows-virtual-machine"></a>Creación de una nueva máquina virtual Windows

[!INCLUDE [create-avs-jump-box-steps](includes/create-jump-box-steps.md)]

## <a name="connect-to-the-local-vcenter-of-your-private-cloud"></a>Conexión a la instancia local de vCenter de la nube privada

1. En el jumpbox, inicie sesión en el cliente de vSphere con SSO de VMware vCenter con un nombre de usuario de administrador de la nube y compruebe que la interfaz de usuario se muestra correctamente.

1. En Azure Portal, seleccione la nube privada y, a continuación, en la vista **Información general**, seleccione **Identidad > Predeterminada**. 

   Se muestran las direcciones URL y las credenciales de usuario para vCenter y NSX-T Manager de la nube privada.

   :::image type="content" source="media/tutorial-access-private-cloud/ss4-display-identity.png" alt-text="Mostrar las direcciones URL y las credenciales del administrador de NSX y vCenter de la nube privada" border="true":::.

1. Vaya a la máquina virtual que creó en el paso anterior y conéctese a ella. 

   Si necesita ayuda para conectarse a la máquina virtual, consulte [Conexión a una máquina virtual](../virtual-machines/windows/connect-logon.md#connect-to-the-virtual-machine) para los detalles.

1. En la máquina virtual Windows, abra un explorador y vaya a las direcciones URL del administrador de NSX-T y vCenter en dos pestañas. 

1. En la pestaña de vCenter, escriba las credenciales del usuario `cloudadmin@vmcp.local` del paso anterior.

   :::image type="content" source="media/tutorial-access-private-cloud/ss5-vcenter-login.png" alt-text="Mostrar las direcciones URL y las credenciales del administrador de NSX y vCenter de la nube privada" border="true":::.

   :::image type="content" source="media/tutorial-access-private-cloud/ss6-vsphere-client-home.png" alt-text="Mostrar las direcciones URL y las credenciales del administrador de NSX y vCenter de la nube privada" border="true":::.

1. En la segunda pestaña del explorador, inicie sesión en el administrador de NSX-T.

   :::image type="content" source="media/tutorial-access-private-cloud/ss10-nsx-manager-home.png" alt-text="Mostrar las direcciones URL y las credenciales del administrador de NSX y vCenter de la nube privada" border="true":::



## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido cómo:

> [!div class="checklist"]
> * Crear una máquina virtual Windows para conectarse a vCenter
> * Iniciar sesión en vCenter desde la máquina virtual

Continúe con el siguiente tutorial para aprender a crear una red virtual a fin de configurar la administración local de los clústeres de nube privada.

> [!div class="nextstepaction"]
> [Creación de una red virtual](tutorial-configure-networking.md)


