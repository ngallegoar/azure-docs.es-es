---
author: dcurwin
ms.service: backup
ms.topic: include
ms.date: 03/12/2020
ms.author: dacurwin
ms.openlocfilehash: b8941bb4fa77cb0008cb7271681e972bc21d6588
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "83659521"
---
Ahora puede usar [puntos de conexión privados](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) para realizar copias de seguridad de los datos de forma segura desde los servidores de una red virtual al almacén de Recovery Services. El punto de conexión privado usa una dirección IP del espacio de direcciones de la red virtual del almacén. El tráfico de red entre los recursos dentro de la red virtual y el almacén viaja a través de la red virtual y un vínculo privado en la red troncal de Microsoft. Esto elimina la exposición de la red pública de Internet. Los puntos de conexión privados se pueden usar para realizar copias de seguridad y restaurar las bases de datos de SQL y SAP HANA que se ejecutan dentro de las máquinas virtuales de Azure. También se puede usar para los servidores locales mediante el agente de MARS.

La copia de seguridad de máquinas virtuales de Azure no requiere conectividad a Internet y, por tanto, no requiere puntos de conexión privados para permitir el aislamiento de red.

Obtenga más información sobre los puntos de conexión privados para Azure Backup [aquí](https://docs.microsoft.com/azure/backup/private-endpoints).
