---
title: 'Microsoft Threat Modeling Tool, versión 16/10/2019: Azure'
description: Documentación de las notas de la versión de Threat Modeling Tool versión 7.1.61015.1.
author: jegeib
ms.author: jegeib
ms.service: security
ms.topic: article
ms.date: 10/16/2019
ms.openlocfilehash: 761ad45496e799a555b60480ff575b9d8f30e984
ms.sourcegitcommit: 5831eebdecaa68c3e006069b3a00f724bea0875a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94516992"
---
# <a name="threat-modeling-tool-update-release-71610151---10162019"></a>Threat Modeling Tool, versión actualizada 7.1.61015.1: 16 de octubre de 2019

El 16 de octubre de 2019 se lanzó la versión 7.1.61015.1 de Microsoft Threat Modeling Tool (TMT), que contiene los siguientes cambios:

- Mejoras de accesibilidad
- Corrección de errores
- Nuevas galerías de símbolos para Azure Logic Apps y Azure Data Explorer

## <a name="notable-bug-fixes"></a>Correcciones de errores importantes

### <a name="improved-backward-compatibility-with-files-created-in-threat-modeling-tool-2016"></a>Compatibilidad mejorada con versiones anteriores de los archivos creados en "Threat Modeling Tool 2016"

Se han corregido varios errores relacionados con la apertura o la presentación de los archivos de modelo de amenazas creados en "Threat Modeling Tool 2016".

## <a name="feature-enhancements"></a>Mejoras de las características

### <a name="new-stencils-for-azure-logic-apps-and-azure-data-explorer"></a>Nuevas galerías de símbolos para Azure Logic Apps y Azure Data Explorer

Se han agregado nuevas galerías de símbolos para Azure Logic Apps y Azure Data Explorer a la galería de símbolos de Azure junto con sus amenazas y mitigaciones asociadas.

![Galerías de símbolos de Azure Logic Apps y Azure Data Explorer](./media/threat-modeling-tool-releases-71610151/tmt-logic-apps.png)

## <a name="known-issues"></a>Problemas conocidos

### <a name="errors-related-to-priority-values-outside-of-the-expected-ranges"></a>Errores relacionados con los valores de prioridad fuera de los intervalos esperados

Algunos clientes han informado que reciben el siguiente mensaje de error al abrir los archivos creados en "Threat Modeling Tool 2016" o en plantillas personalizadas:

```output
System.InvalidOperationException: Invalid Priority value. Accepted values are [0..4] and 'High', 'Medium', 'Low' at ThreatModeling.Model.Threat.get_Priority()

System.ArgumentOutOfRangeException: Accepted values are 'High', 'Medium', and 'Low' Parameter name: value Actual value was 5.6. at ThreatModeling.Model.Threat.set_Priority(String value)
```

Este problema se está investigando.

## <a name="system-requirements"></a>Requisitos del sistema

- Sistemas operativos compatibles
  - [Actualización de aniversario de Microsoft Windows 10](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) o posterior
- Versión de .NET necesaria
  - [.Net 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262) o posterior
- Requisitos adicionales
  - Se necesita una conexión a Internet para recibir actualizaciones de la herramienta, así como plantillas.

## <a name="documentation-and-feedback"></a>Documentación y comentarios

- La documentación de Threat Modeling Tool se encuentra en [docs.microsoft.com](./threat-modeling-tool.md) e incluye información [sobre el uso de la herramienta](./threat-modeling-tool-getting-started.md).

## <a name="next-steps"></a>Pasos siguientes

Descargue la versión más reciente de [Microsoft Threat Modeling Tool](https://aka.ms/threatmodelingtool).