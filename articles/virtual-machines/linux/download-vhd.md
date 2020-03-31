---
title: Descarga de un VHD de Linux desde Azure
description: Descarga de un VHD de Linux mediante la CLI de Azure y Azure Portal.
author: cynthn
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 08/21/2019
ms.author: cynthn
ms.openlocfilehash: 02c3ee483e6a31960fd5123070a49f568ac4c690
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78968790"
---
# <a name="download-a-linux-vhd-from-azure"></a>Descarga de un VHD de Linux desde Azure

En este artículo aprenderá a descargar un archivo de disco duro virtual (VHD) de Linux desde Azure mediante la CLI de Azure y Azure Portal. 

Si aún no lo ha hecho, instale la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="stop-the-vm"></a>Parada de la máquina virtual

No se puede descargar un VHD desde Azure si está conectado a una máquina virtual en ejecución. Debe detener la máquina virtual para descargar un VHD. Si quiere usar un VHD como [imagen](tutorial-custom-images.md) para crear otras máquinas virtuales con discos nuevos, debe desaprovisionar y generalizar el sistema operativo incluido en el archivo y detener la máquina virtual. Para usar el VHD como disco para una nueva instancia de una máquina virtual o un disco de datos existente, basta con detener y cancelar la asignación de la máquina virtual.

Para usar el VHD como imagen para crear otras máquinas virtuales, siga estos pasos:

1. Use SSH, el nombre de la cuenta y la dirección IP pública de la máquina virtual para conectarse a ella y desaprovisionarla. Puede encontrar la dirección IP pública con [az network public-ip show](https://docs.microsoft.com/cli/azure/network/public-ip#az-network-public-ip-show). El parámetro +user también quita la última cuenta de usuario aprovisionada. Si está creando credenciales de cuenta en la máquina virtual, ignore este parámetro +user. En el ejemplo siguiente se quita la última cuenta de usuario aprovisionada:

    ```bash
    ssh azureuser@<publicIpAddress>
    sudo waagent -deprovision+user -force
    exit 
    ```

2. Inicie sesión en su cuenta de Azure con [az login](https://docs.microsoft.com/cli/azure/reference-index).
3. Detenga y desasigne la máquina virtual.

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

4. Generalice la máquina virtual. 

    ```azurecli
    az vm generalize --resource-group myResourceGroup --name myVM
    ``` 

Para usar el VHD como disco para una nueva instancia de una máquina virtual o un disco de datos existente, realice estos pasos:

1.  Inicie sesión en [Azure Portal](https://portal.azure.com/).
2.  En el menú de la izquierda, seleccione **Máquinas virtuales**.
3.  Seleccione la máquina virtual en la lista.
4.  En la página de la máquina virtual, seleccione **Detener**.

    ![Detención de la máquina virtual](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a>Generación de dirección URL de SAS

Para descargar el archivo de VHD, debe generar una dirección URL de [firma de acceso compartido (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Una vez generada la dirección URL, se asigna un tiempo de expiración a la dirección URL.

1.  En el menú de la página de la máquina virtual, seleccione **Discos**.
2.  Seleccione el disco de sistema operativo de la máquina virtual y luego seleccione **Exportación del disco**.
3.  Seleccione **Generar dirección URL**.

    ![Generar dirección URL](./media/download-vhd/export-generate.png)

## <a name="download-vhd"></a>Descarga de VHD

1.  En la dirección URL generada, seleccione **Descargar el archivo VHD**.
**
    ![Descargar VHD](./media/download-vhd/export-download.png)

2.  Es posible que tenga que seleccionar **Guardar** en el explorador para iniciar la descarga. El nombre predeterminado del archivo de VHD es *abcd*.

    ![Seleccione Guardar en el explorador](./media/download-vhd/export-save.png)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [cargar y crear una máquina virtual de Linux desde un disco personalizado con la CLI de Azure](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
- [Administración de discos de Azure con la CLI de Azure](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

