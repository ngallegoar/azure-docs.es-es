---
title: 'Microsoft Threat Modeling Tool, versión 02/11/2020: Azure'
description: Documentación de las notas de la versión de Threat Modeling Tool versión 7.3.00206.1.
author: jegeib
ms.author: jegeib
ms.service: security
ms.topic: article
ms.date: 02/25/2020
ms.openlocfilehash: fa89ffcfd37629f95fbf281a6a703a173acd5d48
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91317929"
---
# <a name="threat-modeling-tool-update-release-73002061---02112020"></a>Threat Modeling Tool, versión actualizada 7.3.00206.1: 11 de febrero de 2020

El 11 de febrero de 2020 se lanzó la versión 7.3.00206.1 de Microsoft Threat Modeling Tool (TMT), que incluye los cambios siguientes:

- Corrección de errores

## <a name="notable-bug-fixes"></a>Correcciones de errores importantes

### <a name="errors-related-to-priority-values-outside-of-the-expected-ranges"></a>Errores relacionados con los valores de prioridad fuera de los intervalos esperados

Algunos clientes han informado que reciben el siguiente mensaje de error al abrir los archivos creados en "Threat Modeling Tool 2016" o en plantillas personalizadas:

```output
System.InvalidOperationException: Invalid Priority value. Accepted values are [0..4] and 'High', 'Medium', 'Low' at ThreatModeling.Model.Threat.get_Priority()

System.ArgumentOutOfRangeException: Accepted values are 'High', 'Medium', and 'Low' Parameter name: value Actual value was 5.6. at ThreatModeling.Model.Threat.set_Priority(String value)
```

Este problema se resolvió en esta versión.

## <a name="system-requirements"></a>Requisitos del sistema

- Sistemas operativos compatibles
  - [Actualización de aniversario de Microsoft Windows 10](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) o posterior
- Versión de .NET necesaria
  - [.Net 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262) o posterior
- Requisitos adicionales
  - Se necesita una conexión a Internet para recibir actualizaciones de la herramienta, así como plantillas.

## <a name="documentation-and-feedback"></a>Documentación y comentarios

- La documentación de Threat Modeling Tool se encuentra en [docs.microsoft.com](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool) e incluye información [sobre el uso de la herramienta](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-getting-started).

## <a name="next-steps"></a>Pasos siguientes

Descargue la versión más reciente de [Microsoft Threat Modeling Tool](https://aka.ms/threatmodelingtool).
