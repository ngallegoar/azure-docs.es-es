---
title: Configuración de vRealize Operations para Azure VMware Solution
description: Aprenda a configurar vRealize Operations para una nube privada de Azure VMware Solution.
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 9e512d107ddc4d9bca28323658d09f4b4b378dc3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91579458"
---
# <a name="set-up-vrealize-operations-for-azure-vmware-solution"></a>Configuración de vRealize Operations para Azure VMware Solution


vRealize Operations Manager es una plataforma de administración de operaciones que permite a los administradores de infraestructura de VMware supervisar recursos del sistema. Estos recursos del sistema podrían ser objetos de nivel de aplicación o de nivel de infraestructura (ambos físicos y virtuales). Históricamente, la mayoría de los administradores han usado vRealize Operations para supervisar y administrar los componentes de nube privada de VMware: vCenter, ESXi, NSX-T, vSAN e Hybrid Cloud Extension (HCX). Cada nube privada de Azure VMware Solution se aprovisiona con una implementación dedicada de vCenter, NSX-T, vSAN y HCX. 

Revise primero detenidamente las secciones [Antes de comenzar](#before-you-begin) y [Requisitos previos](#prerequisites). A continuación, lo guiaremos a través de las dos topologías de implementación típicas para vRealize Operations Manager con Azure VMware Solution:

> [!div class="checklist"]
> * [vRealize Operations locales que administran la implementación de Azure VMware Solution](#on-premises-vrealize-operations-managing-azure-vmware-solution-deployment)
> * [vRealize Operations que se ejecutan en la implementación de Azure VMware Solution](#vrealize-operations-running-on-azure-vmware-solution-deployment)

## <a name="before-you-begin"></a>Antes de empezar
* Revise la [documentación del producto vRealize Operations Manager](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-7FFC61A0-7562-465C-A0DC-46D092533984.html) para obtener más información sobre la implementación de vRealize Operations. 
* Revise la [serie de tutoriales](tutorial-network-checklist.md) básicos de Software Defined Datacenter (SDDC) de Azure VMware Solution.
* Opcionalmente, revise la documentación del producto [vRealize Operations Remote Controller](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-263F9219-E801-4383-8A59-E84F3D01ED6B.html) para la opción de administración de vRealize Operations local de la implementación de Azure VMware Solution. 



## <a name="prerequisites"></a>Requisitos previos
* Se deben configurar VPN o Azure ExpressRoute entre el SDDC de Azure VMware Solution y el entorno local.
* Una nube privada de Azure VMware Solution se ha implementado en Azure.



## <a name="on-premises-vrealize-operations-managing-azure-vmware-solution-deployment"></a>vRealize Operations locales que administran la implementación de Azure VMware Solution
La mayoría de los clientes tienen una implementación local ya existente de vRealize Operations que se usa para administrar uno o varios dominios vCenters locales. Cuando los clientes aprovisionan una nueva nube privada de Azure VMware Solution en Azure, habitualmente conectan su entorno local con Azure VMware Solution mediante una instancia de Azure ExpressRoute o una solución VPN de nivel 3, como se muestra a continuación.   

:::image type="content" source="media/vrealize-operations-manager/vrealize-operations-deployment-option-1.png" alt-text="vRealize Operations locales que administran la implementación de Azure VMware Solution"  border="false":::

Para ampliar las funcionalidades de vRealize Operations a la nube privada de Azure VMware Solution, cree una [ instancia del adaptador para los recursos de una nube privada de Azure VMware Solution](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.config.doc/GUID-640AD750-301E-4D36-8293-1BFEB67E2600.html) para recopilar datos de la nube privada de Azure VMware Solution e incluirlos en vRealize Operations local. La instancia de vRealize Operations Manager local puede conectarse directamente a vCenter y NSX-T Manager en Azure VMware Solution, o bien, opcionalmente, puede implementar una instancia de vRealize Operations Remote Collector en la nube privada de Azure VMware Solution. Una instancia de vRealize Operations Remote Collector comprime y cifra los datos recopilados de la nube privada de Azure VMware Solution antes de su envío a través de la red VPN o de ExpressRoute a la instancia de vRealize Operations Manager que se ejecuta en el entorno local. 

> [!TIP]
> Consulte la [documentación de VMware](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-7FFC61A0-7562-465C-A0DC-46D092533984.html) y obtenga una guía paso a paso de cómo instalar vRealize Operations Manager. 



## <a name="vrealize-operations-running-on-azure-vmware-solution-deployment"></a>vRealize Operations que se ejecutan en la implementación de Azure VMware Solution

Otra opción de implementación es implementar una instancia de vRealize Operations Manager en uno de los clústeres de vSphere de la nube privada de Azure VMware Solution, como se muestra a continuación. 

:::image type="content" source="media/vrealize-operations-manager/vrealize-operations-deployment-option-2.png" alt-text="vRealize Operations locales que administran la implementación de Azure VMware Solution" border="false":::

Después de implementar la instancia de Azure VMware Solution de vRealize Operations, puede configurar vRealize Operations para recopilar datos de vCenter, ESXi, NSX-T, vSAN y HCX. 

> [!TIP]
> Consulte la [documentación de VMware](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-7FFC61A0-7562-465C-A0DC-46D092533984.html) y obtenga una guía paso a paso de cómo instalar vRealize Operations Manager.


## <a name="known-limitations"></a>Limitaciones conocidas

- El usuario **cloudadmin@vsphere.local** en Azure VMware Solution tiene [privilegios limitados](concepts-role-based-access-control.md). La recopilación de la memoria de invitado con las herramientas de VMware no es compatible con máquinas virtuales de Azure VMware Solution. Los usos de memoria activos y consumidos continúan funcionando en este caso.
- La optimización de la carga de trabajo para la intención empresarial basada en host no funciona porque Azure VMware Solution administra configuraciones de clúster, incluida la configuración de DRS.
- La optimización de la carga de trabajo para la colocación entre clústeres cruzados dentro de SDDC mediante la intención empresarial basada en clúster es totalmente compatible con vRealize Operations Manager 8.0 y versiones posteriores. Sin embargo, la optimización de la carga de trabajo no tiene en cuenta los grupos de recursos y coloca las máquinas virtuales en el nivel de clúster. Un usuario puede corregirlo manualmente en la interfaz de Azure VMware Solution vCenter Server.
- No se puede iniciar sesión en vRealize Operations Manager con las credenciales de Azure VMware Solution vCenter Server. 
- Azure VMware Solution no admite el complemento vRealize Operations Manager.

Al conectar Azure VMware Solution vCenter a vRealize Operations Manager mediante una cuenta de nube vCenter Server, encontrará la siguiente advertencia:

:::image type="content" source="./media/vrealize-operations-manager/warning-adapter-instance-creation-succeeded.png" alt-text="vRealize Operations locales que administran la implementación de Azure VMware Solution":::

La advertencia se produce porque el usuario **cloudadmin@vsphere.local** de Azure VMware Solution no tiene privilegios suficientes para realizar todas las acciones de vCenter Server necesarias para el registro. Sin embargo, los privilegios son suficientes para que la instancia de adaptador realice la recopilación de datos, como se muestra a continuación:

:::image type="content" source="./media/vrealize-operations-manager/adapter-instance-to-perform-data-collection.png" alt-text="vRealize Operations locales que administran la implementación de Azure VMware Solution":::

Para obtener más información, consulte [Privilegios necesarios para configurar una instancia del adaptador de vCenter](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.core.doc/GUID-3BFFC92A-9902-4CF2-945E-EA453733B426.html).

<!-- LINKS - external -->


<!-- LINKS - internal -->




