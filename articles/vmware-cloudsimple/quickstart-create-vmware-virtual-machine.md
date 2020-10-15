---
title: 'Inicio rápido: Uso de máquinas virtuales de VMware en Azure'
titleSuffix: Azure VMware Solution by CloudSimple
description: Aprenda a configurar y a usar máquinas virtuales de VMware desde Azure Portal mediante VMware Solution by CloudSimple
author: sharaths-cs
ms.author: dikamath
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 4ab613c251bc43a025e0381046805ec998a04227
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "77019560"
---
# <a name="quickstart---consume-vmware-vms-on-azure"></a>Inicio rápido: Uso de máquinas virtuales de VMware en Azure

Para crear una máquina virtual en Azure Portal, use plantillas de máquina virtual que el administrador de CloudSimple haya habilitado para la suscripción. Las plantillas de máquina virtual se encuentran en la infraestructura de VMware.

## <a name="cloudsimple-vm-creation-on-azure-requires-a-vm-template"></a>La creación de máquinas virtuales de CloudSimple en Azure requiere una plantilla de máquina virtual

Cree una máquina virtual en la nube privada desde la interfaz de usuario de vCenter. Para crear una plantilla, siga las instrucciones de [Clonar una máquina virtual en una plantilla en vSphere Web Client](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-FE6DE4DF-FAD0-4BB0-A1FD-AFE9A40F4BFE.html). Almacene la plantilla de máquina virtual en la instancia de vCenter de la nube privada.

## <a name="create-a-virtual-machine-in-the-azure-portal"></a>Creación de una máquina virtual en Azure Portal

1. Seleccione **Todos los servicios**.

2. Busque **CloudSimple Virtual Machines**.

3. Haga clic en **Agregar**.

    ![Creación de una máquina virtual de CloudSimple](media/create-cloudsimple-virtual-machine.png)

4. Escriba la información básica y haga clic en **Siguiente: Tamaño**.

    ![Creación de una máquina virtual de CloudSimple: Conceptos básicos](media/create-cloudsimple-virtual-machine-basic-info.png)

    | Campo | Descripción |
    | ------------ | ------------- |
    | Subscription | Suscripción de Azure asociada a la nube privada.  |
    | Grupo de recursos | Grupo de recursos al que se va a asignar la máquina virtual. Puede seleccionar un grupo existente o crear uno. |
    | Nombre | Nombre para identificar la máquina virtual.  |
    | Location | Región de Azure en la que se hospeda la máquina virtual.  |
    | Nube privada | Nube privada de CloudSimple en la que quiere crear la máquina virtual. |
    | Grupo de recursos | Grupo de recursos asignado para la máquina virtual. Seleccione entre los grupos de recursos disponibles. |
    | Plantilla de vSphere | Plantilla de vSphere para la máquina virtual.  |
    | Nombre de usuario | Nombre de usuario del administrador de la máquina virtual (para plantillas de Windows).|
    | Contraseña |  Contraseña del administrador de la máquina virtual (para plantillas de Windows). |
    | Confirmar contraseña | Confirme la contraseña. |

5. Seleccione el número de núcleos y la capacidad de memoria de la máquina virtual y haga clic en **Siguiente: Configuraciones**. Active la casilla si quiere exponer la virtualización completa de la CPU al sistema operativo invitado. Las aplicaciones que requieren virtualización de hardware se pueden ejecutar en máquinas virtuales sin traducción binaria o paravirtualización. Para más información, consulte el artículo de VMware <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html" target="_blank">Exponer la virtualización asistida por hardware de VMware</a>.

    ![Creación de una máquina virtual de CloudSimple: Tamaño](media/create-cloudsimple-virtual-machine-size.png)

6. Configure las interfaces de red y los discos como se describe en las tablas siguientes y haga clic en **Revisar y crear**.

    ![Creación de una máquina virtual de CloudSimple: Configuraciones](media/create-cloudsimple-virtual-machine-configurations.png)

    En las interfaces de red, haga clic en **Add network interface** (Agregar interfaz de red) y configure los siguientes valores.

    | Control | Descripción |
    | ------------ | ------------- |
    | Nombre | Escriba un nombre para identificar la interfaz.  |
    | Red | Seleccione en la lista de grupos de puertos distribuidos configurados de la instancia de vSphere de la nube privada.  |
    | Adapter (Adaptador) | Seleccione un adaptador de vSphere en la lista de tipos disponibles configurados para la máquina virtual. Para más información, consulte el artículo de la base de conocimiento de VMware <a href="https://kb.vmware.com/s/article/1001805" target="_blank">Elegir un adaptador de red para la máquina virtual</a>. |
    | Power on at Boot (Encender al arrancar) | Elija si quiere habilitar el hardware de NIC al arrancar la máquina virtual. El valor predeterminado es **Habilitar**. |

    En los discos, haga clic en **Add disk** (Agregar disco) y configure los siguientes valores.

    | Elemento | Descripción |
    | ------------ | ------------- |
    | Nombre | Escriba un nombre para identificar el disco.  |
    | Size | Seleccione uno de los tamaños disponibles.  |
    | SCSI Controller (Controladora SCSI) | Seleccione una controladora SCSI para el disco.  |
    | Mode | Determina cómo participa el disco en las instantáneas. Elija una de estas opciones: <br> - Independent persistent (Independiente persistente): todos los datos escritos en el disco se escriben de forma permanente.<br> - Independent non-persistent (Independiente no persistente): los cambios escritos en el disco se descartan cuando se apaga o restablece la máquina virtual.  El modo independiente no persistente permite reiniciar siempre la máquina virtual en el mismo estado. Para más información, consulte la <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html" target="_blank">documentación de VMware</a>.

7. Cuando finalice la validación, revise la configuración y haga clic en **Crear**. Para realizar cambios, haga clic en las pestañas de la parte superior.

    ![Creación de una máquina virtual de CloudSimple: Revisión](media/create-cloudsimple-virtual-machine-review.png)

## <a name="next-steps"></a>Pasos siguientes

* [View list of CloudSimple virtual machines](azure-create-vm.md#view-list-of-cloudsimple-virtual-machines) (Visualización de la lista de máquinas virtuales de CloudSimple)
* [Administración de máquinas virtuales de CloudSimple desde Azure](azure-manage-vm.md)
