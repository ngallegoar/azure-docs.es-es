---
title: Información general sobre Azure Security Benchmark (V2)
description: Información general sobre Azure Security Benchmark V2
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 09/11/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 2be5df88a665c800f55f773b2470cc095fa27b2b
ms.sourcegitcommit: 94c750edd4d755d6ecee50ac977328098a277479
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2020
ms.locfileid: "90058689"
---
# <a name="overview-of-the-azure-security-benchmark"></a>Información general sobre la prueba comparativa de seguridad de Azure

La prueba comparativa de seguridad de Azure (ASB) proporciona recomendaciones y procedimientos recomendados para ayudar a mejorar la seguridad de las cargas de trabajo, los datos y los servicios de Azure.

Esta prueba comparativa forma parte de un conjunto de instrucciones de seguridad holísticas que también incluyen:

- **Cloud Adoption Framework**: orientación sobre la seguridad, incluida la [estrategia](/azure/cloud-adoption-framework/strategy/define-security-strategy), [roles y responsabilidades](/azure/cloud-adoption-framework/organize/cloud-security), [los diez principales procedimientos recomendados de seguridad de Azure](/azure/cloud-adoption-framework/get-started/security#step-1-establish-essential-security-practices) e [implementación de referencia](/azure/cloud-adoption-framework/ready/enterprise-scale/).
- **Marco de buena arquitectura de Azure**: instrucciones sobre [protección de las cargas de trabajo](https://docs.microsoft.com/assessments/?mode=pre-assessment&session=local) en Azure.
- **Procedimientos recomendados de seguridad de Microsoft**: [recomendaciones](/security/compass/microsoft-security-compass-introduction) con ejemplos de Azure.
 La prueba comparativa de seguridad de Azure se enfoca en las áreas de control centradas en la nube. Estos controles son coherentes con las pruebas comparativas de seguridad conocidas, como las descritas por los controles de Center for Internet Security (CIS) versión 7.1 y de National Institute of Standards and Technology (NIST) SP800-53.
Los siguientes controles se incluyen en la prueba comparativa de seguridad de Azure:

| Dominios de control de ASB | Descripción 
|--|--|
| [Seguridad de red (NS)](security-controls-v2-network-security.md) | La seguridad de red cubre los controles para proteger y asegurar las redes de Azure, incluida la protección de redes virtuales, el establecimiento de conexiones privadas, la prevención y la mitigación de ataques externos, y la protección de DNS. |
| [Administración de identidades (IM)](security-controls-v2-identity-management.md) | La administración de identidades cubre controles para establecer una identidad segura y controles de acceso mediante Azure Active Directory, incluido el uso del inicio de sesión único, las autenticaciones sólidas, las identidades administradas (y las entidades de servicio) para las aplicaciones, el acceso condicional y la supervisión de las anomalías de cuenta. |
| [Acceso con privilegios (PA)](security-controls-v2-privileged-access.md) | El acceso con privilegios cubre los controles para proteger el acceso con privilegios a los recursos y al inquilino de Azure, e incluye una serie de controles para proteger el modelo administrativo, las cuentas administrativas y las estaciones de trabajo de acceso con privilegios frente a riesgos deliberados e involuntarios. |
| [Protección de datos (DP)](security-controls-v2-data-protection.md) | La protección de datos cubre el control de la protección de datos en reposo, en tránsito y a través de mecanismos de acceso autorizados, incluido la detección, clasificación, protección y supervisión de los recursos de datos confidenciales mediante el control de acceso, el cifrado y el registro en Azure. |
| [Administración de recursos (AM)](security-controls-v2-asset-management.md) | La administración de recursos cubre controles para garantizar la visibilidad y el control de la seguridad de los recursos de Azure, incluidas recomendaciones sobre permisos para el personal de seguridad, acceso de seguridad al inventario de activos y administración de aprobaciones de servicios y recursos (inventario, seguimiento y corrección). |
| [Registro y detección de amenazas (LT)](security-controls-v2-logging-threat-detection.md) | El registro y la detección de amenazas cubre los controles para detectar amenazas en Azure y habilitar, recopilar y almacenar registros de auditoría para servicios de Azure, incluida la habilitación de procesos de detección, investigación y corrección con controles para generar alertas de alta calidad con la detección de amenazas nativa en los servicios de Azure. También incluye la recopilación de registros con Azure Monitor, la centralización del análisis de seguridad con Azure Sentinel, la sincronización de la hora y la retención de registros. |
| [Respuesta a los incidentes (IR)](security-controls-v2-incident-response.md) | La respuesta a los incidentes cubre los controles del ciclo de vida de respuesta a los incidentes: preparación, detección y análisis, contención y actividades posteriores a incidentes, incluido el uso de servicios de Azure como Azure Security Center y Sentinel para automatizar el proceso de respuesta a incidentes. |
| [Administración de posiciones y vulnerabilidades (PV)](security-controls-v2-posture-vulnerability-management.md) | La administración de posiciones y vulnerabilidades se centra en los controles para evaluar y mejorar la posición de seguridad de Azure, incluido el examen de vulnerabilidades, las pruebas de penetración y la corrección, así como el seguimiento de la configuración de seguridad, los informes y la corrección en los recursos de Azure. |
| [Seguridad del punto de conexión (ES)](security-controls-v2-endpoint-security.md) | La seguridad del punto de conexión cubre los controles de detección y respuesta de puntos de conexión, incluido el uso de la detección de puntos de conexión y la respuesta (EDR), así como el servicio antimalware para los puntos de conexión en entornos de Azure. |
| [Copia de seguridad y recuperación (BR)](security-controls-v2-backup-recovery.md) | La copia de seguridad y la recuperación conllevan controles para garantizar que las copias de seguridad de los datos y la configuración de los distintos niveles de servicio se realizan, se validan y se protegen. |
| [Gobernanza y estrategia (GS)](security-controls-v2-governance-strategy.md) | La gobernanza y la estrategia proporcionan una guía para garantizar una estrategia de seguridad coherente y un enfoque de gobierno documentado para guiar y mantener el control de la seguridad, incluido el establecimiento de roles y responsabilidades para las diferentes funciones de seguridad en la nube, estrategia técnica unificada y directivas y estándares de soporte. |

También puede descargar las pruebas comparativas de seguridad de Azure en [formato de hoja de cálculo](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Security%20Benchmark).

## <a name="azure-security-benchmark-recommendations"></a>Recomendaciones de pruebas comparativas de seguridad de Azure

Cada recomendación incluye la siguiente información:

- **Identificador de Azure**: El identificador de la prueba comparativa de seguridad de Azure que corresponde a la recomendación.
- **Identificadores de CIS Controls v7.1**: Controles de CIS v7.1 que corresponden a esta recomendación.
- **Identificadores de NIST SP800-53 r4**: Controles NIST SP800-53 R4 (moderado) que corresponden a esta recomendación.
- **Detalles**: La lógica de la recomendación y vínculos a instrucciones sobre cómo implementarla. Si la recomendación es compatible con Azure Security Center, esta información también se mostrará.
- **Responsabilidad**: Si el cliente, el proveedor de servicios, o ambos, son los responsables de implementar esta recomendación. Las responsabilidades de seguridad se comparten en la nube pública. Algunos controles de seguridad solo están disponibles para el proveedor de servicios en la nube y, por tanto, el proveedor es responsable de dirigirlos. Estas son las observaciones generales: para algunos servicios individuales, la responsabilidad será distinta de la que se muestra en la prueba comparativa de seguridad de Azure. Estas diferencias se describen en las recomendaciones de línea de base para el servicio individual.
- **Partes interesadas de seguridad del cliente**: Las funciones de seguridad de la organización del cliente que pueden ser responsables, comprometidas o consultadas para el control correspondiente. Puede ser diferente entre las organizaciones en función de la estructura de la organización de seguridad de la empresa, así como de los roles y las responsabilidades que configure relacionados con la seguridad de Azure.

> [!NOTE]
> Las asignaciones de control entre ASB y las pruebas comparativas del sector (como NIST y CIS) solo indican que una característica específica de Azure se puede usar para resolver de forma completa o parcial un requisito de control definido en NIST o CIS. Debe tener en cuenta que dicha implementación no se traduce necesariamente en el cumplimiento completo del control correspondiente en CIS o NIST.

Le agradecemos sus comentarios detallados y su participación activa en el trabajo de pruebas comparativas de seguridad de Azure. Si desea proporcionar la entrada directa del equipo de pruebas comparativas de seguridad de Azure, rellene el formulario en https://aka.ms/AzSecBenchmark.


- Vea el primer control de seguridad: [Seguridad de las redes](security-control-network-security.md)
- Lea [Introducción a las pruebas comparativas de seguridad de Azure](introduction.md)
- Descargue la [hoja de cálculo de Excel de las pruebas comparativas de seguridad de Azure v2](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Security%20Benchmark)