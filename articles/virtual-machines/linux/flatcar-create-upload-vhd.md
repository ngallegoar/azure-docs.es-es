---
title: Creación y carga de un VHD de Flatcar Container Linux para su uso en Azure
description: Aprenda a crear y cargar un VHD que contiene el sistema operativo Flatcar Container Linux.
author: marga-kinvolk
ms.author: danis
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: article
ms.date: 07/16/2020
ms.reviewer: cynthn
ms.openlocfilehash: 2bdbe93d5d593d429c4f7073227684c1e361864d
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86528203"
---
# <a name="using-a-prebuilt-flatcar-image-for-azure"></a>Uso de una imagen de Flatcar precompilada para Azure

Puede descargar imágenes de disco duro virtual de Azure precompiladas de Flatcar Container Linux para cada uno de los canales de Flatcar admitidos:

- [stable](https://stable.release.flatcar-linux.net/amd64-usr/current/flatcar_production_azure_image.vhd.bz2)
- [beta](https://beta.release.flatcar-linux.net/amd64-usr/current/flatcar_production_azure_image.vhd.bz2)
- [alpha](https://alpha.release.flatcar-linux.net/amd64-usr/current/flatcar_production_azure_image.vhd.bz2)
- [edge](https://edge.release.flatcar-linux.net/amd64-usr/current/flatcar_production_azure_image.vhd.bz2)

Esta imagen ya está completamente configurada y optimizada para ejecutarse en Azure. Solo tiene que descomprimirla.

## <a name="building-your-own-flatcar-based-virtual-machine-for-azure"></a>Creación de su propia máquina virtual basada en Flatcar para Azure

También puede optar por crear su propia imagen de Flatcar Container Linux.

En cualquier máquina basada en Linux, siga las instrucciones que se detallan en la [guía del SDK para desarrolladores de Flatcar Container Linux](https://docs.flatcar-linux.org/os/sdk-modifying-flatcar/). Al ejecutar el script `image_to_vm.sh`, asegúrese de pasar el parámetro `--format=azure` para crear un disco duro virtual de Azure.

## <a name="next-steps"></a>Pasos siguientes

Una vez que disponga del archivo .vhd, puede usar el archivo resultante para crear nuevas máquinas virtuales en Azure. Si es la primera vez que carga un archivo .vhd en Azure, vea [Creación de una VM Linux a partir de un disco personalizado](upload-vhd.md#option-1-upload-a-vhd).
