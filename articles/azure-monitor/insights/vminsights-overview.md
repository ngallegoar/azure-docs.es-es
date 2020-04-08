---
title: ¿Qué es Azure Monitor para máquinas virtuales?
description: Información general de Azure Monitor para VM, que se encarga de supervisar el estado y el rendimiento de las máquinas virtuales de Azure, además de detectar y asignar automáticamente los componentes de la aplicación y sus dependencias.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/11/2020
ms.openlocfilehash: 6f65ba96f768a4b88aa13c073cc66920b8c5e049
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480494"
---
# <a name="what-is-azure-monitor-for-vms"></a>¿Qué es Azure Monitor para máquinas virtuales?

Azure Monitor para VM supervisa las máquinas virtuales (VM) y los conjuntos de escalado de máquinas virtuales de Azure. El servicio analiza el rendimiento y el estado de las VM Windows y Linux, y supervisa sus procesos y dependencias en otros recursos y procesos externos. Permite supervisar el rendimiento y las dependencias de las aplicaciones en máquinas virtuales que están hospedadas en el entorno local o en otro proveedor de servicios en la nube. Las siguientes características clave ofrecen información detallada:

- **Gráficos de rendimiento de tendencias previamente definidos**: Muestra las principales métricas de rendimiento del sistema operativo de la VM.

- **Mapa de dependencias**: Muestra los componentes interconectados con la VM de varios grupos de recursos y suscripciones.  

>[!NOTE]
>Recientemente [anunciamos cambios](https://azure.microsoft.com/updates/updates-to-azure-monitor-for-virtual-machines-preview-before-general-availability-release/
) que estamos realizando en la característica de mantenimiento en función de los comentarios que hemos recibido de nuestros clientes de la versión preliminar pública. Dado el número de cambios que realizaremos, vamos a dejar de ofrecer esta característica a los nuevos clientes. Los clientes existentes pueden seguir usando la característica de mantenimiento. Para más información, consulte las [preguntas frecuentes sobre la disponibilidad general](vminsights-ga-release-faq.md).  

La integración con los registros de Azure Monitor ofrece una agregación y un filtrado eficaces, lo que permite a Azure Monitor para VM analizar las tendencias de los datos a lo largo del tiempo. Puede ver estos datos en una sola máquina virtual directamente desde la máquina virtual, o puede usar Azure Monitor para ofrecer una vista agregada de las máquinas virtuales; la vista admite los modos de contexto del recurso de Azure o de contexto del área de trabajo. Para más información, consulte [Introducción a los modos de acceso](../platform/design-logs-deployment.md#access-mode).

![Perspectiva de conclusiones de la máquina virtual desde Azure Portal](media/vminsights-overview/vminsights-azmon-directvm.png)

Azure Monitor para VM puede ofrecer un rendimiento y disponibilidad predecibles de aplicaciones vitales. Identifica los cuellos de botella de rendimiento y los problemas de red, y también puede ayudarle a entender si un problema está relacionado con otras dependencias.  

## <a name="data-usage"></a>Uso de datos

Cuando implementa Azure Monitor para VM, los datos que recopilen sus VM se ingieren y almacenan en Azure Monitor. Los datos de rendimiento y dependencia recopilados se almacenan en un área de trabajo de Log Analytics. Según los precios que se publican en la [página de precios de Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/), Azure Monitor para VM se factura según:

- Los datos que se ingieren y almacenan.
- Las reglas de alerta que se crean.
- Las notificaciones que se envían. 

El tamaño del registro varía en función de las longitudes de cadena de los contadores de rendimiento y puede aumentar con el número de discos lógicos y adaptadores de red asignados a la máquina virtual. Si ya tiene un área de trabajo y está recopilando estos contadores, no se aplicará ningún cargo duplicado. Si ya usa Service Map, el único cambio que verá son los datos de conexión adicionales que se envían a Azure Monitor.

## <a name="next-steps"></a>Pasos siguientes

Para conocer los requisitos y los métodos para habilitar la supervisión de máquinas virtuales, consulte [Implementación de Azure Monitor para VM](vminsights-enable-overview.md).
