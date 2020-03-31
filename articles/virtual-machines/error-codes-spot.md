---
title: Códigos de error para las máquinas virtuales y los conjuntos de escalado de Azure Spot
description: Más información sobre los códigos de error que podría ver al usar las máquinas virtuales y los conjuntos de escalado de Spot.
author: cynthn
manager: gwallace
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 11/20/2019
ms.author: cynthn
ms.openlocfilehash: 1863c510967dcf80044d768aee1a37643a068ce4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77115854"
---
# <a name="preview-error-messages-for-spot-vms-and-scale-sets"></a>Vista previa: Mensajes de error de las máquinas virtuales y los conjuntos de escalado de Spot


> [!IMPORTANT]
> Las máquinas virtuales y los conjuntos de escalado de máquinas virtuales de Spot se encuentran actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


Estos son algunos de los códigos de error que podría recibir al usar las máquinas virtuales de Spot y los conjuntos de escalado.


| Clave | Message | Descripción |
|-----|---------|-------------|
| SkuNotAvailable | El nivel solicitado para el recurso "\<recurso\>" no está disponible actualmente en la ubicación "\<ubicación\>" para la suscripción "\<IDdesuscripción\>". Pruebe otro nivel o realice la implementación en una ubicación diferente. | No hay suficiente capacidad en Azure Spot para crear la máquina virtual o el conjunto de escalado. |
| EvictionPolicyCanBeSetOnlyOnAzureSpotVirtualMachines  |  La directiva de expulsión solo se puede establecer en las máquinas virtuales de Azure Spot. | Esta máquina virtual no forma parte de Spot, por lo que no se puede establecer la directiva de expulsión. |
| AzureSpotVMNotSupportedInAvailabilitySet  |  La máquina virtual de Azure Spot no es compatible con el conjunto de disponibilidad. | Tiene la opción de usar una máquina virtual de Spot o una de un conjunto de disponibilidad, no ambas. |
| AzureSpotFeatureNotEnabledForSubscription  |  Suscripción no habilitada con la característica Azure Spot. | Necesita una suscripción que admita las máquinas virtuales de Spot. |
| VMPriorityCannotBeApplied  |  El valor de prioridad especificado "{0}" no se puede aplicar a la máquina virtual "{1}", ya que no se especificó ninguna prioridad al crearla. | Debe especificar la prioridad al crear la máquina virtual. |
| SpotPriceGreaterThanProvidedMaxPrice  |  No se pudo realizar la operación "{0}" porque el precio máximo proporcionado ("{1} USD") es menor que el precio actual de Spot ("{2} USD") para el tamaño de máquina virtual de Azure Spot "{3}". | Seleccione un precio máximo mayor. Para más información, consulte la información de precios para [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) o [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/).|
| MaxPriceValueInvalid  |  Valor de precio máximo no válido. Los únicos valores admitidos de precio máximo son -1 o un valor decimal mayor que cero. El valor de precio máximo -1 indica que la máquina virtual de Azure Spot no se expulsará por razones de precio. | Escriba un precio máximo válido. Para más información, consulte la información de precios para [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) o [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). |
| MaxPriceChangeNotAllowedForAllocatedVMs | No se permite cambiar el precio máximo con la máquina virtual "{0}" asignada. Cancele la asignación e inténtelo de nuevo. | Detenga la máquina virtual o cancele su asignación para cambiar el precio máximo. |
| MaxPriceChangeNotAllowed | No se permite el cambio de precio máximo. | No se puede cambiar el precio máximo para esta máquina virtual. |
| AzureSpotIsNotSupportedForThisAPIVersion  |  Azure Spot no es compatible con esta versión de API. | La versión de API debe ser 2019-03-01. |
| AzureSpotIsNotSupportedForThisVMSize  |  Azure Spot no es compatible con este tamaño de máquina virtual: {0}. | Seleccione otro. Para más información, consulte [Máquinas virtuales de Spot](./linux/spot-vms.md). |
| MaxPriceIsSupportedOnlyForAzureSpotVirtualMachines  |  El precio máximo solo es compatible con las máquinas virtuales de Azure Spot. | Para más información, consulte [Máquinas virtuales de Spot](./linux/spot-vms.md). |
| MoveResourcesWithAzureSpotVMNotSupported  |  La solicitud Mover recursos contiene una máquina virtual de Azure Spot. Actualmente no se admite. Consulte los detalles del error para conocer los identificadores de máquina virtual. | Las máquinas virtuales de Spot no se pueden mover. |
| MoveResourcesWithAzureSpotVmssNotSupported  |  La solicitud Mover recursos contiene un conjunto de escalado de máquinas virtuales de Azure Spot. Actualmente no se admite. Consulte los detalles del error para conocer los identificadores de conjunto de escalado de máquinas virtuales. | Los conjuntos de escalado de Spot no se pueden mover. |
| EphemeralOSDisksNotSupportedForSpotVMs | Las máquinas virtuales de Spot no admiten discos de sistema operativo efímeros. | En este caso, debe usar uno normal. |
| AzureSpotVMNotSupportedInVmssWithVMOrchestrationMode | Con el modo de orquestación de máquinas virtuales en el conjunto de escalado no se admiten máquinas virtuales de Azure Spot. | Permita el uso de instancias de Spot en el modo de orquestación del conjunto de escalado de máquinas virtuales. |


**Pasos siguientes** Para más información, consulte [Máquinas virtuales de Spot](./linux/spot-vms.md).