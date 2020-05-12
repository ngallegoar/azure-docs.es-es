---
title: Reducción de los costos de servicio con Azure Advisor
description: Utilice Azure Advisor para optimizar el costo de las implementaciones de Azure.
ms.topic: article
ms.date: 01/29/2019
ms.openlocfilehash: 13e7b1d7c6b0fe342020c40e1bb4abeba97d18bb
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/05/2020
ms.locfileid: "82788100"
---
# <a name="reduce-service-costs-using-azure-advisor"></a>Reducción de los costos de servicio con Azure Advisor

Advisor lo ayuda a optimizar y reducir el gasto global de Azure mediante la identificación de recursos inactivos e infrautilizados. Puede obtener recomendaciones sobre el costo en la pestaña **Cost** (Costo) del panel de Advisor.

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a>Optimización del gasto en máquinas virtuales mediante la adecuación del tamaño o el apagado en instancias infrautilizadas 

Mientras que determinados escenarios de aplicaciones pueden dar lugar a un uso escaso debido al diseño, a menudo puede ahorrar dinero administrando el tamaño y número de máquinas virtuales. Los modelos de evaluación avanzada de Advisor consideran el cierre de una máquina virtual cuando el percentil 95 del valor máximo del uso de CPU es inferior al 3 % y el uso de la red es inferior al 2 % en un período de siete días. Se considera que las máquinas virtuales tienen el tamaño correcto cuando es posible ajustar la carga actual en una SKU más pequeña (de la misma familia de SKU) o un número de instancia inferior, de forma que el uso de la carga actual no supere el 80 % cuando las cargas de trabajo no estén orientadas a los usuarios y no supere el 40 % cuando sí lo estén. Aquí, el tipo de carga de trabajo se determina analizando las características de uso de la CPU de la carga de trabajo.

Las acciones recomendadas son apagar o cambiar de tamaño, en función de cada recurso. Advisor muestra el ahorro de costos estimado para cada acción recomendada: apagado o cambio de tamaño. Además, si se recomienda cambiar de tamaño, proporcionará información sobre la SKU actual y la de destino. 

Si desea que sea más exigente en la identificación de las máquinas virtuales infrautilizadas, puede ajustar la regla de uso de la CPU según la suscripción.

## <a name="reduce-costs-by-eliminating-unprovisioned-expressroute-circuits"></a>Reducción de los costos mediante la eliminación de circuitos ExpressRoute no aprovisionados

Advisor identifica los circuitos ExpressRoute que han permanecido en el estado de proveedor de *No aprovisionado* durante más de un mes y recomienda eliminar el circuito si no piensa realizar el aprovisionamiento del circuito con el proveedor de conectividad.

## <a name="reduce-costs-by-deleting-or-reconfiguring-idle-virtual-network-gateways"></a>Reducción de costos mediante la eliminación o reconfiguración de puertas de enlace de red virtual inactivas

Advisor identifica las puertas de enlace de red virtual que han estado inactivas durante más de 90 días. Dado que estas puertas de enlace se facturan por hora, debe considerar volver a configurar o eliminarlas si no va a utilizarlas. 

## <a name="buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs"></a>Compra de instancias reservadas de máquina virtual para ahorrar dinero en los costos de pago por uso

