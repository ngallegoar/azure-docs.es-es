---
title: 'Preguntas frecuentes sobre Azure Security Center: preguntas sobre máquinas virtuales'
description: Preguntas más frecuentes sobre máquinas virtuales en Azure Security Center, un producto que ayuda a evitar, detectar y responder a amenazas.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/25/2020
ms.author: memildin
ms.openlocfilehash: 1d91f8242e31aa1efdf9301101307d5bad39a459
ms.sourcegitcommit: f88074c00f13bcb52eaa5416c61adc1259826ce7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92342219"
---
# <a name="faq---questions-about-virtual-machines"></a>Preguntas frecuentes: preguntas sobre máquinas virtuales


## <a name="what-types-of-virtual-machines-are-supported"></a>¿Qué tipos de máquinas virtuales se admiten?

La funcionalidad de supervisión y las recomendaciones están disponibles para las máquinas virtuales creadas con los [modelos de implementación clásica y de Resource Manager](/azure/security-center/./azure-resource-manager/management/deployment-models).

Consulte [Plataformas compatibles con Azure Security Center](security-center-os-coverage.md) para ver una lista de plataformas compatibles.


## <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>¿Por qué Azure Security Center no reconoce la solución antimalware que se ejecuta en mi máquina virtual de Azure?

Azure Security Center solo tiene visibilidad del antimalware instalado mediante extensiones de Azure. Por ejemplo, Security Center no puede detectar antimalware instalado previamente en una imagen que haya proporcionado o si instaló este tipo de herramienta en máquinas virtuales con sus propios procesos (como sistemas de administración de configuración).


## <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>¿Por qué recibo el mensaje Faltan los datos de detección en mi máquina virtual?

Este mensaje aparece cuando no hay datos de examen de una máquina virtual. Los datos de detección pueden tardar algún tiempo (menos de una hora) en rellenarse después de habilitar la recolección de datos en Azure Security Center. Tras el rellenado inicial de los datos de examen, se puede recibir este mensaje porque no hay ningún dato de examen o porque no hay ninguno que sea reciente. Estos análisis no se rellenan para las máquinas virtuales que estén detenidas. Este mensaje también podría aparecer si no se han rellenado datos de examen recientemente (de acuerdo con la directiva de retención del agente de Windows, que tiene un valor predeterminado de 30 días).


## <a name="how-often-does-security-center-scan-for-operating-system-vulnerabilities-system-updates-and-endpoint-protection-issues"></a>¿Con qué frecuencia Security Center analiza las vulnerabilidades del sistema operativo, las actualizaciones del sistema y los problemas de Endpoint Protection?

A continuación se muestran los tiempos de latencia para los exámenes de vulnerabilidades, actualizaciones y problemas de Security Center:

- Configuraciones de seguridad del sistema operativo: los datos se actualizan en un plazo de 48 horas
- Actualizaciones del sistema: los datos se actualizan en un plazo de 24 horas.
- Problemas de Endpoint Protection: los datos se actualizan en un plazo de 8 horas.

Normalmente, Security Center busca nuevos datos cada hora y actualiza las recomendaciones en consecuencia. 

> [!NOTE]
> Security Center usa el agente de Log Analytics para recopilar y almacenar datos. Para obtener más información, consulte [Migración de la plataforma de Azure Security Center](./security-center-enable-data-collection.md).


## <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>¿Por qué recibo el mensaje Falta el agente de máquina virtual?

El agente de VM debe instalarse en VM para habilitar la recopilación de datos. De manera predeterminada, el agente de máquina virtual está instalado en las máquinas virtuales que se implementan desde Azure Marketplace. La entrada de blog sobre [las extensiones y el agente de máquina virtual](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/)proporciona información sobre cómo instalar el Agente de máquina virtual.