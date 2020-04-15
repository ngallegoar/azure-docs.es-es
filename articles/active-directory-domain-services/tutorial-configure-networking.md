---
title: 'Tutorial: Configuración de redes virtuales para Azure Active Directory Domain Services | Microsoft Docs'
description: En este tutorial aprenderá a crear y configurar un emparejamiento de red o subred virtual de Azure para una instancia de Azure Active Directory Domain Services mediante Azure Portal.
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: tutorial
ms.date: 03/30/2020
ms.author: iainfou
ms.openlocfilehash: af284e4c10487123c8c2a2105a25a2285ae0aa99
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2020
ms.locfileid: "80474364"
---
# <a name="tutorial-configure-virtual-networking-for-an-azure-active-directory-domain-services-instance"></a>Tutorial: Configuración de redes virtuales para una instancia de Azure Active Directory Domain Services

Para proporcionar conectividad a usuarios y aplicaciones, se implementa un dominio administrado de Azure Active Directory Domain Services (Azure AD DS) en una subred de red virtual de Azure. Esta subred de red virtual solo debe usarse para los recursos del dominio administrado que proporciona la plataforma Azure. Cuando cree sus propias máquinas virtuales y aplicaciones, estas no se deben implementar en la misma subred de red virtual. En su lugar, debe crear e implementar las aplicaciones en una subred de red virtual independiente, o en una red virtual independiente que esté emparejada con la red virtual de Azure AD DS.

En este tutorial se muestra cómo crear y configurar una subred de red virtual dedicada o cómo emparejar una red diferente a la red virtual del dominio administrado de Azure AD DS.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Comprender las opciones de conectividad de red virtual a Azure AD DS para recursos unidos a un dominio
> * Crear un intervalo de direcciones IP y una subred adicional en la red virtual de Azure AD DS
> * Configurar el emparejamiento de red virtual con una red independiente de Azure AD DS

