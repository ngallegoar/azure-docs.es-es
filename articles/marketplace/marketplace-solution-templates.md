---
title: Guía de publicación de ofertas de plantillas de solución de aplicaciones de Azure | Azure Marketplace
description: En este artículo se describen los requisitos para publicar una plantilla de solución en Azure Marketplace.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 9/25/2019
ms.author: dsindona
ms.openlocfilehash: c84436015ad37b57f6603551f1d328ac76181836
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288740"
---
# <a name="azure-applications-solution-template-offer-publishing-guide"></a>Aplicaciones de Azure: guía de publicación de ofertas de plantillas de solución

Las plantillas de solución son uno de los principales mecanismos para publicar una solución en Marketplace. Use esta guía para comprender los requisitos para esta oferta. 

Use el tipo de oferta Aplicación de Azure: plantilla de solución cuando la solución requiera automatización adicional de implementación y configuración para algo más que una sola máquina virtual. Puede automatizar el aprovisionamiento de una o más VM con plantillas de solución de aplicaciones de Azure. También puede aprovisionar los recursos de redes y almacenamiento. El tipo de oferta Aplicaciones de Azure: plantillas de solución ofrece ventajas de automatización para VM únicas y soluciones completas basadas en IaaS.

Estas plantillas de solución no son ofertas de transacción, pero se pueden usar para implementar las ofertas de máquina virtual de pago facturadas mediante Marketplace comercial de Microsoft. La llamada a la acción que el usuario ve es "Obtener ahora".


## <a name="requirements-for-solution-templates"></a>Requisitos para las plantillas de solución

| **Requisitos** | **Detalles**  |
| ---------------  | -----------  |
|Facturación y medición    |  Los recursos se aprovisionarán en la suscripción a Azure del cliente. Las transacciones de máquinas virtuales de pago por uso (PAYGO) se realizarán con el cliente mediante Microsoft y se facturarán a través de la suscripción a Azure del cliente (PAYGO).  <br/> En el caso del modelo denominado traiga su propia licencia (BYOL), Microsoft facturará los costos de infraestructura derivados de la suscripción del cliente, mientras que usted realizará la transacción de las tarifas de licencia de software directamente con el cliente.   |
|Disco duro virtual (VHD) compatible con Azure  |   Las máquinas virtuales deben estar basadas en Windows o Linux.  Para más información, consulte [Creación de un disco duro virtual compatible con Azure](./cloud-partner-portal/virtual-machine/cpp-create-vhd.md). |
| Atribución de uso del cliente | Se requiere habilitar la atribución de uso del cliente en todas las plantillas de solución publicadas en Azure Marketplace. Para más información sobre la atribución de uso del cliente y cómo habilitarla, consulte [Atribución de uso del cliente para asociados de Azure](./azure-partner-customer-usage-attribution.md).  |
| Uso de Managed Disks | [Managed Disks](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) es la opción predeterminada para los discos persistentes de máquinas virtuales de IaaS en Azure. Debe usar Managed Disks en Plantillas de solución. <br> <br> 1. Siga las [instrucciones](https://docs.microsoft.com/azure/virtual-machines/windows/using-managed-disks-template-deployments) y [ejemplos](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md) para usar Managed Disks en las plantillas de Azure ARM y actualizar las plantillas de la solución. <br> <br> 2. Siga las instrucciones que se indican a continuación para importar el disco duro virtual subyacente de Managed Disks a una cuenta de almacenamiento para publicar el disco duro virtual como una imagen en Marketplace: <br> <ul> <li> [PowerShell](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-vhd?toc=%2fpowershell%2fmodule%2ftoc.json) </li> <li> [CLI](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-vhd?toc=%2fcli%2fmodule%2ftoc.json) </li> </ul> |

## <a name="next-steps"></a>Pasos siguientes
Si no lo ha hecho ya, [regístrese](https://azuremarketplace.microsoft.com/sell) en Marketplace.

Si ya lo está y va a crear una oferta o trabaja en una existente, inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com) para crear o completar la oferta.
