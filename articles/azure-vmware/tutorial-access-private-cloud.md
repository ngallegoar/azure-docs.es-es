---
title: 'Tutorial: Acceso a la nube privada'
description: Acceso a una nube privada de Azure VMware Solution (AVS)
ms.topic: tutorial
ms.date: 05/04/2020
ms.openlocfilehash: fcee332818c89d9c8b00795dca9e74f68260eefd
ms.sourcegitcommit: d9cd51c3a7ac46f256db575c1dfe1303b6460d04
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/04/2020
ms.locfileid: "82739590"
---
# <a name="tutorial-learn-how-to-access-an-azure-vmware-solution-avs-private-cloud"></a>Tutorial: Acceso a una nube privada de Azure VMware Solution (AVS)

Durante la versión preliminar, AVS no permite administrar la nube privada con la instancia local de vCenter. Tendrá que conectar y configurar adicionalmente una instancia local de vCenter mediante un JumpBox. 

En este tutorial, creará una máquina virtual Windows para un JumpBox en el grupo de recursos que creó en el tutorial anterior [Tutorial: Configuración de redes para la nube privada de VMWare en Azure](tutorial-configure-networking.md) e iniciará sesión en vCenter. Se trata de una máquina virtual que se encuentra en la misma red virtual que ha creado y que proporciona acceso al administrador NSX y vCenter. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una máquina virtual Windows para conectarse a vCenter
> * Iniciar sesión en vCenter desde la máquina virtual

## <a name="create-a-new-windows-virtual-machine"></a>Creación de una nueva máquina virtual Windows

En el grupo de recursos, seleccione **+ Agregar**, busque y seleccione **Microsoft Windows 10** y, a continuación, haga clic en **Crear**.

:::image type="content" source="./media/tutorial-access-private-cloud/ss8-azure-w10vm-create.png" alt-text="Adición de una nueva máquina virtual de Windows 10 para un JumpBox" border="true":::

Escriba la información requerida en los campos y seleccione **Revisar y crear**. Para más información sobre los campos, consulte la tabla siguiente.

| Campo | Value |
| --- | --- |
| **Suscripción** | Este valor ya se ha rellenado con la suscripción a la que pertenece el grupo de recursos. |
| **Grupos de recursos** | Este valor ya se ha rellenado para el grupo de recursos actual. Debería ser el grupo de recursos que creó en un tutorial anterior. |
| **Nombre de la máquina virtual** | Introduzca un nombre único para la máquina virtual. |
| **Región** | Seleccione la ubicación geográfica de la máquina virtual. |
| **Opciones de disponibilidad** | Deje el valor predeterminado seleccionado. |
| **Imagen** | Seleccione la imagen de la máquina virtual. |
| **Tamaño** | Deje el valor de tamaño predeterminado. |
| **Tipo de autenticación**  | Seleccione **Contraseña**. |
| **Nombre de usuario** | Escriba el nombre de usuario para iniciar sesión en la máquina virtual. |
| **Contraseña** | Escriba la contraseña para iniciar sesión en la máquina virtual. |
| **Confirmar contraseña** | Escriba la contraseña para iniciar sesión en la máquina virtual. |
| **Puertos de entrada públicos** | Seleccione **Ninguno**. Si selecciona Ninguno, puede usar el [acceso JIT](../security-center/security-center-just-in-time.md#configure-jit-access-from-an-azure-vms-page-) para controlar el acceso a la máquina virtual solo cuando desee acceder a ella.  |

Cuando haya especificado la información correspondiente, haga clic en **Revisar y crear**. Una vez superada la validación, seleccione **Crear** para iniciar el proceso de creación de la máquina virtual.

:::image type="content" source="./media/tutorial-access-private-cloud/ss11-review-create-wjb01.png" alt-text="Creación de una nueva máquina virtual de Windows 10 para un JumpBox" border="true":::

## <a name="connect-to-the-local-vcenter-of-your-private-cloud"></a>Conexión a la instancia local de vCenter de la nube privada

En el JumpBox, inicie sesión en el cliente de vSphere con SSO de VMware vCenter. Inicie sesión en el cliente de vSphere con un nombre de usuario de administrador de la nube, acepte el riesgo de seguridad y continúe cuando vea una advertencia sobre un posible riesgo de seguridad, inicie sesión en VMware vCenter con credenciales de inicio de sesión único y compruebe que la interfaz de usuario se muestra correctamente.

En Azure Portal, seleccione la nube privada y, a continuación, en la vista **Información general**, seleccione **Identidad > Predeterminada**. Se mostrarán las direcciones URL y las credenciales de inicio de sesión para el administrador de NSX-T y vCenter de la nube privada.

:::image type="content" source="./media/tutorial-access-private-cloud/ss4-display-identity.png" alt-text="Mostrar las direcciones URL y las credenciales del administrador de NSX y vCenter de la nube privada" border="true":::

Vaya a la máquina virtual que creó en el paso anterior y conéctese a ella. Para ver los pasos detallados para conectarse a la máquina virtual, consulte [Conexión a una máquina virtual](../virtual-machines/windows/connect-logon.md#connect-to-the-virtual-machine).

En la máquina virtual Windows, abra un explorador y vaya a las direcciones URL del administrador de NSX-T y vCenter en dos pestañas. En la pestaña de vCenter, escriba las credenciales del usuario `cloudadmin@vmcp.local` del paso anterior.

:::image type="content" source="./media/tutorial-access-private-cloud/ss5-vcenter-login.png" alt-text="Inicio de sesión en vCenter de la nube privada" border="true":::

:::image type="content" source="./media/tutorial-access-private-cloud/ss6-vsphere-client-home.png" alt-text="Portal de vCenter" border="true":::

En la segunda pestaña del explorador, inicie sesión en el administrador de NSX-T.

:::image type="content" source="./media/tutorial-access-private-cloud/ss10-nsx-manager-home.png" alt-text="Inicio del administrador de NSX de la nube privada local" border="true":::

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido cómo:

> [!div class="checklist"]
> * Crear una máquina virtual Windows para conectarse a vCenter
> * Iniciar sesión en vCenter desde la máquina virtual

Continúe con el siguiente tutorial para aprender a escalar la nube privada de AVS.

> [!div class="nextstepaction"]
> [Escalado de una nube privada de AVS](tutorial-scale-private-cloud.md)