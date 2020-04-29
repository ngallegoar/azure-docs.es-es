---
author: dcurwin
ms.service: backup
ms.topic: include
ms.date: 03/12/2020
ms.author: dacurwin
ms.openlocfilehash: 024f70a21b36bffa662ba6f568adc8efbd0a909d
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81539357"
---
Ahora puede usar [puntos de conexión privados](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) para realizar copias de seguridad de los datos de forma segura desde los servidores de una red virtual al almacén de Recovery Services. El punto de conexión privado usa una dirección IP del espacio de direcciones de la red virtual del almacén. El tráfico de red entre los recursos dentro de la red virtual y el almacén viaja a través de la red virtual y un vínculo privado en la red troncal de Microsoft. Esto elimina la exposición de la red pública de Internet. Los puntos de conexión privados se pueden usar para realizar copias de seguridad y restaurar las bases de datos de SQL y SAP HANA que se ejecutan dentro de las máquinas virtuales de Azure. También se puede usar para los servidores locales mediante el agente de MARS.

La copia de seguridad de máquinas virtuales de Azure no requiere conectividad a Internet y, por tanto, no requiere puntos de conexión privados para permitir el aislamiento de red.

>[!NOTE]
> Esta característica se encuentra actualmente en disponibilidad limitada y está disponible en las regiones Centro-oeste de EE. UU., Centro-sur de EE. UU., Oeste de EE. UU. 2 y Este de EE. UU. (disponibilidad en otras regiones de Azure para seguir). Rellene [esta encuesta](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0H3_nezt2RNkpBCUTbWEapUQk5EQ1QxRzVOWDNDS1Y1Q0xLTkdLQ0U0RC4u) y envíenos un correo electrónico a azbackupnetsec@microsoft.com si está interesado en usar puntos de conexión privados para Azure Backup. La posibilidad de utilizar esta característica está sujeta a la aprobación del servicio Azure Backup.
