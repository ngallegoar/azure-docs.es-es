---
title: 'Aplicaciones de representación: Azure Batch'
description: Todas las aplicaciones de representación se pueden usar con Azure Batch. Sin embargo, las imágenes de máquina virtual de Azure Marketplace están disponibles con aplicaciones comunes preinstaladas.
services: batch
ms.service: batch
author: LauraBrenner
ms.author: labrenne
ms.date: 09/19/2019
ms.topic: conceptual
ms.openlocfilehash: 77672534b2aad993a44e9b637fbed58df8610e97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022994"
---
# <a name="pre-installed-applications-on-rendering-vm-images"></a>Aplicaciones preinstaladas en imágenes de máquina virtual de representación

Todas las aplicaciones de representación se pueden usar con Azure Batch. Sin embargo, las imágenes de máquina virtual de Azure Marketplace están disponibles con aplicaciones comunes preinstaladas.

Si procede, hay licencias de pago por uso disponibles para las aplicaciones de representación preinstaladas. Cuando se crea un grupo de Batch, se pueden especificar las aplicaciones necesarias y el costo tanto de la máquina virtual como de las aplicaciones se facturará por minuto. Los precios de las aplicaciones se muestran en la [página de precios de Azure Batch](https://azure.microsoft.com/pricing/details/batch/#graphic-rendering).

Algunas aplicaciones solo admiten Windows, pero la mayoría son compatibles con Windows y con Linux.

## <a name="applications-on-centos-7-rendering-images"></a>Aplicaciones en imágenes de representación de CentOS 7

La lista siguiente se aplica a las imágenes de representación de CentOS 7.6, versión 1.1.6.

* Autodesk Maya I/O 2017 Actualización 5 (versión 201708032230)
* Autodesk Maya I/O 2018 Actualización 2 (versión 201711281015)
* Autodesk Maya I/O 2019 Actualización 1
* Autodesk Arnold para Maya 2017 (Arnold versión 5.3.1.1) MtoA-3.2.1.1-2017
* Autodesk Arnold para Maya 2018 (Arnold versión 5.3.1.1) MtoA-3.2.1.1-2018
* Autodesk Arnold para Maya 2019 (Arnold versión 5.3.1.1) MtoA-3.2.1.1-2019
* Chaos Group V-Ray para Maya 2017 (versión 3.60.04)
* Chaos Group V-Ray para Maya 2018 (versión 3.60.04)
* Blender (2.68)
* Blender (2.8)

## <a name="applications-on-latest-windows-server-2016-rendering-images"></a>Aplicaciones en las imágenes de representación más recientes de Windows Server 2016

La lista siguiente se aplica a las imágenes de representación de Windows Server 2016, versión 1.3.8.

* Autodesk Maya I/O 2017 Actualización 5 (versión 17.4.5459)
* Autodesk Maya I/O 2018 Update 6 (versión 18.4.0.7622)
* Autodesk Maya I/O 2019
* Autodesk 3ds Max I/O 2018 Actualización 4 (versión 20.4.0.4254)
* Autodesk 3ds Max I/O 2019 Actualización 1 (versión 21.2.0.2219)
* Autodesk 3ds Max I/O 2020 Update 2
* Autodesk Arnold para Maya 2017 (versión de Arnold 5.3.0.2) MtoA-3.2.0.2-2017
* Autodesk Arnold para Maya 2018 (versión de Arnold 5.3.0.2) MtoA-3.2.0.2-2018
* Autodesk Arnold para Maya 2019 (Arnold versión 5.3.0.2) MtoA-3.2.0.2-2019
* Autodesk Arnold para 3ds Max 2018 (versión de Arnold 5.3.0.2) (versión 1.2.926)
* Autodesk Arnold para 3ds Max 2019 (versión de Arnold 5.3.0.2) (versión 1.2.926)
* Autodesk Arnold para 3ds Max 2020 (versión de Arnold 5.3.0.2) (versión 1.2.926)
* Chaos Group V-Ray para Maya 2017 (versión 4.12.01)
* Chaos Group V-Ray para Maya 2018 (versión 4.12.01)
* Chaos Group V-Ray para Maya 2019 (versión 4.04.03)
* Chaos Group V-Ray para 3ds Max 2018 (versión 4.20.01)
* Chaos Group V-Ray para 3ds Max 2019 (versión 4.20.01)
* Chaos Group V-Ray para 3ds Max 2020 (versión 4.20.01)
* Blender (2.79)
* Blender (2.80)
* AZ 10

> [!IMPORTANT]
> Para ejecutar V-Ray con Maya fuera de las [plantillas de extensión de Azure Batch](https://github.com/Azure/batch-extension-templates), inicie `vrayses.exe` antes de ejecutar la representación. Para iniciar el archivo vrayses.exe fuera de las plantillas, puede usar el siguiente comando `%MAYA_2017%\vray\bin\vrayses.exe"`.
>
> Para ver un ejemplo, consulte la tarea de inicio de la [plantilla de Maya y V-Ray](https://github.com/Azure/batch-extension-templates/blob/master/templates/maya/render-vray-windows/pool.template.json) en GitHub.

## <a name="applications-on-previous-windows-server-2016-rendering-images"></a>Aplicaciones en imágenes de representación de Windows Server 2016

La lista siguiente se aplica a las imágenes de representación de Windows Server 2016, versión 1.3.7.

* Autodesk Maya I/O 2017 Actualización 5 (versión 17.4.5459)
* Autodesk Maya I/O 2018 Actualización 4 (versión 18.4.0.7622)
* Autodesk 3ds Max I/O 2019 Actualización 1 (versión 21.2.0.2219)
* Autodesk 3ds Max I/O 2018 Actualización 4 (versión 20.4.0.4254)
* Autodesk Arnold para Maya 2017 (Arnold versión 5.2.0.1) MtoA-3.1.0.1-2017
* Autodesk Arnold para Maya 2018 (Arnold versión 5.2.0.1) MtoA-3.1.0.1-2018
* Autodesk Arnold para 3ds Max 2018 (Arnold versión 5.0.2.4) (versión 1.2.926)
* Autodesk Arnold para 3ds Max 2019 (Arnold versión 5.0.2.4) (versión 1.2.926)
* Chaos Group V-Ray para Maya 2018 (versión 3.52.03)
* Chaos Group V-Ray para 3ds Max 2018 (versión 3.60.02)
* Chaos Group V-Ray para Maya 2019 (versión 3.52.03)
* Chaos Group V-Ray para 3ds Max 2019 (versión 4.10.01)
* Blender (2.79)

> [!NOTE]
> Chaos Group V-Ray para 3ds Max 2019 (versión 4.10.01) introduce cambios importantes en V-ray. Para usar la versión anterior (versión 3.60.02), use los nodos de representación de Windows Server 2016, versión 1.3.2.

## <a name="next-steps"></a>Pasos siguientes

Para usar las imágenes de máquina virtual de representación, deben especificarse en la configuración del grupo cuando se crea; consulte las [funcionalidades del grupo de Batch para la representación](https://docs.microsoft.com/azure/batch/batch-rendering-functionality#batch-pools).
