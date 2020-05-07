---
title: Guía de publicación de ofertas de plantillas de solución de aplicaciones de Azure | Azure Marketplace
description: En este artículo se describen los requisitos para publicar una plantilla de solución en Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/22/2020
ms.author: dsindona
ms.openlocfilehash: 4cce509b8f4db7549a05eccc3127d2c437f9c037
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2020
ms.locfileid: "82594583"
---
# <a name="azure-applications-solution-template-offer-publishing-requirements"></a>Aplicaciones de Azure: Requisitos de publicación de ofertas de plantillas de solución

En este artículo se explican los requisitos para el tipo de oferta de plantilla de solución, que es una manera de publicar una oferta de aplicación de Azure en Azure Marketplace. El tipo de oferta de plantilla de solución requiere una [plantilla de Azure Resource Manager (plantilla de ARM)](../azure-resource-manager/templates/overview.md) para implementar automáticamente la infraestructura de la solución.

Use el tipo de oferta de plantilla de solución de aplicación de Azure en las siguientes circunstancias:

- La solución requiere la automatización adicional de la implementación y la configuración no solo de una máquina virtual, sino también de una combinación de máquinas virtuales, redes y recursos de almacenamiento.
- El cliente va a administrar la solución por su cuenta.

La llamada a la acción que un usuario ve para este tipo de oferta es "Obtenerla ahora".

## <a name="requirements-for-solution-template-offers"></a>Requisitos de las ofertas de plantillas de solución

| **Requisitos** | **Detalles**  |
| ---------------  | -----------  |
|Facturación y medición    |  Las ofertas de plantillas de solución no son ofertas de transacción, sino que se pueden usar para implementar ofertas de máquina virtual de pago facturadas mediante Marketplace comercial de Microsoft. Los recursos que implementa la plantilla de ARM de la solución se aprovisionarán en la suscripción de Azure del cliente. Las transacciones de máquinas virtuales de pago por uso (PAYGO) se realizarán con el cliente mediante Microsoft y se facturarán a través de la suscripción de Azure del cliente.<br/> En el caso del modelo denominado traiga su propia licencia (BYOL), Microsoft facturará los costos de infraestructura derivados de la suscripción del cliente, mientras que usted realizará la transacción de las tarifas de licencia de software directamente con el cliente.   |
|Disco duro virtual (VHD) compatible con Azure  |   Las máquinas virtuales deben estar basadas en Windows o Linux. Para más información, consulte: <ul> <li>[Creación de una oferta de aplicación de Azure](./partner-center-portal/create-new-azure-apps-offer.md) (para discos duros virtuales de Windows).</li><li>[Distribuciones de Linux aprobadas en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) (para discos duros virtuales de Linux).</li></ul> |
| Atribución de uso del cliente | Se requiere habilitar la atribución de uso del cliente en todas las plantillas de solución publicadas en Azure Marketplace. Para más información sobre la atribución de uso del cliente y cómo habilitarla, consulte [Atribución de uso del cliente para asociados de Azure](./azure-partner-customer-usage-attribution.md).  |
| Uso de Managed Disks | [Managed Disks](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) es la opción predeterminada para los discos persistentes de máquinas virtuales de IaaS en Azure. Debe usar Managed Disks en plantillas de solución. <br> <br> 1. Siga las [instrucciones](https://docs.microsoft.com/azure/virtual-machines/windows/using-managed-disks-template-deployments) y [ejemplos](https://github.com/Azure/azure-quickstart-templates) para usar Managed Disks en las plantillas de Azure ARM y actualizar las plantillas de la solución. <br> <br> 2. Siga las instrucciones que se indican a continuación para importar el disco duro virtual subyacente de Managed Disks a una cuenta de almacenamiento a fin de publicarlo como una imagen en Marketplace: <br> <ul> <li> [PowerShell](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-vhd?toc=%2fpowershell%2fmodule%2ftoc.json) </li> <li> [CLI](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-vhd?toc=%2fcli%2fmodule%2ftoc.json) </li> </ul> |

## <a name="next-steps"></a>Pasos siguientes

- Si aún no lo ha hecho, [aprenda](https://azuremarketplace.microsoft.com/sell) sobre Azure Marketplace.
- [Inicie sesión en el Centro de Partners](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) para crear o completar la oferta.
- Para más información, consulte [Creación de una oferta de aplicaciones de Azure](./partner-center-portal/create-new-azure-apps-offer.md).
