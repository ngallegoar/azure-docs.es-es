---
title: 'Guía de inicio rápido: Administración de máquinas virtuales Linux en Azure con Ansible'
description: En este inicio rápido, aprenderá a administrar una máquina virtual Linux en Azure mediante Ansible.
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
ms.topic: quickstart
ms.service: ansible
author: tomarchermsft
manager: gwallace
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: d94858391951aaf9387394afeb5ad2ae373fa7b5
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80239529"
---
# <a name="quickstart-manage-linux-virtual-machines-in-azure-using-ansible"></a>Inicio rápido: Administración de máquinas virtuales Linux en Azure con Ansible

Ansible permite automatizar la implementación y la configuración de recursos en un entorno. En este artículo, va a utilizar un cuaderno de estrategias de Ansible para iniciar y detener una máquina virtual Linux. 

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [open-source-devops-prereqs-azure-sub.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="stop-a-virtual-machine"></a>Detención de una máquina virtual

En esta sección, va a usar Ansible para desasignar (detener) una máquina virtual de Azure.

1. Inicie sesión en [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Abra [Cloud Shell](/azure/cloud-shell/overview).

1. Cree un archivo llamado `azure-vm-stop.yml` y ábralo en el editor:

    ```bash
    code azure-vm-stop.yml
    ```

1. Pegue el siguiente código de ejemplo en el editor:

    ```yaml
    - name: Stop Azure VM
      hosts: localhost
      connection: local
      tasks:
        - name: Stop virtual machine
          azure_rm_virtualmachine:
            resource_group: {{ resource_group_name }}
            name: {{ vm_name }}
            allocated: no
    ```

1. Reemplace los marcadores de posición `{{ resource_group_name }}` y `{{ vm_name }}` por sus valores.

1. Guarde el archivo y salga del editor.

1. Use el comando `ansible-playbook` para ejecutar el cuaderno de estrategias:

    ```bash
    ansible-playbook azure-vm-stop.yml
    ```

1. Tras ejecutar el cuaderno de estrategias, debería ver resultados similares a los siguientes:

    ```bash
    PLAY [Stop Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Deallocate the Virtual Machine] ***************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="start-a-virtual-machine"></a>Inicio de una máquina virtual

En esta sección, usará Ansible para iniciar una máquina virtual de Azure desasignada (detenida).

1. Inicie sesión en [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Abra [Cloud Shell](/azure/cloud-shell/overview).

1. Cree un archivo llamado `azure-vm-start.yml` y ábralo en el editor:

    ```bash
    code azure-vm-start.yml
    ```

1. Pegue el siguiente código de ejemplo en el editor:

    ```yaml
    - name: Start Azure VM
      hosts: localhost
      connection: local
      tasks:
        - name: Start virtual machine
          azure_rm_virtualmachine:
            resource_group: {{ resource_group_name }}
            name: {{ vm_name }}
    ```

1. Reemplace los marcadores de posición `{{ resource_group_name }}` y `{{ vm_name }}` por sus valores.

1. Guarde el archivo y salga del editor.

1. Use el comando `ansible-playbook` para ejecutar el cuaderno de estrategias:

    ```bash
    ansible-playbook azure-vm-start.yml
    ```

1. Tras ejecutar el cuaderno de estrategias, debería ver resultados similares a los siguientes:

    ```bash
    PLAY [Start Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Start the Virtual Machine] ********************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"] 
> [Tutorial: Administración de inventarios dinámicos de Azure con Ansible](./ansible-manage-azure-dynamic-inventories.md)