Si no tiene una suscripción a Azure, [cree una cuenta](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial, necesitará los siguientes recursos y privilegios:

* Una suscripción de Azure activa.
    * Si no tiene una suscripción a Azure, [cree una cuenta](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Un inquilino de Azure Active Directory asociado a su suscripción, ya sea sincronizado con un directorio en el entorno local o con un directorio solo en la nube.
    * Si es necesario, [cree un inquilino de Azure Active Directory][create-azure-ad-tenant] o [asocie una suscripción a Azure con su cuenta][associate-azure-ad-tenant].
* Necesita privilegios de *administrador global* en el inquilino de Azure AD para habilitar Azure AD DS.
* Necesita privilegios de *colaborador* en la suscripción de Azure para crear los recursos de Azure AD DS necesarios.
* Un dominio administrado de Azure Active Directory Domain Services habilitado y configurado en su inquilino de Azure AD.
    * Si es necesario, el primer tutorial [crea y configura una instancia de Azure Active Directory Domain Services][create-azure-ad-ds-instance].

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

En este tutorial creará y configurará la instancia de Azure AD DS mediante Azure Portal. Para empezar a trabajar, primero inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="application-workload-connectivity-options"></a>Opciones de conectividad de carga de trabajo de aplicación

En el tutorial anterior, se creó un dominio administrado de Azure AD DS que usaba algunas opciones de configuración predeterminadas para la red virtual. Estas opciones predeterminadas crearon una red virtual de Azure y una subred de la red virtual. Los controladores de dominio de Azure AD DS que proporcionan los servicios de dominio administrado están conectados a esta subred de red virtual.

Al crear y ejecutar máquinas virtuales que necesitan usar el dominio administrado de Azure AD DS, es necesario proporcionar conectividad de red. Esta conectividad de red se puede proporcionar de una de las siguientes maneras:

* Cree una subred de red virtual adicional en la red virtual predeterminada del dominio administrado de Azure AD DS. Esta subred adicional es donde creará y conectará las máquinas virtuales.
    * Dado que las máquinas virtuales forman parte de la misma red virtual, pueden realizar automáticamente la resolución de nombres y comunicarse con los controladores de dominio de Azure AD DS.
* Configure el emparejamiento de red virtual de Azure desde la red virtual del dominio administrado de Azure AD DS con una o varias redes virtuales independientes. Estas redes virtuales independientes son donde creará y conectará las máquinas virtuales.
    * Al configurar el emparejamiento de red virtual, también debe especificar la configuración de DNS para volver a usar la resolución de nombres en los controladores de dominio de Azure AD DS.

Normalmente solo se usa una de estas opciones de conectividad de red. La elección suele depender de cómo desee administrar los recursos de Azure. Si desea administrar Azure AD DS y las máquinas virtuales conectadas como un grupo de recursos, puede crear una subred de red virtual adicional para las máquinas virtuales. Si desea separar la administración de Azure AD DS y realizar después la de las máquinas virtuales conectadas, puede usar el emparejamiento de red virtual. También puede usar el emparejamiento de red virtual para proporcionar conectividad a las máquinas virtuales existentes en el entorno de Azure que están conectadas a una red virtual existente.

En este tutorial, solo tiene que configurar una de estas opciones de conectividad de red virtual.

Para más información sobre cómo planear y configurar la red virtual, consulte [Consideraciones de red de Azure Active Directory Domain Services][consideraciones de red].

## <a name="create-a-virtual-network-subnet"></a>Creación de una subred de red virtual

De forma predeterminada, la red virtual de Azure creada con el dominio administrado de Azure AD DS contiene una sola subred de red virtual. Esta subred de red virtual solo debe usarla la plataforma Azure para proporcionar servicios de dominio administrado. Para crear y usar sus propias máquinas virtuales en esta red virtual de Azure, cree una subred adicional.

Para crear una subred de red virtual para las máquinas virtuales y las cargas de trabajo de la aplicación, complete los pasos siguientes:

1. En Azure Portal, seleccione el grupo de recursos de su dominio administrado de Azure AD DS, como *myResourceGroup*. En la lista de recursos, elija la red virtual predeterminada, como *aadds-vnet*.
1. En el menú de la izquierda de la ventana de la red virtual, seleccione **Espacio de direcciones**. La red virtual se crea con un solo espacio de direcciones de *10.0.2.0/24*, que es la que utiliza la subred predeterminada.

    Agregue un intervalo de direcciones IP adicional a la red virtual. El tamaño de este intervalo de direcciones y el intervalo de direcciones IP real que se usará dependerán de otros recursos de red ya implementados. El intervalo de direcciones IP no debe solaparse con los intervalos de direcciones existentes en el entorno de Azure o local. Asegúrese de que el intervalo de direcciones IP sea lo suficientemente grande para el número de máquinas virtuales que espera implementar en la subred.

    En el ejemplo siguiente, se agrega un intervalo de direcciones IP adicional de *10.0.3.0/24*. Cuando esté preparado, seleccione **Guardar**.

    ![Adición de un intervalo de direcciones IP de red virtual adicional en Azure Portal](./media/tutorial-configure-networking/add-vnet-address-range.png)

1. A continuación, en el menú de la izquierda de la ventana de la red virtual, seleccione **Subredes** y, a continuación, seleccione **+ Subred** para agregar una subred.
1. Escriba un nombre para la subred, como *workloads*. Si es necesario, actualice el **intervalo de direcciones** si desea usar un subconjunto del intervalo de direcciones IP configurado para la red virtual en los pasos anteriores. Por el momento, deje los valores predeterminados de opciones como Grupo de seguridad de red, Tabla de rutas y Puntos de conexión de servicio.

    En el ejemplo siguiente, se crea una subred denominada *workloads* que usa el intervalo de direcciones IP de *10.0.3.0/24*:

    ![Adición de una subred de red virtual adicional en Azure Portal](./media/tutorial-configure-networking/add-vnet-subnet.png)

1. Cuando esté preparado, seleccione **Aceptar**. La creación de la subred de red virtual tarda unos instantes.

Cuando cree una máquina virtual que necesite usar el dominio administrado de Azure AD DS, asegúrese de seleccionar esta subred de red virtual. No cree máquinas virtuales en la subred *aadds-subnet* predeterminada. Si selecciona una red virtual diferente, no habrá conectividad de red ni resolución DNS para comunicarse con el dominio administrado de Azure AD DS a menos que configure el emparejamiento de red virtual.

## <a name="configure-virtual-network-peering"></a>Configuración del emparejamiento de red virtual

Es posible que ya tenga una red virtual de Azure para las máquinas virtuales o que desee que su red virtual de dominio administrado de Azure AD DS siga siendo independiente. Para usar el dominio administrado, las máquinas virtuales de otras redes virtuales necesitan una manera de comunicarse con los controladores de dominio de Azure AD DS. Esta conectividad se puede proporcionar mediante el emparejamiento de red virtual de Azure.

Con el emparejamiento de red virtual de Azure, se conectan dos redes virtuales entre sí, sin necesidad de un dispositivo de red privada virtual (VPN). El emparejamiento de red permite conectar rápidamente redes virtuales y definir flujos de tráfico en el entorno de Azure. Para más información sobre el emparejamiento, consulte [Emparejamiento de redes virtuales de Azure][peering-overview].

Para emparejar una red virtual con la red virtual de dominio administrado de Azure AD DS, realice los pasos siguientes:

1. Elija la red virtual predeterminada creada para la instancia de Azure AD DS llamada *aadds-vnet*.
1. En el menú de la izquierda de la ventana de la red virtual, seleccione **Emparejamientos**.
1. Para crear un emparejamiento, seleccione **+ Agregar**. En el ejemplo siguiente, la red virtual *aadds-vnet* predeterminada está emparejada con una red virtual denominada *myVnet*. Configure las siguientes opciones con sus propios valores:

    * **Nombre del emparejamiento de aadds-vnet a red virtual remota**: un identificador descriptivo de las dos redes, como *aadds-vnet-to-myvnet*
    * **Modelo de implementación de red virtual**: *Resource Manager*
    * **Suscripción**: la suscripción de la red virtual con la que quiere realizar el emparejamiento, como *Azure*.
    * **Red virtual**: la red virtual con la que quiere realizar el emparejamiento, como *myVnet*.
    * **Nombre del emparejamiento de myVnet a aadds-vnet**: un identificador descriptivo de las dos redes, como *myvnet-to-aadds-vnet*

    ![Configuración del emparejamiento de red virtual en Azure Portal](./media/tutorial-configure-networking/create-peering.png)

    Deje los demás valores predeterminados para el acceso de red virtual o el tráfico reenviado a menos que su entorno presente requisitos específicos y, a continuación, seleccione **Aceptar**.

1. El emparejamiento tarda unos instantes en crearse tanto en la red virtual de Azure AD DS como en la red virtual que ha seleccionado. Cuando esté listo, **Estado de emparejamiento** indicará *Conectado*, como se muestra en la siguiente imagen:

    ![Redes emparejadas conectadas correctamente en Azure Portal](./media/tutorial-configure-networking/connected-peering.png)

Antes de que las máquinas virtuales de la red virtual emparejada puedan usar el dominio administrado de Azure AD DS, configure los servidores DNS para permitir una resolución de nombres correcta.

### <a name="configure-dns-servers-in-the-peered-virtual-network"></a>Configuración de servidores DNS en la red virtual emparejada

Para que las máquinas virtuales y las aplicaciones de la red virtual emparejada se comuniquen correctamente con el dominio administrado de Azure AD DS, se debe actualizar la configuración de DNS. Las direcciones IP de los controladores de dominio de Azure AD DS deben configurarse como servidores DNS en la red virtual emparejada. Existen dos maneras de configurar los controladores de dominio como servidores DNS para la red virtual emparejada:

* Configure los servidores DNS de la red virtual de Azure para usar los controladores de dominio de Azure AD DS.
* Configure el servidor DNS existente en uso en la red virtual emparejada para usar el reenvío condicional de DNS para dirigir las consultas al dominio administrado de Azure AD DS. Estos pasos varían en función del servidor DNS existente en uso.

En este tutorial, se configurarán los servidores DNS de la red virtual de Azure para dirigir todas las consultas a los controladores de dominio de Azure AD DS.

1. En Azure Portal, seleccione el grupo de recursos de la red virtual emparejada, como *myResourceGroup*. En la lista de recursos, elija la red virtual emparejada, como *myVnet*.
1. En el menú de la izquierda de la ventana de la red virtual, seleccione **Servidores DNS**.
1. De forma predeterminada, una red virtual usa los servidores DNS integrados que proporciona Azure. Seleccione **Personalizado** para usar servidores DNS personalizados. Escriba las direcciones IP de los controladores de dominio de Azure AD DS, que suelen ser *10.0.2.4* y *10.0.2.5*. Confirme estas direcciones IP en la ventana **Información general** del dominio administrado de Azure AD DS en el portal.

    ![Configuración de los servidores DNS de la red virtual para usar los controladores de dominio de Azure AD DS](./media/tutorial-configure-networking/custom-dns.png)

1. Cuando esté preparado, seleccione **Guardar**. La actualización de los servidores DNS de la red virtual tarda unos instantes.
1. Para aplicar la configuración de DNS actualizada a las máquinas virtuales, reinicie las máquinas virtuales conectadas a la red virtual emparejada.

Cuando cree una máquina virtual que necesite usar el dominio administrado de Azure AD DS, asegúrese de seleccionar esta red virtual emparejada. Si selecciona una red virtual diferente, no habrá conectividad de red ni resolución DNS para comunicarse con el dominio administrado de Azure AD DS.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Comprender las opciones de conectividad de red virtual a Azure AD DS para recursos unidos a un dominio
> * Crear un intervalo de direcciones IP y una subred adicional en la red virtual de Azure AD DS
> * Configurar el emparejamiento de red virtual con una red independiente de Azure AD DS

Para ver este dominio administrado en acción, cree una máquina virtual y únala al dominio.

> [!div class="nextstepaction"]
> [Unión de una máquina virtual de Windows Server a un dominio administrado](join-windows-vm.md)

<!-- INTERNAL LINKS --> 
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[create-join-windows-vm]: join-windows-vm.md
[peering-overview]: ../virtual-network/virtual-network-peering-overview.md
