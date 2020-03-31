---
title: Alta disponibilidad de Azure Analysis Services | Microsoft Docs
description: En este artículo se describe cómo Azure Analysis Services proporciona alta disponibilidad durante una interrupción del servicio.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 78a6d41b638d79111a58830f0cb0d5190ea0796c
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2020
ms.locfileid: "80408682"
---
# <a name="analysis-services-high-availability"></a>Alta disponibilidad de Azure Analysis Services

En este artículo se describe cómo garantizar la alta disponibilidad de los servidores de Azure Analysis Services. 

## <a name="assuring-high-availability-during-a-service-disruption"></a>Garantizar la alta disponibilidad durante una interrupción del servicio

Aunque es poco habitual, en los centros de datos de Azure pueden producirse interrupciones. Cuando esto ocurre, provoca también una interrupción en el negocio que podría extenderse unos pocos minutos o incluso horas. La alta disponibilidad se suele lograr con la redundancia de servidores. Con Azure Analysis Services, la redundancia se obtiene gracias a la creación de servidores secundarios adicionales en una o varias regiones. Para garantizar que los datos y metadatos de esos servidores redundantes que cree estén sincronizados con el servidor en una región que se ha desconectado, puede:

* Implementar modelos en servidores redundantes en otras regiones. Este método requiere el procesamiento en paralelo de los datos en el servidor principal y en los servidores redundantes, lo que asegura que todos los servidores están sincronizados.

* Realizar [copias de seguridad](analysis-services-backup.md) de las bases de datos del servidor principal y restaurarlas en los servidores redundantes. Por ejemplo, se puede automatizar la creación de copias de seguridad en Azure Storage durante la noche y hacer una restauración en los servidores redundantes de otras regiones. 

En cualquier caso, si se produce una interrupción en el servidor principal, se deben cambiar las cadenas de conexión en los clientes de informes para conectarse al servidor en otro centro de datos regional. Este cambio debe considerarse un último recurso y solo si se produce una interrupción catastrófica del centro de datos regional. Es más probable que una interrupción en el centro de datos que hospeda el servidor principal se recupere antes de que se puedan actualizar las conexiones en todos los clientes. 

Para evitar tener que cambiar las cadenas de conexión de los clientes de informes, puede crear un [alias](analysis-services-server-alias.md) de servidor para el servidor principal. Si el servidor principal deja de funcionar, puede cambiar el alias y dirigirlo a un servidor redundante de otra región. Puede automatizar el alias para el nombre del servidor mediante la codificación de una comprobación de estado del punto de conexión en el servidor principal. Si se produce un error en la comprobación del estado, el mismo punto de conexión puede dirigir a un servidor redundante de otra región. 

## <a name="related-information"></a>Información relacionada

[Copias de seguridad y restauración](analysis-services-backup.md)   
[Administración de Azure Analysis Services](analysis-services-manage.md)   
[Alias de nombre de servidor](analysis-services-server-alias.md) 

