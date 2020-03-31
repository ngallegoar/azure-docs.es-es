---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/23/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 7cfa6e9810057493cc3007eec7fd1668a70c727e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77179136"
---
El uso de máquinas virtuales de Spot permite aprovechar las ventajas de nuestra capacidad no utilizada con un importante ahorro en los costos. Siempre que Azure necesite recuperar la capacidad, su infraestructura expulsará las máquinas virtuales de Spot. Por lo tanto, estas son excelentes para cargas de trabajo que soportan interrupciones, como los trabajos de procesamiento por lotes, los entornos de desarrollo y pruebas, las grandes cargas de trabajo de proceso, etc.

La cantidad de capacidad sin usar disponible varía, por ejemplo, en función del tamaño, la región o la hora del día. Al implementar máquinas virtuales de Spot, Azure las asigna si hay capacidad disponible, pero no hay un Acuerdo de Nivel de Servicio para ellas. Una máquina virtual de Spot no ofrece garantías de alta disponibilidad. Siempre que Azure necesite recuperar la capacidad, su infraestructura expulsará las máquinas virtuales de Spot previo aviso 30 segundos antes. 

> [!IMPORTANT]
> Las instancias de Spot se encuentran actualmente en versión preliminar pública,
> la cual no se recomienda para las cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="eviction-policy"></a>Directiva de expulsión

Las máquinas virtuales se pueden expulsar en función de la capacidad o del precio máximo establecido. Para las máquinas virtuales, la directiva de expulsión se establece en *Deallocate* (Desasignar); por lo tanto, las que se hayan expulsado pasan al estado stopped-deallocated para que pueda volver a implementarlas posteriormente. Sin embargo, la reasignación de máquinas virtuales de Spot dependerá de la disponibilidad de capacidad de Spot. Las máquinas virtuales desasignadas se siguen teniendo en cuenta en la cuota de vCPU de Spot y se cobra por los discos subyacentes. 

Los usuarios pueden optar por recibir notificaciones en las máquinas virtuales mediante [Azure Scheduled Events](../articles/virtual-machines/linux/scheduled-events.md). De este modo se le notificará que se van a expulsar las máquinas virtuales y tendrá 30 segundos para terminar los trabajos y cerrar las tareas antes de que esto ocurra. 


| Opción | Resultado |
|--------|---------|
| El precio máximo se establece en un valor igual o mayor que el precio actual. | La máquina virtual se implementa si hay capacidad y cuota disponibles. |
| El precio máximo se establece en un valor menor que el precio actual. | La máquina virtual no se implementa. Obtendrá un mensaje de error que indicará que el precio máximo debe ser igual o mayor que el precio actual. |
| Reinicio de una máquina virtual detenida o desasignada si el precio máximo es igual o mayor que el precio actual | Si hay capacidad y cuota, la máquina virtual se implementa. |
| Reinicio de una máquina virtual detenida o desasignada si el precio máximo es menor que el precio actual | Obtendrá un mensaje de error que indicará que el precio máximo debe ser igual o mayor que el precio actual. | 
| El precio de la máquina virtual ha aumentado y ahora es mayor que el precio máximo. | La máquina virtual se expulsa. Recibirá una notificación 30 segundos antes de la expulsión. | 
| Después, el precio de la máquina virtual volverá a ser menor que el precio máximo. | La máquina virtual no se reiniciará automáticamente. Puede reiniciar la máquina virtual por su cuenta y se le cobrará el precio actual. |
| Si el precio máximo está establecido en `-1` | La máquina virtual no se expulsará por motivos de precio. El precio máximo será el precio actual, hasta como máximo el precio de las máquinas virtuales estándar. No se le cobrará por encima del precio estándar.| 
| Cambio del precio máximo | Debe desasignar la máquina virtual para cambiar el precio máximo. Desasigne la máquina virtual, establezca un nuevo precio máximo y actualícela. |

## <a name="limitations"></a>Limitaciones

No se admiten los siguientes tamaños de máquina virtual para Spot:
 - Serie B
 - Versiones de promoción de cualquier tamaño (como los tamaños de promoción Dv2, NV, NC, H)

En la actualidad, las máquinas virtuales de Spot no pueden usar discos de sistema operativo efímeros.

Las máquinas virtuales de Spot se pueden implementar en cualquier región, excepto Microsoft Azure China 21Vianet.

## <a name="pricing"></a>Precios

Los precios de las máquinas virtuales de Spot varían en función de la región y la SKU. Para más información, consulte precios de las máquinas virtuales para [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) y [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). 


La variabilidad en los precios permite establecer un precio máximo, en dólares estadounidenses (USD), con un máximo de 5 decimales. Por ejemplo, el valor `0.98765` correspondería a un precio máximo de 0,98765 USD por hora. Si establece el precio máximo en `-1`, la máquina virtual no se expulsará por precio. El precio de la máquina virtual será el actual de Spot o el de una máquina virtual estándar, el menor de los dos, siempre que haya capacidad y cuota disponibles.


##  <a name="frequently-asked-questions"></a>Preguntas más frecuentes

**P:** Una vez creadas, ¿son las máquinas virtuales de Spot iguales que las estándar normales?

**R:** Sí, salvo que no hay ningún Acuerdo de Nivel de Servicio para las máquinas virtuales de Spot y se pueden expulsar en cualquier momento.


**P:** ¿Qué se debe hacer si se produce la expulsión pero aún se necesita capacidad?

**R:** Si necesita capacidad de inmediato, se recomienda usar máquinas virtuales estándar en lugar de máquinas virtuales de Spot.


**P:** ¿Cómo se administra la cuota para las máquinas virtuales de Spot?

**R:** Las máquinas virtuales de Spot tienen un grupo de cuotas independiente. La cuota de Spot se compartirá entre las máquinas virtuales y los conjuntos de escalado. Para más información, consulte [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits).


**P:** ¿Puedo solicitar una cuota adicional para Spot?

**R:** Sí, podrá enviar la solicitud para aumentar su cuota para las máquinas virtuales de Spot mediante el [proceso de solicitud de cuota estándar](https://docs.microsoft.com/azure/azure-portal/supportability/per-vm-quota-requests).


**P:** ¿Qué canales admiten las máquinas virtuales de Spot?

**R:** Consulte la tabla siguiente para la disponibilidad de máquinas virtuales de Spot.

<a name="channel"></a>

| Canales de Azure               | Disponibilidad de las máquinas virtuales de Azure       |
|------------------------------|-----------------------------------|
| Contrato Enterprise         | Sí                               |
| Pago por uso                | Sí                               |
| Proveedor de servicios en la nube (CSP) | [Póngase en contacto con su asociado](https://docs.microsoft.com/partner-center/azure-plan-get-started) |
| Ventajas                     | No disponible                     |
| Patrocinados                    | No disponible                     |
| Versión de prueba gratuita                   | No disponible                     |


**P:** ¿Dónde puedo publicar preguntas?

**R:** Puede publicar y etiquetar la pregunta con `azure-spot` en [Q&A](https://docs.microsoft.com/answers/topics/azure-spot.html) (Preguntas y respuestas). 



