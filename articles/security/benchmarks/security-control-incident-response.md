---
title: 'Control de seguridad de Azure: respuesta a incidentes'
description: Respuesta a incidentes del control de seguridad
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: fb3560aa2d3fbf48ab63c4da4d3a8d69cb677209
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75934412"
---
# <a name="security-control-incident-response"></a>Control de seguridad: Respuesta a los incidentes

Proteja la información de la organización, así como su reputación, mediante el desarrollo y la implementación de una infraestructura de respuesta a incidentes (por ejemplo, planes, roles definidos, formación, comunicaciones, supervisión de la administración) para detectar rápidamente un ataque y, a continuación, contener el daño de manera eficaz, erradicar la presencia del atacante y restaurar la integridad de la red y los sistemas.

## <a name="101-create-an-incident-response-guide"></a>10.1: Creación de una guía de respuesta ante incidentes

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 10.1 | 19.1, 19.2, 19.3 | Customer |

Cree una guía de respuesta a incidentes para su organización. Asegúrese de que haya planes de respuesta a incidentes escritos que definan todos los roles del personal, así como las fases de administración y gestión de los incidentes, desde la detección hasta la revisión posterior a la incidencia.

Configuración de las automatizaciones del flujo de trabajo en Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide

Instrucciones para crear su propio proceso de respuesta a incidentes de seguridad:

https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

Anatomía de un incidente del Centro de respuestas de seguridad de Microsoft:

https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

El cliente también puede utilizar la guía de control de incidentes de seguridad de equipos de NIST como referencia para la creación de su propio plan de respuesta a incidentes:

https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf

## <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Creación de un procedimiento de priorización y puntuación de incidentes

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 10.2 | 19,8 | Customer |

Security Center asigna una gravedad a cada alerta para ayudarle a priorizar aquellas que se deben investigar en primer lugar. La gravedad se basa en la confianza que tiene Security Center en la búsqueda o en el análisis utilizados para emitir la alerta, así como en el nivel de confianza de que ha habido un intento malintencionado detrás de la actividad que ha provocado la alerta.

Además, marque claramente las suscripciones (por ejemplo, producción, no producción) y cree un sistema de nomenclatura para identificar y clasificar claramente los recursos de Azure.

## <a name="103-test-security-response-procedures"></a>10.3: Prueba de los procedimientos de respuesta de seguridad

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 10,3 | 19 | Customer |

Realice ejercicios para probar las capacidades de respuesta a los incidentes de los sistemas con regularidad. Identifique puntos débiles y brechas y revise el plan según sea necesario.

Consulte la publicación de NIST: Guía para probar, entrenar y ejecutar programas para planes y capacidades de TI:

https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

## <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Provisión de detalles de contacto de incidentes de seguridad y configuración de notificaciones de alerta para incidentes de seguridad

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 10,4 | 19.5 | Customer |

La información de contacto del incidente de seguridad la utilizará Microsoft para ponerse en contacto con usted si Microsoft Security Response Center (MSRC) detecta que un tercero no autorizado o ilegal ha accedido a los datos del cliente.  Revise los incidentes después del hecho para asegurarse de que se resuelven los problemas.

Establecimiento del contacto de seguridad de Azure Security Center:

https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details

## <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Incorporación de alertas de seguridad en el sistema de respuesta a incidentes

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 10.5 | 19.6 | Customer |

Exporte sus alertas y recomendaciones de Azure Security Center mediante la característica de exportación continua. La exportación continua le permite exportar alertas y recomendaciones de forma manual o continua. Puede usar el conector de datos de Azure Security Center para transmitir las alertas a Sentinel.

Configuración de la exportación continua:

https://docs.microsoft.com/azure/security-center/continuous-export

Transmisión de alertas a Azure Sentinel:

https://docs.microsoft.com/azure/sentinel/connect-azure-security-center

## <a name="106-automate-the-response-to-security-alerts"></a>10.6: Automatización de la respuesta a las alertas de seguridad

| Id. de Azure | Id. de CIS | Responsabilidad |
|--|--|--|
| 10.6 | 19 | Customer |

Use la característica de automatización del flujo de trabajo de Azure Security Center para desencadenar automáticamente las respuestas a través de &quot;Logic Apps&quot; en las alertas y recomendaciones de seguridad.

Configuración de la automatización de flujo de trabajo y Logic Apps:

https://docs.microsoft.com/azure/security-center/workflow-automation

## <a name="next-steps"></a>Pasos siguientes

Vea el siguiente control de seguridad: [Pruebas de penetración y ejercicios del equipo rojo](security-control-penetration-tests-red-team-exercises.md)