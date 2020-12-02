---
title: archivo de inclusión
description: archivo de inclusión
services: iot-accelerators
author: dominicbetts
ms.service: iot-accelerators
ms.topic: include
ms.date: 08/16/2018
ms.author: dobett
ms.custom: include file, devx-track-azurecli
ms.openlocfilehash: e3eed0e7378f513e45e0eb9ac719a92e87ec1d53
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96027499"
---
## <a name="access-the-virtual-machine"></a>Acceso a la máquina virtual

En los siguientes pasos se usa la CLI de Azure en Azure Cloud Shell. Si lo prefiere, también puede [instalar la CLI de Azure](/cli/azure/install-azure-cli) en el equipo de desarrollo y ejecutar los comandos de forma local.

En los pasos siguientes se muestra cómo configurar la máquina virtual de Azure para permitir el acceso de **SSH**. En los pasos que se muestran se asume que el nombre que eligió para el acelerador de soluciones es **contoso-simulation**; reemplace este valor con el nombre de la implementación:

1. Lista de contenidos del grupo de recursos que contiene los recursos del acelerador de soluciones:

    ```azurecli-interactive
    az resource list -g contoso-simulation -o table
    ```

    Tome nota del nombre de la máquina virtual, la dirección IP pública y el grupo de seguridad de red; necesitará estos valores más adelante.

1. Actualice el grupo de seguridad de red para permitir el acceso de SSH. En el comando siguiente se da por supuesto que el nombre del grupo de seguridad de red es **contoso-simulation-nsg**; reemplace este valor con el nombre de su grupo de seguridad de red:

    ```azurecli-interactive
    az network nsg rule update --name SSH --nsg-name contoso-simulation-nsg -g contoso-simulation --access Allow -o table
    ```

    Solo se debe habilitar el acceso de SSH durante el desarrollo y las pruebas. Si habilita SSH, [debe deshabilitarlo nuevamente tan pronto como sea posible](../articles/security/fundamentals/network-best-practices.md#disable-rdpssh-access-to-virtual-machines).

1. Actualice la contraseña de la cuenta **azureuser** en la máquina virtual, con una contraseña que conozca. Elija su propia contraseña cuando ejecute el siguiente comando:

    ```azurecli-interactive
    az vm user update --name vm-vikxv --username azureuser --password YOURSECRETPASSWORD  -g contoso-simulation
    ```

1. Busque la dirección IP pública de la máquina virtual. En el comando siguiente se da por supuesto que el nombre de la máquina virtual es **vm vikxv**; reemplace este valor con el nombre de la máquina virtual que anotó anteriormente:

    ```azurecli-interactive
    az vm list-ip-addresses --name vm-vikxv -g contoso-simulation -o table
    ```

    Anote la dirección IP pública de la máquina virtual.