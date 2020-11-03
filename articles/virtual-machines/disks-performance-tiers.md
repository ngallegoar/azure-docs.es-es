---
title: Cambio del rendimiento de Azure Managed Disks
description: Obtenga información sobre los niveles de rendimiento de los discos administrados y cómo cambiar dichos niveles para los discos existentes.
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 09/24/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 404f435e321e53694807a627121d84f6cbf6724d
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2020
ms.locfileid: "92359686"
---
# <a name="performance-tiers-for-managed-disks-preview"></a>Niveles de rendimiento de discos administrados (versión preliminar)

Actualmente, Azure Disk Storage ofrece funcionalidades de expansión integradas para proporcionar un mayor rendimiento en el control del tráfico inesperado a corto plazo. Los SSD Premium tienen la flexibilidad para aumentar el rendimiento del disco sin aumentar el tamaño real del disco. Esta funcionalidad permite responder a las necesidades de rendimiento de la carga de trabajo y reducir los costos. 

> [!NOTE]
> Esta funcionalidad actualmente está en su versión preliminar. 

Esta característica resulta muy conveniente para los eventos que requieren temporalmente un nivel de rendimiento más alto de forma constante, como las compras navideñas, las pruebas de rendimiento o la ejecución de un entorno de aprendizaje. Para administrar estos eventos, puede usar un nivel de rendimiento superior mientras sea necesario. Después, puede volver al nivel original cuando ya no sea necesario el rendimiento adicional.

## <a name="how-it-works"></a>Cómo funciona

La primera vez que implementa o aprovisiona un disco, el nivel de rendimiento de línea de base para ese disco se establece en función del tamaño del disco aprovisionado. Puede usar un nivel de rendimiento superior para responder a la mayor demanda. Cuando ya no necesite ese nivel de rendimiento, podrá volver al nivel de rendimiento de línea de base inicial.

La facturación cambia a medida que cambia el nivel. Por ejemplo, si aprovisiona un disco P10 (128 GiB), el nivel de rendimiento de línea de base se establece como P10 (500 IOPS y 100 Mbps). Se le facturará según la tarifa de P10. Puede actualizar el nivel para cumplir con el rendimiento de P50 (7500 IOPS y 250 Mbps) sin aumentar el tamaño del disco. En el momento de la actualización, se le facturará según la tarifa P50. Cuando ya no necesite el alto rendimiento, puede volver al nivel P10. El disco se facturará una vez más según la tarifa P10.

| Tamaño del disco | Rendimiento de línea de base | Se puede actualizar a |
|----------------|-----|-------------------------------------|
| 4 GiB | P1 | P2, P3, P4, P6, P10, P15, P20, P30, P40, P50 |
| 8 GiB | P2 | P3, P4, P6, P10, P15, P20, P30, P40, P50 |
| 16 GiB | P3 | P4, P6, P10, P15, P20, P30, P40, P50 | 
| 32 GiB | P4 | P6, P10, P15, P20, P30, P40, P50 |
| 64 GiB | P6 | P10, P15, P20, P30, P40, P50 |
| 128 GB | P10 | P15, P20, P30, P40, P50 |
| 256 GiB | P15 | P20, P30, P40, P50 |
| 512 GB | P20 | P30, P40, P50 |
| 1 TiB | P30 | P40, P50 |
| 2 TiB | P40 | P50 |
| 4 TiB | P50 | None |
| 8 TiB | P60 |  P70, P80 |
| 16 TiB | P70 | P80 |
| 32 TiB | P80 | None |

Para obtener información de facturación, consulte los [Precios de Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/).

## <a name="restrictions"></a>Restricciones

- Esta característica es compatible actualmente solo con SSD Premium.
- Debe desasignar la máquina virtual o desconectar el disco de una máquina virtual en ejecución antes de que pueda cambiar el nivel del disco.
- El uso de los niveles de rendimiento P60, P70 y P80 está restringido a los discos de 4096 GiB o más.
- El nivel de rendimiento de un disco solo se puede cambiar a un nivel inferior una vez cada 24 horas.

## <a name="regional-availability"></a>Disponibilidad regional

La capacidad de ajustar el nivel de rendimiento de un disco administrado solo está disponible en las SSD Premium en las regiones del este de EE. UU. 2, centro-sur de EE. UU., centro-oeste de EE. UU. y sudeste de Australia.

## <a name="create-an-empty-data-disk-with-a-tier-higher-than-the-baseline-tier"></a>Creación de un disco de datos vacío con un nivel superior al nivel de línea de base

```azurecli
subscriptionId=<yourSubscriptionIDHere>
resourceGroupName=<yourResourceGroupNameHere>
diskName=<yourDiskNameHere>
diskSize=<yourDiskSizeHere>
performanceTier=<yourDesiredPerformanceTier>
region=westcentralus

az login

az account set --subscription $subscriptionId

az disk create -n $diskName -g $resourceGroupName -l $region --sku Premium_LRS --size-gb $diskSize --tier $performanceTier
```
## <a name="create-an-os-disk-with-a-tier-higher-than-the-baseline-tier-from-an-azure-marketplace-image"></a>Creación de un disco del sistema operativo con un nivel superior al nivel de línea de base a partir de una imagen de Azure Marketplace

```azurecli
resourceGroupName=<yourResourceGroupNameHere>
diskName=<yourDiskNameHere>
performanceTier=<yourDesiredPerformanceTier>
region=westcentralus
image=Canonical:UbuntuServer:18.04-LTS:18.04.202002180

az disk create -n $diskName -g $resourceGroupName -l $region --image-reference $image --sku Premium_LRS --tier $performanceTier
```
     
## <a name="update-the-tier-of-a-disk"></a>Actualización del nivel de un disco

```azurecli
resourceGroupName=<yourResourceGroupNameHere>
diskName=<yourDiskNameHere>
performanceTier=<yourDesiredPerformanceTier>

az disk update -n $diskName -g $resourceGroupName --set tier=$performanceTier
```
## <a name="show-the-tier-of-a-disk"></a>Consulta del nivel de un disco

```azurecli
az disk show -n $diskName -g $resourceGroupName --query [tier] -o tsv
```

## <a name="next-steps"></a>Pasos siguientes

Si necesita cambiar el tamaño de un disco para aprovechar las ventajas de los niveles de rendimiento superiores, consulte estos artículos:

- [Expansión de discos duros virtuales en una VM Linux con la CLI de Azure](linux/expand-disks.md)
- [Expansión de un disco administrado asociado a una máquina virtual de Windows](windows/expand-os-disk.md)
