---
title: 'Azure Security Benchmark v2: gobernanza y estrategia'
description: Gobernanza y estrategia de Azure Security Benchmark v2
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/20/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 420ab87591e230592ec8f728c6acb155266b5912
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91403533"
---
# <a name="security-control-v2-governance-and-strategy"></a>Control de seguridad V2: Gobernanza y estrategia

La gobernanza y la estrategia proporcionan una guía para garantizar una estrategia de seguridad coherente y un enfoque de gobierno documentado para guiar y mantener el control de la seguridad, incluido el establecimiento de roles y responsabilidades para las diferentes funciones de seguridad en la nube, estrategia técnica unificada y directivas y estándares de soporte.

## <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>GS-1: Definición de la estrategia de protección de datos y administración de recursos

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| GS-1 | 2, 13 | SC, AC |

Asegúrese de documentar y comunicar una estrategia clara para la protección y supervisión continua de sistemas y datos. Dé prioridad a la detección, evaluación, protección y supervisión de los sistemas y datos críticos para la empresa. 

Esta estrategia debe incluir instrucciones, directivas y estándares documentados para los siguientes elementos: 

- Norma de clasificación de datos de acuerdo con los riesgos empresariales

- Visibilidad en la organización de seguridad de los riesgos y el inventario de recursos 

- Aprobación de la organización de seguridad de los servicios de Azure para su uso 

- Seguridad de los recursos durante su ciclo de vida

- Estrategia de control de acceso necesaria según la clasificación de datos de la organización

- Uso de las funcionalidades de protección de datos nativa de Azure y de terceros

- Requisitos de cifrado de datos para casos de uso en tránsito y en reposo

- Normas criptográficas adecuadas

