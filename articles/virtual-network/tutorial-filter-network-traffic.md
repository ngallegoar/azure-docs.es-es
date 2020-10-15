---
title: Tutorial sobre el filtrado del tráfico de red en Azure Portal
titlesuffix: Azure Virtual Network
description: En este tutorial, aprenderá a filtrar el tráfico de red que se dirige a una subred, con un grupo de seguridad de red, mediante Azure Portal.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
tags: azure-resource-manager
Customer intent: I want to filter network traffic to virtual machines that perform similar functions, such as web servers.
ms.service: virtual-network
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 12/13/2018
ms.author: kumud
ms.openlocfilehash: b5a136ae05b3cd410ca252b6d5a1df443aff6f7a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75350134"
---
# <a name="tutorial-filter-network-traffic-with-a-network-security-group-using-the-azure-portal"></a>Tutorial: Filtrado del tráfico de red con un grupo de seguridad de red mediante Azure Portal

Puede filtrar el tráfico de red entrante y saliente de una subred de una red virtual con un grupo de seguridad de red. Los grupos de seguridad de red contienen reglas de seguridad que filtran el tráfico de red por dirección IP, puerto y protocolo. Las reglas de seguridad se aplican a los recursos implementados en una subred. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear un grupo de seguridad de red y reglas de seguridad
> * Crear una red virtual y asociar un grupo de seguridad de red a una subred
> * Implementar máquinas virtuales (VM) en una subred
> * Probar los filtros de tráfico

Si lo prefiere, puede completar este tutorial con la [CLI de Azure](tutorial-filter-network-traffic-cli.md) o [Azure PowerShell](tutorial-filter-network-traffic-powershell.md).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="create-a-virtual-network"></a>Creación de una red virtual

1. En el menú de Azure Portal o en la **página principal**, seleccione **Crear un recurso**. 
2. Seleccione **Redes** y **Red virtual**.
3. Escriba o seleccione la siguiente información, acepte los valores predeterminados para el resto de la configuración y luego seleccione **Crear**:

    | Configuración                 | Value                                              |
    | ---                     | ---                                                |
    | Nombre                    | myVirtualNetwork                                   |
    | Espacio de direcciones           | 10.0.0.0/16                                        |
    | Subscription            | Seleccione su suscripción.                          |
    | Resource group          | Haga clic en **Crear nuevo** y escriba *myResourceGroup*. |
    | Location                | Seleccione **Este de EE. UU**.                                |
    | Nombre de subred            | mySubnet                                           |
    | Subred: intervalo de direcciones  | 10.0.0.0/24                                        |

## <a name="create-application-security-groups"></a>Creación de grupos de seguridad de aplicaciones

Un grupo de seguridad de aplicaciones permite agrupar servidores con funciones similares, como servidores web.

1. En el menú de Azure Portal o en la **página principal**, seleccione **Crear un recurso**. 
2. En el cuadro **Buscar en Marketplace**, escriba *Grupo de seguridad de aplicaciones*. Cuando **Grupo de seguridad de aplicaciones** aparezca en los resultados de la búsqueda, selecciónelo, vuelva a seleccionar **Grupo de seguridad de aplicaciones** en **Todo** y, después, haga clic en **Crear**.
3. Especifique o seleccione los siguientes datos y haga clic en **Crear**:

    | Configuración        | Value                                                         |
    | ---            | ---                                                           |
    | Nombre           | myAsgWebServers                                               |
    | Subscription   | Seleccione su suscripción.                                     |
    | Resource group | Seleccione **Usar existente** y después seleccione **myResourceGroup**. |
    | Location       | Este de EE. UU.                                                       |

4. Complete el paso 3 de nuevo, especificando los valores siguientes:

    | Configuración        | Value                                                         |
    | ---            | ---                                                           |
    | Nombre           | myAsgMgmtServers                                              |
    | Subscription   | Seleccione su suscripción.                                     |
    | Resource group | Seleccione **Usar existente** y después seleccione **myResourceGroup**. |
    | Location       | Este de EE. UU.                                                       |

## <a name="create-a-network-security-group"></a>Crear un grupo de seguridad de red

1. En el menú de Azure Portal o en la **página principal**, seleccione **Crear un recurso**. 
2. Seleccione **Redes** y **Grupo de seguridad de red**.
3. Especifique o seleccione los siguientes datos y haga clic en **Crear**:

    |Configuración|Value|
    |---|---|
    |Nombre|myNsg|
    |Subscription| Seleccione su suscripción.|
    |Resource group | Seleccione **Usar existente** y después seleccione *myResourceGroup*.|
    |Location|Este de EE. UU.|

## <a name="associate-network-security-group-to-subnet"></a>Asociación del grupo de seguridad de red a la subred

1. En el cuadro *Search resources, services, and docs* (Buscar en recursos, servicios y documentos) en la parte superior del portal, comience a escribir *myNsg*. Cuando **myNsg** aparezca en los resultados de búsqueda, selecciónelo.
2. En **CONFIGURACIÓN**, seleccione **Subredes** y, luego, **+ Asociar**, como se muestra en la imagen siguiente:

    ![Asociación del NSG a la subred](./media/tutorial-filter-network-traffic/associate-nsg-subnet.png)