Advisor revisará el uso de la máquina virtual durante los últimos 30 días y determinará si podría ahorrar dinero mediante la adquisición de una reserva de Azure. Advisor le mostrará las regiones y los tamaños en los que, potencialmente, puede ahorrar más, así como el ahorro estimado al comprar reservas. Con las reservas de Azure, puede adquirir previamente los costos de base de las máquinas virtuales. Se aplicarán descuentos automáticamente a las máquinas virtuales nuevas o existentes que tengan el mismo tamaño y la misma región que sus reservas. [Más información sobre Azure Reserved VM Instances](https://azure.microsoft.com/pricing/reserved-vm-instances/).

Advisor también le notificará las instancias reservadas que tenga que expiren en los próximos 30 días. Le recomendará que compre instancias reservadas nuevas para evitar pagar el precio de pago por uso.

## <a name="delete-unassociated-public-ip-addresses-to-save-money"></a>Eliminación de direcciones IP públicas no asociadas para ahorrar dinero

Advisor identifica las direcciones IP públicas que no están asociadas actualmente a los recursos de Azure, como los equilibradores de carga o las máquinas virtuales. Estas direcciones IP públicas conllevan un cargo nominal. Si no piensa usarlas, eliminarlas pueden dar lugar a un ahorro de costos.

## <a name="delete-azure-data-factory-pipelines-that-are-failing"></a>Eliminación de las canalizaciones de Azure Data Factory que dan error

Azure Advisor detectará las canalizaciones de Azure Data Factory que produzcan un error repetidamente y recomienda resolver los problemas o eliminar dichas canalizaciones si ya no son necesarias. Se le facturará por estas canalizaciones aunque no le presten servicio mientras que produzcan errores. 

## <a name="use-standard-snapshots-for-managed-disks"></a>Uso de instantáneas estándar para Managed Disks
Para ahorrar el 60 % del costo, se recomienda almacenar las instantáneas en Standard Storage, independientemente del tipo de almacenamiento del disco principal. Esta opción es la predeterminada para las instantáneas de Managed Disks. Azure Advisor identificará las instantáneas de Premium Storage y se recomienda migrar la instantánea de Premium Storage a Standard Storage. [Más información sobre los precios de Managed Disks](https://aka.ms/aa_manageddisksnapshot_learnmore)

## <a name="utilize-lifecycle-management"></a>Uso de la administración del ciclo de vida
Azure Advisor utilizará la inteligencia relacionada con el recuento de objetos de Azure Blob Storage, el tamaño total y las transacciones para detectar si una o varias de sus cuentas de almacenamiento serían más adecuadas para habilitar la administración del ciclo de vida en los datos de niveles. Se le pedirá que cree reglas de administración del ciclo de vida para organizar automáticamente los datos en un nivel de acceso esporádico o el de archivo para optimizar los costos de almacenamiento, al tiempo que conserva los datos en Azure Blob Storage para que sean compatibles con las aplicaciones.

## <a name="create-an-ephemeral-os-disk-recommendation"></a>Creación de una recomendación de disco del sistema operativo efímero
Con el [disco del sistema operativo efímero](https://docs.microsoft.com/azure/virtual-machines/windows/ephemeral-os-disks), los clientes obtienen estas ventajas: Ahorran en el costo de almacenamiento para el disco del sistema operativo. Reducen la latencia de lectura y escritura en el disco del sistema operativo. Una operación de restablecimiento de la imagen inicial de la máquina virtual más rápida mediante el restablecimiento del sistema operativo (y del disco temporal) a su estado original. Es preferible usar el disco del sistema operativo efímero para máquinas virtuales IaaS de corta duración o máquinas virtuales con cargas de trabajo sin estado. Advisor tiene recomendaciones para los recursos que pueden beneficiarse del disco del sistema operativo efímero. 

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>Obtención de acceso a las recomendaciones sobre el costo en Azure Advisor

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Busque y seleccione [**Advisor**](https://aka.ms/azureadvisordashboard) desde cualquier página.

1. En el panel **Advisor**, seleccione la pestaña **Costo**.

## <a name="next-steps"></a>Pasos siguientes

Para aprender más sobre las recomendaciones de Advisor, consulte:
* [Introducción a Advisor](advisor-overview.md)
* [Primeros pasos](advisor-get-started.md)
* [Recomendaciones sobre rendimiento de Advisor](advisor-performance-recommendations.md)
* [Recomendaciones sobre alta disponibilidad de Advisor](advisor-high-availability-recommendations.md)
* [Recomendaciones sobre seguridad de Advisor](advisor-security-recommendations.md)
* [Recomendaciones de excelencia operativa de Advisor](advisor-operational-excellence-recommendations.md)
