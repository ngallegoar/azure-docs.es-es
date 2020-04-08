---
title: 'Microsoft Threat Modeling Tool, versión 03/22/2020: Azure'
description: Documentación de las notas de la versión de la herramienta de modelado de amenazas
author: jegeib
ms.author: jegeib
ms.service: security
ms.topic: article
ms.date: 03/22/2020
ms.openlocfilehash: 6fea4d48b62d6ea429d37924ffd15855db6294cc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80137644"
---
# <a name="threat-modeling-tool-update-release-73003161---03222020"></a>Threat Modeling Tool, versión actualizada 7.3.00316.1: 22 de marzo de 2020

El 22 de marzo de 2020 se lanzó la versión 7.3.00316.1 de Microsoft Threat Modeling Tool (TMT), que incluye los cambios siguientes:

- Mejoras de accesibilidad
- Corrección de errores
- Nueva característica DiagramReader

## <a name="notable-bug-fixes"></a>Correcciones de errores importantes

### <a name="exporting-the-threat-list-to-csv"></a>Exportación de la lista de amenazas a CSV

La función de exportación de CSV seleccionaba de forma incoherente los campos de la lista de amenazas que se exportarían. Ahora todos los campos de la lista de amenazas se exportarán al archivo CSV. 

### <a name="ux-bugs"></a>Errores de experiencia del usuario

- Los menús de ayuda del flujo de trabajo principal (crear/abrir/analizar) y la experiencia del editor de plantillas ahora tienen opciones de menú coherentes.
- La barra de búsqueda del panel de galerías de símbolos ahora tiene un cursor estándar y se han agregado etiquetas adecuadas.

## <a name="new-features"></a>Nuevas características

### <a name="diagramreader-feature-has-been-added"></a>Se ha agregado la característica DiagramReader

Se ha agregado una nueva característica DiagramReader en el menú principal mientras un modelo está abierto. Esta característica convertirá la representación gráfica del modelo en una narrativa basada en texto. 

## <a name="system-requirements"></a>Requisitos del sistema

- Sistemas operativos admitidos:
  - [Actualización de aniversario de Microsoft Windows 10](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) o posterior
- Versión de .NET necesaria:
  - [.NET 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262) o posterior
- Requisitos adicionales:
  - Una conexión a Internet para recibir actualizaciones de la herramienta, así como plantillas

## <a name="documentation-and-feedback"></a>Documentación y comentarios

- La documentación de Threat Modeling Tool se encuentra en [docs.microsoft.com](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool) e incluye información [sobre el uso de la herramienta](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-getting-started).

## <a name="next-steps"></a>Pasos siguientes

Descargue la versión más reciente de [Microsoft Threat Modeling Tool](https://aka.ms/threatmodelingtool).
