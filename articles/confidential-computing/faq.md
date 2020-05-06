---
title: Preguntas frecuentes de computación confidencial de Azure
description: Respuestas a las preguntas frecuentes sobre la computación confidencial de Azure.
author: JBCook
ms.topic: troubleshooting
ms.workload: infrastructure
ms.service: virtual-machines
ms.subservice: workloads
ms.date: 4/17/2020
ms.author: jencook
ms.openlocfilehash: e9cb691ef60f612672078a9ef84db904c79cbc87
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82189452"
---
# <a name="frequently-asked-questions-for-azure-confidential-computing"></a>Preguntas frecuentes de computación confidencial de Azure

En este artículo se proporcionan respuestas a algunas de las preguntas más comunes sobre la ejecución de [cargas de trabajo de computación confidencial en Azure](overview.md).

Si su problema con Azure no se trata en este artículo, visite los foros de Azure en [MSDN y Stack Overflow](https://azure.microsoft.com/support/forums/). Puede publicar su problema en ellos o [@AzureSupport en Twitter](https://twitter.com/AzureSupport). También puede enviar una solicitud de soporte técnico de Azure. Para enviar una solicitud de soporte técnico, en la [página de soporte técnico de Azure](https://azure.microsoft.com/support/options/), seleccione Obtener soporte técnico.

## <a name="confidential-computing-virtual-machines"></a>Máquinas virtuales de computación confidencial<a id="vm-faq"></a>

1. **¿Cómo puede empezar a implementar máquinas virtuales de la serie DCsv2?**

   Estas son algunas formas de implementar una máquina virtual DCsv2:
   - Uso de una [plantilla de Azure Resource Manager](../virtual-machines/windows/template-description.md)
   - En [Azure Portal](https://portal.azure.com/#create/hub)
   - En la plantilla de solución de Marketplace [Azure confidential compute (Virtual Machine)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-compute.acc-virtual-machine-v2?tab=overview). La plantilla de solución de Marketplace le ayudará a restringir un cliente a los escenarios admitidos (regiones, imágenes, disponibilidad, cifrado de disco). 

1. **¿Funcionarán todas las imágenes de sistema operativo con la computación confidencial de Azure?**

   No. Las máquinas virtuales solo se pueden implementar en máquinas virtuales de generación 2. Ofrecemos compatibilidad con la generación 2 para Ubuntu Server 18.04, Ubuntu Server 16.04 y Windows Server 2016 Datacenter. Obtenga más información sobre las máquinas virtuales de generación 2 en [Linux](../virtual-machines/linux/generation-2.md) y [Windows](../virtual-machines/windows/generation-2.md)

1. **Las máquinas virtuales de DCsv2 aparecen atenuadas en el portal y no puedo seleccionar ninguna**.

   En función de la burbuja de información que se encuentra junto a la máquina virtual, hay que realizar diferentes acciones:
    -   **UnsupportedGeneration**: cambie la generación de la imagen de máquina virtual a "Gen2".
    -   **NotAvailableForSubscription**: la región todavía no está disponible para su suscripción. Seleccione una región disponible.
    -   **InsufficientQuota**: [cree una solicitud de soporte técnico para aumentar la cuota](../azure-portal/supportability/per-vm-quota-requests.md). Las suscripciones de evaluación gratuita no tienen cuota de máquinas virtuales de computación confidencial. 

1. **Las máquinas virtuales de DCsv2 no aparecen cuando intento buscarlas en el selector de tamaño del portal**.

   Asegúrese de que ha seleccionado una región disponible. Asegúrese también de seleccionar "Borrar todos los filtros" en el selector de tamaño. 

1. **¿Cuál es la diferencia entre las máquinas virtuales de la serie DCsv2 y la serie DC?**

   Las máquinas virtuales de la serie DC se ejecutan en procesadores Intel de 6 núcleos anteriores con Intel SGX. Estos tienen menos memoria total y menos EPC (enclave Page Cage) y están disponibles en menos regiones. Estas máquinas virtuales solo están disponibles en el Este de EE. UU. y Oeste de Europa en dos tamaños: Standard_DC2s y Standard_DC4s. No entrarán en disponibilidad general y solo se pueden implementar en la instancia de Marketplace [Confidential Compute DC-Series VM [Preview]](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-compute.confidentialcompute?tab=Overview) (Máquina virtual de la serie DC de computación confidencial [versión preliminar]).

1. **¿Las máquinas virtuales de DCsv2 están disponibles globalmente?**

   No, estas máquinas virtuales solo están disponibles en regiones seleccionadas. Active la [página de productos por regiones](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines) para ver las últimas regiones disponibles. 

1. **¿Cómo instalo el SDK de Open Enclave?**
   
   Para obtener instrucciones sobre cómo instalar el SDK de OE en una máquina, ya sea en Azure o de forma local, siga las instrucciones del [GitHub del SDK de Open Enclave](https://github.com/openenclave/openenclave).
     
   También puede consultar el GitHub del SDK de Open Enclave para obtener instrucciones de instalación específicas del sistema operativo.
     - [Instalación del SDK de OE en Windows](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/install_oe_sdk-Windows.md)
     - [Instalación del SDK de OE en Ubuntu 18.04](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/install_oe_sdk-Ubuntu_18.04.md)
     - [Instalación del SDK de OE en Ubuntu 16.04](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/install_oe_sdk-Ubuntu_16.04.md)