Para más información, consulte las siguientes referencias:
- [Recomendación para la arquitectura de seguridad de Azure: almacenamiento, datos y cifrado](https://docs.microsoft.com/azure/architecture/framework/security/storage-data-encryption?toc=/security/compass/toc.json&amp;bc=/security/compass/breadcrumb/toc.json)

- [Aspectos básicos de la seguridad de Azure: seguridad, cifrado y almacenamiento de datos de Azure](../fundamentals/encryption-overview.md)

- [Cloud Adoption Framework: procedimientos recomendados de cifrado y seguridad de los datos de Azure](https://docs.microsoft.com/azure/security/fundamentals/data-encryption-best-practices?toc=/azure/cloud-adoption-framework/toc.json&amp;bc=/azure/cloud-adoption-framework/_bread/toc.json)

- [Azure Security Benchmark: administración de recursos](security-controls-v2-asset-management.md)

- [Azure Security Benchmark: protección de datos](security-controls-v2-data-protection.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Todas las partes interesadas](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-2-define-enterprise-segmentation-strategy"></a>GS-2: Definición de una estrategia de segmentación empresarial

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| GS-2 | 4, 9, 16 | AC, CA, SC |

Establezca en toda la empresa una estrategia para segmentar el acceso a los recursos mediante una combinación de identidad, red, aplicación, suscripción, grupo de administración y otros controles.

Equilibre concienzudamente la necesidad de separación de seguridad con la necesidad de habilitar el funcionamiento diario de los sistemas que necesitan comunicarse entre sí y acceder a los datos.

Asegúrese de que la estrategia de segmentación se implementa de forma coherente en todos los tipos de control, como la seguridad de red, los modelos de identidad y acceso, y los modelos de acceso y permisos de las aplicaciones, y los controles de los procesos humanos.

- [Guía de la estrategia de segmentación en Azure (vídeo)](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Guía de la estrategia de segmentación en Azure (documento)](/security/compass/governance#enterprise-segmentation-strategy)

- [Alineación de la segmentación de red con la estrategia de segmentación empresarial](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Todas las partes interesadas](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-3-define-security-posture-management-strategy"></a>GS-3: Definición de la estrategia de administración de la posición de seguridad

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| GS-3 | 20, 3, 5 | RA, CM, SC |

Mida y mitigue continuamente los riesgos de los recursos individuales y el entorno en el que se hospedan. Dé prioridad a los recursos de gran valor y a las superficies de ataque muy expuestas, como las aplicaciones publicadas, los puntos de entrada y salida de red, los puntos de conexión de usuario y administrador, etc.

- [Azure Security Benchmark: administración de posturas y vulnerabilidades](security-controls-v2-posture-vulnerability-management.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Todas las partes interesadas](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>GS-4: Alineación de los roles y responsabilidades de la organización

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| GS-4 | N/D | PL, PM |

Asegúrese de documentar y comunicar una estrategia clara para los roles y las responsabilidades de la organización de seguridad. Dé prioridad a ofrecer una responsabilidad clara en las decisiones de seguridad, a proporcionar a todos los usuarios formación sobre el modelo de responsabilidad compartida y a los equipos técnicos sobre la tecnología para proteger la nube.

- [Procedimiento recomendado de seguridad de Azure 1. Personas: Formación del equipo sobre el recorrido de seguridad de la nube](https://aka.ms/AzSec1)

- [Procedimiento recomendado de seguridad de Azure 2. Personas: Formación del equipo en tecnología de seguridad de la nube](https://aka.ms/AzSec2)

- [Procedimiento recomendado de seguridad de Azure 3. Proceso: Asignación de responsabilidades por las decisiones de seguridad en la nube](https://aka.ms/AzSec3)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Todas las partes interesadas](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-5-define-network-security-strategy"></a>GS-5: Definición de la estrategia de seguridad de red

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| GS-5 | 9 | CA, SC |

Establezca un enfoque de seguridad de red de Azure como parte de la estrategia de control de acceso de seguridad general de su organización.  

Esta estrategia debe incluir instrucciones, directivas y estándares documentados para los siguientes elementos: 

- Centralización de la responsabilidad de seguridad y la administración de redes

- Modelo de segmentación de la red virtual alineado con la estrategia de segmentación empresarial

- Estrategia de corrección en diferentes escenarios de amenazas y ataques

- Estrategia de entrada y salida y perimetral de Internet

- Estrategia de interconectividad entre el entorno local y la nube híbrida

- Artefactos de seguridad de red actualizados (por ejemplo, diagramas de red y arquitectura de red de referencia)

Para más información, consulte las siguientes referencias:

- [Procedimiento recomendado de seguridad de Azure 11. Arquitectura: Establecimiento de una estrategia de seguridad unificada](https://aka.ms/AzSec11)

- [Azure Security Benchmark: seguridad de red](security-controls-v2-network-security.md)

- [Azure Network Security Overview (Información general sobre Azure Network Security)](../fundamentals/network-overview.md)

- [Estrategia para la arquitectura de red empresarial](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Todas las partes interesadas](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-6-define-identity-and-privileged-access-strategy"></a>GS-6: Definición de la estrategia de acceso con privilegios e identidades

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| GS-6 | 16, 4 | AC, AU, SC |

Establezca una identidad de Azure y enfoques de acceso con privilegios como parte de la estrategia de control de acceso de seguridad general de su organización.  

Esta estrategia debe incluir instrucciones, directivas y estándares documentados para los siguientes elementos: 

- Un sistema de identidad y autenticación centralizado y su interconectividad con otros sistemas de identidad internos y externos

- Métodos de autenticación sólida en diferentes casos de uso y condiciones

- Protección de usuarios con privilegios elevados

- Supervisión y control de anomalías en las actividades de los usuarios  

- Proceso de revisión y conciliación del acceso y la identidad de los usuarios

Para más información, consulte las siguientes referencias:

- [Azure Security Benchmark: administración de identidades](security-controls-v2-identity-management.md)

- [Azure Security Benchmark: acceso con privilegios](security-controls-v2-privileged-access.md)

- [Procedimiento recomendado de seguridad de Azure 11. Arquitectura: Establecimiento de una estrategia de seguridad unificada](https://aka.ms/AzSec11)

- [Información general sobre seguridad de administración de identidades de Azure](../fundamentals/identity-management-overview.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Todas las partes interesadas](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-7-define-logging-and-threat-response-strategy"></a>GS-7: Definición de la estrategia de registro y respuesta a amenazas

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| GS-7 | 19 | IR, AU, RA, SC |

Establezca una estrategia de registro y respuesta a amenazas para detectar y corregir rápidamente las amenazas mientras cumple los requisitos de cumplimiento. Dé prioridad a ofrecer a los analistas alertas de alta calidad y experiencias fluidas para que puedan centrarse en las amenazas, en lugar de en la integración y los pasos manuales. 

Esta estrategia debe incluir instrucciones, directivas y estándares documentados para los siguientes elementos: 

- Las responsabilidades y el rol de la organización en las operaciones de seguridad (SecOps) 

- Un proceso de respuesta a incidentes bien definido que esté alineado con NIST u otro marco del sector. 

- Captura y retención de registros para admitir la detección de amenazas, la respuesta ante incidentes y las necesidades de cumplimiento

- Visibilidad y correlación centralizada de la información sobre amenazas, mediante SIEM, funcionalidades nativas de Azure y otros orígenes 

- Plan de comunicación y notificación con sus clientes, proveedores y entidades públicas de interés

- Uso de plataformas nativas de Azure y de terceros para el tratamiento de incidentes, como el registro y la detección de amenazas, los análisis forenses y la corrección y erradicación de ataques

- Procesos para controlar incidentes y actividades posteriores a incidentes, como las lecciones aprendidas y la retención de pruebas

Para más información, consulte las siguientes referencias:
- [Azure Security Benchmark: registro y detección de amenazas](security-controls-v2-logging-threat-detection.md)

- [Azure Security Benchmark: respuesta a incidentes](security-controls-v2-incident-response.md)

- [Procedimiento recomendado de seguridad de Azure 4. Proceso: Actualización de los procesos de respuesta a incidentes para la nube](https://aka.ms/AzSec4)

- [Guía de decisiones sobre registros e informes en Azure Adoption Framework](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

- [Escala, administración y supervisión empresarial de Azure](/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Todas las partes interesadas](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-8-define-backup-and-recovery-strategy"></a>GS-8: Definición de la estrategia de copia de seguridad y recuperación

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| GS-8 | 10 | CP |

Establezca una estrategia de copia de seguridad y recuperación de Azure para su organización. 

Esta estrategia debe incluir instrucciones, directivas y estándares documentados para los siguientes elementos: 

- Definiciones de objetivo de tiempo de recuperación (RTO) y objetivo de punto de recuperación (RPO) de acuerdo con los objetivos de resistencia de su negocio

- Diseño de redundancia en la configuración de la infraestructura y las aplicaciones

- Protección de la copia de seguridad mediante el control de acceso y el cifrado de datos

Para más información, consulte las siguientes referencias:
- [Azure Security Benchmark: copia de seguridad y recuperación](security-controls-v2-backup-recovery.md)

- [Marco de buena arquitectura de Azure: copia de seguridad y recuperación ante desastres para aplicaciones de Azure](/azure/architecture/framework/resiliency/backup-and-recovery)

- [Azure Adoption Framework: continuidad empresarial y recuperación ante desastres](/azure/cloud-adoption-framework/ready/enterprise-scale/business-continuity-and-disaster-recovery)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente** ([Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Todas las partes interesadas](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