3. En **Asociar subred**, seleccione **Red virtual** y **myVirtualNetwork**. Seleccione **Subred**, **MySubnet** y, a continuación, **Aceptar**.

## <a name="create-security-rules"></a>Creación de reglas de seguridad

1. Seleccione **CONFIGURACIÓN**, **Reglas de seguridad de entrada** y, después, **+ Agregar**, tal como se muestra en la imagen siguiente:

    ![Incorporación de una regla de seguridad de entrada](./media/tutorial-filter-network-traffic/add-inbound-rule.png)

2. Cree una regla de seguridad que permita a los puertos 80 y 443 para el grupo de seguridad de la aplicación **myAsgWebServers**. En **Agregar regla de seguridad de entrada**, escriba o seleccione los valores siguientes, acepte el resto de valores predeterminados y, después, seleccione **Agregar**:

    | Configuración                 | Value                                                                                                           |
    | ---------               | ---------                                                                                                       |
    | Destination             | Seleccione **Grupo de seguridad de la aplicación** y, a continuación, seleccione **myAsgWebServers** para **Grupo de seguridad de la aplicación**.  |
    | Intervalos de puertos de destino | Escriba 80,443                                                                                                    |
    | Protocolo                | Selección de TCP                                                                                                      |
    | Nombre                    | Allow-Web-All                                                                                                   |

3. Repita el paso 2 de nuevo, utilizando los siguientes valores:

    | Configuración                 | Value                                                                                                           |
    | ---------               | ---------                                                                                                       |
    | Destination             | Seleccione **Grupo de seguridad de la aplicación** y, a continuación, seleccione **myAsgMgmtServers** para **Grupo de seguridad de la aplicación**. |
    | Intervalos de puertos de destino | Escriba 3389                                                                                                      |
    | Protocolo                | Selección de TCP                                                                                                      |
    | Priority                | Escriba 110                                                                                                       |
    | Nombre                    | Allow-RDP-All (Permitir-RDP-Todo)                                                                                                   |

    En este tutorial, se expone RDP (puerto 3389) a Internet para la máquina virtual que se asigna al grupo de seguridad de la aplicación *myAsgMgmtServers*. En entornos de producción, en lugar de exponer el puerto 3389 a Internet, se recomienda conectarse a los recursos de Azure que desee administrar mediante una VPN o una conexión de red privada.

Una vez completados los pasos 1 a 3, revise las reglas creadas. La lista debe ser similar a la que aparece en la siguiente imagen:

![Reglas de seguridad](./media/tutorial-filter-network-traffic/security-rules.png)

## <a name="create-virtual-machines"></a>Creación de máquinas virtuales

Cree dos máquinas virtuales en la red virtual.

### <a name="create-the-first-vm"></a>Creación de la primera máquina virtual

