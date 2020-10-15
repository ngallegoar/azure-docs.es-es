---
title: Configuración de la preferencia de enrutamiento de una VM - Azure Portal
description: Obtenga información sobre cómo crear una VM con una dirección IP pública con preferencia de enrutamiento mediante Azure Portal.
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2020
ms.author: mnayak
ms.openlocfilehash: af3d9e9fcf0dad6a5e51a3db87b63567d701970e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84687997"
---
# <a name="configure-routing-preference-for-a-vm-using-the-azure-portal"></a>Configuración de la preferencia de enrutamiento de una VM mediante Azure Portal

En este artículo se muestra cómo configurar las preferencias de enrutamiento para una máquina virtual. El tráfico de Internet enlazado desde la VM se enrutará a través de la red del ISP cuando elija **Internet** como opción de preferencia de enrutamiento. El enrutamiento predeterminado se hará a través de la red global de Microsoft.

En este artículo se muestra cómo crear una máquina virtual con una dirección IP pública que esté configurada para enrutar el tráfico a través de la red pública de Internet mediante Azure Portal.

> [!IMPORTANT]
> La preferencia de enrutamiento se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="register-the-feature-for-your-subscription"></a>Registro de la característica de la suscripción
La característica de preferencias de enrutamiento se encuentra actualmente en versión preliminar. Debe registrar la característica de la suscripción mediante Azure PowerShell, tal como se indica a continuación:
```azurepowershell
Register-AzProviderFeature -FeatureName AllowRoutingPreferenceFeature ProviderNamespace Microsoft.Network
```

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](https://preview.portal.azure.com/).

## <a name="create-a-virtual-machine"></a>Creación de una máquina virtual

1. Seleccione **+ Crear un recurso** en la esquina superior izquierda de Azure Portal.
2. Seleccione **Proceso** y, luego, **Máquina virtual de Windows Server 2016** u otro sistema operativo de su elección.
3. Escriba o seleccione la siguiente información, acepte los valores predeterminados para el resto de la configuración y luego seleccione **Aceptar**:

    |Configuración|Value|
    |---|---|
    |Nombre|myVM|
    |Nombre de usuario| Escriba un nombre de usuario de su elección.|
    |Contraseña| Escriba una contraseña de su elección. La contraseña debe tener al menos 12 caracteres de largo y cumplir con los [requisitos de complejidad definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Subscription| Seleccione su suscripción.|
    |Resource group| Seleccione **Usar existente** y, a continuación, **myResourceGroup**.|
    |Location| Seleccione **Este de EE. UU**.|

4. Seleccione un tamaño para la máquina virtual y luego **Seleccionar**.
5. En la pestaña de **redes**, haga clic en **Crear nueva** para crear una **dirección IP pública**.
6. Escriba *myPublicIpAddress*, seleccione una SKU como **estándar** y, a continuación, seleccione **Internet** como preferencia de enrutamiento; una vez hecho esto, presione **Aceptar**, tal como se muestra en la siguiente imagen:

   ![Selección de estática](./media/tutorial-routing-preference-virtual-machine-portal/routing-preference-internet-new.png)

6. Seleccione un puerto o ningún puerto en **Seleccionar puertos de entrada públicos**. Se selecciona el puerto 3389 para permitir el acceso remoto a la máquina virtual Windows Server desde Internet. No se recomienda abrir el puerto 3389 desde Internet con cargas de trabajo de producción.

   ![Selección de un puerto](./media/tutorial-routing-preference-virtual-machine-portal/pip-ports-new.png)

7. Acepte los valores predeterminados restantes y seleccione **Aceptar**.
8. En la página **Resumen**, seleccione **Crear**. La máquina virtual tarda minutos en crearse.
9. Una vez implementada la máquina virtual, escriba *myPublicIpAddress* en el cuadro de búsqueda de la parte superior del portal. Cuando **myPublicIpAddress** aparezca en los resultados de búsqueda, selecciónela.
10. Puede ver la dirección IP pública asignada, y que la dirección se asigna a la máquina virtual **myVM**, como se muestra en la siguiente imagen:

    ![Ver dirección IP pública](./media/tutorial-routing-preference-virtual-machine-portal/pip-properties-new.png)

11. Seleccione **Redes** y haga clic en el valor NIC **mynic**; a continuación, seleccione la dirección IP pública para confirmar que la preferencia de enrutamiento está asignada como **Internet**.
    ![Ver dirección IP pública](./media/tutorial-routing-preference-virtual-machine-portal/pip-routing-internet-new.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no sea necesario, elimine el grupo de recursos y todos los recursos que contiene:

1. Escriba *myResourceGroup* en el cuadro **Buscar** que se encuentra en la parte superior del portal. Seleccione **myResourceGroup** cuando lo vea en los resultados de la búsqueda.
2. Seleccione **Eliminar grupo de recursos**.
3. Escriba *myResourceGroup* para **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS:** y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre las [direcciones IP públicas con preferencia de enrutamiento](routing-preference-overview.md).
- Obtenga más información acerca de las [direcciones IP públicas](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) en Azure.
- Obtenga más información acerca de toda la [configuración de direcciones IP públicas](virtual-network-public-ip-address.md#create-a-public-ip-address).
