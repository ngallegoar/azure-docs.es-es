---
author: dcurwin
ms.service: backup
ms.topic: include
ms.date: 03/12/2020
ms.author: dacurwin
ms.openlocfilehash: fd7124ad83a446d7dde39a836c337a97b12a0a0a
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95560794"
---
Ahora puede usar [puntos de conexión privados](../articles/private-link/private-endpoint-overview.md) para realizar copias de seguridad de los datos de forma segura desde los servidores de una red virtual al almacén de Recovery Services. El punto de conexión privado usa una dirección IP del espacio de direcciones de la red virtual del almacén. El tráfico de red entre los recursos dentro de la red virtual y el almacén viaja a través de la red virtual y un vínculo privado en la red troncal de Microsoft. Esto elimina la exposición de la red pública de Internet. Los puntos de conexión privados se pueden usar para realizar copias de seguridad y restaurar las bases de datos de SQL y SAP HANA que se ejecutan dentro de las máquinas virtuales de Azure. También se puede usar para los servidores locales mediante el agente de MARS.

La copia de seguridad de máquinas virtuales de Azure no requiere conectividad a Internet y, por tanto, no requiere puntos de conexión privados para permitir el aislamiento de red.

Obtenga más información sobre los puntos de conexión privados para Azure Backup [aquí](../articles/backup/private-endpoints.md).