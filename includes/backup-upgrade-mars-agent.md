---
title: Actualización del agente de Azure Backup
description: Aquí se explica por qué se debe actualizar el agente de Azure Backup y dónde se descarga la actualización.
services: backup
cloud: ''
suite: ''
author: dcurwin
manager: carmonm
ms.service: backup
ms.tgt_pltfrm: <optional>
ms.devlang: <optional>
ms.topic: article
ms.date: 03/03/2020
ms.author: dacurwin
ms.openlocfilehash: bd298f758d6109b908db01dd2ae3b97e5e2f714a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78673182"
---
## <a name="upgrade-the-mars-agent"></a>Actualización del agente de MARS

Las versiones del agente de Microsoft Azure Recovery Services (MARS) inferiores a la 2.0.9083.0 tienen una dependencia en Azure Access Control Service. El agente de MARS también se conoce como agente de Azure Backup.

En 2018, Azure [retiró Azure Access Control Service](../articles/active-directory/azuread-dev/active-directory-acs-migration.md). A partir del 19 de marzo de 2018, todas las versiones del agente MARS inferiores a la 2.0.9083.0 sufrirán errores de copia de seguridad. Para evitar o resolver dichos errores, [actualice el agente de MARS a la versión más reciente](https://support.microsoft.com/help/4538314/update-for-azure-backup-for-microsoft-azure-recovery-services-agent). Para identificar los servidores que necesitan una actualización del agente de MARS, siga los pasos de [Actualización del agente de Microsoft Azure Recovery Services (MARS)](../articles/backup/upgrade-mars-agent.md).

El agente de MARS se usa para hacer copias de seguridad de archivos, carpetas y datos de estado del sistema en Azure. System Center DPM y Azure Backup Server usan al agente de MARS para hacer una copia de seguridad de los datos en Azure.