1. En el menú de Azure Portal o en la **página principal**, seleccione **Crear un recurso**. 
2. Seleccione **Compute** y, después, seleccione **Windows Server 2016 Datacenter**.
3. Escriba o seleccione la siguiente información y acepte los valores predeterminados para el resto de la configuración:

    |Configuración|Value|
    |---|---|
    |Subscription| Seleccione su suscripción.|
    |Resource group| Seleccione **Usar existente** y, a continuación, **myResourceGroup**.|
    |Nombre|myVmWeb|
    |Location| Seleccione **Este de EE. UU**.|
    |Nombre de usuario| Escriba un nombre de usuario de su elección.|
    |Contraseña| Escriba una contraseña de su elección. La contraseña debe tener al menos 12 caracteres de largo y cumplir con los [requisitos de complejidad definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|

   

4. Seleccione un tamaño para la máquina virtual y luego **Seleccionar**.
5. En **Redes**, seleccione los valores siguientes y acepte los valores predeterminados restantes:

    |Configuración|Value|
    |---|---|
    |Virtual network |Seleccione **myVirtualNetwork**.|
    |Grupo de seguridad de red de NIC |Seleccione **Ninguno**.|
  

6. Seleccione **Revisar y crear** en la esquina inferior izquierda y seleccione **Crear** para iniciar la implementación de la máquina virtual.

### <a name="create-the-second-vm"></a>Creación de la segunda máquina virtual

Complete de nuevo los pasos 1 a 6, pero recuerde que, en el paso 3, debe nombrar la máquina virtual *myVmMgmt*. La maquina virtual tarda unos minutos en implementarse. No continúe con el paso siguiente hasta que se implemente la máquina virtual.

## <a name="associate-network-interfaces-to-an-asg"></a>Asociación de interfaces de red a un ASG

Cuando el portal creó las máquinas virtuales, se creó también una interfaz de red para cada una y la interfaz de red se asoció a la máquina virtual. Agregue la interfaz de red para cada máquina virtual a uno de los grupos de seguridad de la aplicación que creó anteriormente:

1. En el cuadro *Search resources, services, and docs* (Buscar en recursos, servicios y documentos) en la parte superior del portal, comience a escribir *myVmWeb*. Cuando la máquina virtual **myVmWeb** aparezca en los resultados de búsqueda, selecciónela.
2. En **CONFIGURACIÓN**, seleccione **Redes**.  Seleccione **Configure the application security groups** (Configurar los grupos de seguridad de la aplicación), seleccione **myAsgWebServers** para **Grupos de seguridad de la aplicación** y, a continuación, seleccione **Guardar**, tal y como se muestra en la siguiente imagen:

    ![Asociación a los ASG](./media/tutorial-filter-network-traffic/associate-to-asg.png)

3. Complete los pasos 1 y 2 de nuevo, buscando la máquina virtual **myVmMgmt** y seleccionando el ASG **myAsgMgmtServers**.

## <a name="test-traffic-filters"></a>Probar los filtros de tráfico

1. Conéctese a la máquina virtual *myVmMgmt*. Escriba *myVmMgmt* en el cuadro de búsqueda que se encuentra en la parte superior del portal. Seleccione **myVmMgmt** cuando aparezca en los resultados de la búsqueda. Seleccione el botón **Conectar**.
2. Seleccione **Descargar archivo RDP**.
3. Abra el archivo RDP descargado y seleccione **Conectar**. Escriba el nombre de usuario y la contraseña que especificó al crear la máquina virtual. Puede que deba seleccionar **More choices** (Más opciones) y, luego, **Use a different account** (Usar una cuenta diferente), para especificar las credenciales que escribió al crear la máquina virtual.
4. Seleccione **Aceptar**.
5. Puede recibir una advertencia de certificado durante el proceso de inicio de sesión. Si recibe la advertencia, seleccione **Sí** o **Continuar** para continuar con la conexión.

    La conexión se realiza correctamente porque el puerto 3389 tiene permitida la entrada desde Internet al grupo de seguridad de aplicaciones *myAsgMgmtServers* en el que se encuentra la interfaz de red conectada a la máquina virtual *myVmMgmt*.

6. Para conectarse a la máquina virtual *myVmWeb* desde la máquina virtual *myVmMgmt*, escriba el siguiente comando en una sesión de PowerShell:

    ``` 
    mstsc /v:myVmWeb
    ```

    Es posible conectarse a la máquina virtual myVmWeb desde la máquina virtual myVmMgmt porque las máquinas virtuales de la misma red virtual pueden comunicarse entre sí a través de cualquier puerto, de forma predeterminada. Sin embargo, no es posible crear una conexión de escritorio remoto con la máquina virtual *myVmWeb* desde Internet porque la regla de seguridad para *myAsgWebServers* no permite el puerto 3389 de entrada desde Internet y, de forma predeterminada, al tráfico de entrada desde Internet se le deniegan todos los recursos.

7. Para instalar Microsoft IIS en la máquina virtual *myVmWeb* use el siguiente comando desde una sesión de PowerShell en la máquina virtual *myVmWeb*:

    ```powershell
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    ```

8. Una vez completada la instalación de IIS, desconecte la sesión de la máquina virtual *myVmWeb*, lo que le deja en la conexión de escritorio remoto de la máquina virtual *myVmMgmt*.
9. Desconéctese de la máquina virtual *myVmMgmt*.
10. En el cuadro *Search resources, services, and docs* (Buscar recursos, servicios y documentos) en la parte superior de Azure Portal, comience a escribir *myVmWeb* desde su equipo. Cuando aparezca **myVmWeb** en los resultados de búsqueda, selecciónelo. Anote la **Dirección IP pública** de la máquina virtual. La dirección que aparece en la siguiente imagen es 137.135.84.74, pero su dirección es diferente:

    ![Dirección IP pública](./media/tutorial-filter-network-traffic/public-ip-address.png)
  
11. Para confirmar que tiene acceso al servidor web *myVmWeb* desde Internet, abra un explorador de Internet en el equipo y vaya a `http://<public-ip-address-from-previous-step>`. Ve la pantalla de bienvenida de IIS, porque el puerto 80 tiene permitida la entrada desde Internet al grupo de seguridad de aplicaciones *myAsgWebServers* en el que se encuentra la interfaz de red conectada a la máquina virtual *myVmWeb*.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no sea necesario, elimine el grupo de recursos y todos los recursos que contiene:

1. Escriba *myResourceGroup* en el cuadro **Buscar** que se encuentra en la parte superior del portal. Seleccione **myResourceGroup** cuando lo vea en los resultados de la búsqueda.
2. Seleccione **Eliminar grupo de recursos**.
3. Escriba *myResourceGroup* para **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS:** y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado un grupo de seguridad de red y lo ha asociado a una subred de una red virtual. Para más información acerca de los grupos de seguridad de red, consulte [Introducción a los grupos de seguridad de red](security-overview.md) y [Administración de un grupo de seguridad de red](manage-network-security-group.md).

De forma predeterminada, Azure enruta el tráfico entre subredes. En su lugar, puede elegir enrutar el tráfico entre subredes a través de una máquina virtual, que actúa como un firewall, por ejemplo. Para aprender a crear una tabla de rutas, avance al siguiente tutorial.

> [!div class="nextstepaction"]
> [Creación de una tabla de rutas](./tutorial-create-route-table-portal.md)
