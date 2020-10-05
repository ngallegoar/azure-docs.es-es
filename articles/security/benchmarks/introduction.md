---
title: Introducción a las pruebas comparativas de seguridad de Azure
description: Introducción a las pruebas comparativas de seguridad
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/16/2019
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 53473b6be9e91adb636c3c9528e97ec644616115
ms.sourcegitcommit: 94c750edd4d755d6ecee50ac977328098a277479
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2020
ms.locfileid: "90058621"
---
# <a name="azure-security-benchmark-introduction"></a>Introducción a las pruebas comparativas de seguridad de Azure

Diariamente se publican nuevos servicios y características en Azure; los desarrolladores publican rápidamente nuevas aplicaciones en la nube basadas en estos servicios y los atacantes buscan siempre nuevas formas de aprovechar los recursos configurados incorrectamente. La nube se mueve rápidamente, los desarrolladores también y los atacantes siempre están en movimiento. ¿Cómo puede mantenerse al día y asegurarse de que sus implementaciones en la nube están seguras? ¿En qué se diferencian los procedimientos de seguridad para sistemas en la nube de los sistemas en el entorno local? ¿Cómo puede supervisar la coherencia en muchos equipos de desarrollo independientes?

Microsoft ha descubierto que el uso de *pruebas comparativas de seguridad* puede ayudarle a proteger rápidamente las implementaciones en la nube. Las recomendaciones para las pruebas comparativas del proveedor de servicios en la nube ofrecen un punto de partida para seleccionar opciones de configuración de seguridad específicas en su entorno y le permiten reducir rápidamente los riesgos para su organización.

Azure Security Benchmark incluye una recopilación de recomendaciones de seguridad de gran impacto que puede usar para ayudar a proteger los servicios que usa en Azure:

- Controles de seguridad: Estas recomendaciones se suelen aplicar tanto en el inquilino de Azure como en los servicios de Azure. Cada recomendación señala una lista de partes interesadas que suelen estar implicadas en el planeamiento, la aprobación o la implementación de la prueba comparativa. 
- Líneas de base del servicio: Estos controles se aplican a los servicios individuales de Azure para ofrecer recomendaciones sobre la configuración de seguridad de dicho servicio.

## <a name="implement-the-azure-security-benchmark"></a>Implementación de Azure Security Benchmark
- **Planifique** la implementación de Azure Security Benchmark mediante la revisión de la [documentación](overview.md) para los controles empresariales y las líneas de base específicas del servicio con el fin de planear el marco de control y cómo se adapta a directrices como las del marco CIS (controles v7.1) y NIST (SP800-53).
- **Supervise** el cumplimiento con el estado de Azure Security Benchmark (y otros conjuntos de controles) mediante [panel de cumplimiento normativo](../../security-center/security-center-compliance-dashboard.md).
- **Establezca límites de protección** para automatizar las configuraciones seguras y exigir el cumplimiento de Azure Security Benchmark (y otros requisitos de su organización) con Azure Blueprints y Azure Policy.
 
Tenga en cuenta que Azure Security Benchmark v2 sigue los [procedimientos recomendados de seguridad de Microsoft](/security/compass/microsoft-security-compass-introduction) (antes Azure Security Compass) para que Azure Security Benchmark proporcione una única vista consolidada de las recomendaciones de seguridad de Microsoft para Azure.

## <a name="common-use-cases"></a>Casos de uso comunes

Azure Security Benchmark se usa con frecuencia para abordar estos desafíos comunes para clientes o socios de servicio que:
- Acaban de comenzar a usar Azure y buscan procedimientos recomendados de seguridad para garantizar una implementación segura.
- Están mejorando la postura de seguridad de sus implementaciones existentes de Azure para clasificar por orden de prioridad los principales riesgos y mitigaciones.
- Están aprobando los servicios de Azure para que los sectores de tecnología y negocios los usen para cumplir con directrices de seguridad específicas.
- Están cumpliendo los requisitos normativos de los clientes que provienen de la administración pública o de sectores altamente regulados, como finanzas y salud (o proveedores de servicios que necesitan crear sistemas para estos clientes). Estos clientes deben asegurarse de que su configuración de Azure cumple con las capacidades de seguridad especificadas en un marco del sector, como CIS, NIST o PCI. Azure Security Benchmark proporciona un enfoque eficaz con los controles ya asignados previamente a estas pruebas comparativas del sector.

## <a name="terminology"></a>Terminología

Los términos "control", "prueba comparativa" y "línea de base" se usan con frecuencia en la documentación de Azure Security Benchmark y es importante comprender cómo usa Azure esos términos.


| Término | Descripción | Ejemplo |
|--|--|--|
| Control | Un control es una descripción de alto nivel de una característica o actividad que debe abordarse y no es específico de una tecnología o implementación. | La protección de datos es uno de los controles de seguridad. Este control contiene acciones específicas que deben abordarse para ayudar a garantizar la protección de datos. |
| Prueba comparativa | Una prueba comparativa contiene recomendaciones de seguridad para una tecnología específica, como Azure. Las recomendaciones se clasifican por el control al que pertenecen. | Azure Security Benchmark incluye las recomendaciones de seguridad específicas de la plataforma Azure. |
| Línea base | Una línea base es la implementación de la prueba comparativa en el servicio individual de Azure. Cada organización decide cuáles recomendaciones de prueba comparativa y configuraciones correspondientes son necesarias en el ámbito de implementación de Azure. | La empresa Contoso busca habilitar las características de seguridad de Azure SQL según la configuración recomendada en la línea de base de seguridad de Azure SQL.

Le agradeceremos sus comentarios sobre la prueba comparativa de seguridad de Azure. Le animamos a que proporcione comentarios en el área de comentarios que aparece a continuación. Si prefiere compartir su opinión de forma más privada con el equipo de pruebas comparativas de seguridad de Azure, debe rellenar el formulario en https://aka.ms/AzSecBenchmark.
