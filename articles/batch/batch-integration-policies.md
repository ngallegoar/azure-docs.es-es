---
title: 'Integración con directivas de Azure: Azure Batch | Microsoft Docs'
description: ''
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 02/24/2020
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 9a306457f838fc79d12be3217d96cc8fb25c9c1b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617868"
---
# <a name="integration-with-azure-policy"></a>Integración con Azure Policy

Azure Policy es un servicio de Azure que puede usar para crear, asignar y administrar directivas que aplican reglas a los recursos para asegurarse de que estos sigan siendo compatibles con los estándares corporativos y los contratos de nivel de servicio. Azure Policy evalúa los recursos que incumplen las directivas asignadas. 

Azure Batch tiene dos extensiones integradas para ayudarle a administrar el cumplimiento de directivas. 

|**Nombre**...|   **Descripción**|**Efectos**|  **Versión**|    **Origen**
|----------------|----------|----------|----------------|---------------|
|Se deben habilitar los registros de diagnóstico en las cuentas de Batch|   Permite auditar la habilitación de registros de diagnóstico. Esto le permite volver a crear seguimientos de actividad con fines de investigación en caso de incidente de seguridad o riesgo para la red.|AuditIfNotExists, Disabled|  2.0.0|  GitHub|
|Las reglas de alerta de métrica deben configurarse en las cuentas de Batch| Permite auditar la configuración de reglas de alertas de métricas en una cuenta de Batch para habilitar la métrica requerida.|   AuditIfNotExists, Disabled| 1.0.0|  GitHub|

Las definiciones de directiva describen las condiciones que se deben cumplir. Una condición compara la propiedad del recurso con un valor necesario. Se accede a los campos de propiedades de recursos mediante alias predefinidos. Los alias de propiedades se utilizan para acceder a propiedades específicas de un tipo de recurso. Los alias le permiten restringir los valores o condiciones que se permiten para una propiedad en un recurso. Cada alias se asigna a las rutas de acceso en las diferentes versiones de la API para un tipo de recurso determinado. Durante la evaluación de directivas, el motor de directiva obtiene la ruta de acceso de propiedad para esa versión de la API.

Los recursos que Batch requiere incluyen: cuenta, nodo de proceso, grupo, trabajo y tarea. Por lo tanto, se usarían alias de propiedad para acceder a propiedades específicas de estos recursos. Más información sobre [alias](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#aliases).

Para asegurarse de que conoce los alias actuales y revise los recursos y las directivas, use la extensión de Azure Policy para Visual Studio Code. Se puede instalar en todas las plataformas compatibles con Visual Studio Code. Esta compatibilidad incluye Windows, Linux y macOS. Consulte la [guía de instalación](https://docs.microsoft.com/azure/governance/policy/how-to/extension-for-vscode).



