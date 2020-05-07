---
title: 'Latencia de la conexión para usuarios de Windows Virtual Desktop: Azure'
description: Latencia de la conexión para usuarios de Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 8a60779fb045aa612a6ba0988c4635752f973f60
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2020
ms.locfileid: "82607408"
---
# <a name="determine-user-connection-latency-in-windows-virtual-desktop"></a>Determinación de la latencia de la conexión de los usuarios en Windows Virtual Desktop

Windows Virtual Desktop está disponible a nivel mundial. Los administradores pueden crear máquinas virtuales (VM) en cualquier región de Azure que deseen. La latencia de la conexión variará en función de la ubicación de los usuarios y las máquinas virtuales. Los servicios de Windows Virtual Desktop se implementarán continuamente en nuevas zonas geográficas para mejorar la latencia. 
 
La [herramienta Estimador de experiencia de Windows Virtual Desktop](https://azure.microsoft.com/services/virtual-desktop/assessment/) puede ayudarle a determinar la mejor ubicación para optimizar la latencia de las máquinas virtuales. Se recomienda usar la herramienta cada dos o tres meses para asegurarse de que la ubicación óptima no ha cambiado a medida que Windows Virtual Desktop se implementa en nuevas áreas. 

## <a name="azure-traffic-manager"></a>Administrador de tráfico de Azure

Windows Virtual Desktop emplea Azure Traffic Manager, que comprueba la ubicación del servidor DNS del usuario para encontrar la instancia más cercana de este servicio. Se recomienda a los administradores que revisen la ubicación del servidor DNS del usuario antes de elegir la ubicación de las máquinas virtuales.

## <a name="next-steps"></a>Pasos siguientes

- Para comprobar cuál es la mejor ubicación de cara a una latencia óptima, consulte la [herramienta Estimador de experiencia de Windows Virtual Desktop](https://azure.microsoft.com/services/virtual-desktop/assessment/).
- Para información sobre los planes de precios, consulte [Precios de Windows Virtual Desktop](https://azure.microsoft.com/pricing/details/virtual-desktop/).
- Para empezar a trabajar con la implementación de Windows Virtual Desktop, consulte [nuestro tutorial](./virtual-desktop-fall-2019/tenant-setup-azure-active-directory.md).