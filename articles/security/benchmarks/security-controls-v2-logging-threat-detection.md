---
title: 'Azure Security Benchmark v2: registro y detección de amenazas'
description: 'Azure Security Benchmark v2: registro y detección de amenazas'
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/13/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 24ddca37a729a459382fc96a2f6e0ef1a7832f37
ms.sourcegitcommit: 94c750edd4d755d6ecee50ac977328098a277479
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2020
ms.locfileid: "90059093"
---
# <a name="security-control-logging-and-threat-detection"></a>Control de seguridad: registro y detección de amenazas

El registro y la detección de amenazas cubre los controles para detectar amenazas en Azure y habilitar, recopilar y almacenar registros de auditoría de los servicios de Azure. Esto incluye la habilitación de procesos de detección, investigación y corrección con controles para generar alertas de alta calidad con la detección de amenazas nativa en los servicios de Azure, así como la recopilación de registros con Azure Monitor, la centralización del análisis de seguridad con Azure Sentinel, la sincronización de la hora y la retención de registros. 

## <a name="lt-1-enable-threat-detection-for-azure-resources"></a>LT-1: Habilitación de la detección de amenazas para recursos de Azure

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| LT-1 | 6.7 | AU-3, AU-6, AU-12, SI-4 |

Asegúrese de que está supervisando distintos tipos de recursos de Azure para detectar posibles amenazas y anomalías. Céntrese en obtener alertas de alta calidad para reducir los falsos positivos que deben revisar los analistas. Las alertas se pueden crear a partir de datos de registro, agentes u otros datos.

Use la funcionalidad integrada de detección de amenazas de Azure Security Center, que se basa en la supervisión de los datos de telemetría de los servicios de Azure y en el análisis de los registros de servicio. Los datos se recopilan con el agente de Log Analytics, que lee distintas configuraciones relacionadas con la seguridad y diversos registros de eventos del sistema y copia los datos en el área de trabajo para analizarlos. 

Además, use Azure Sentinel para compilar reglas de análisis, que capturan las amenazas que cumplen criterios específicos en su entorno. Las reglas generan incidentes cuando los criterios coinciden, por lo que puede investigar cada incidente. Azure Sentinel también puede importar inteligencia contra amenazas de terceros para mejorar su capacidad de detección de amenazas. 

- [Protección contra amenazas en Azure Security Center](../../security-center/threat-protection.md)

- [Guía de referencia de alertas de seguridad de Azure Security Center](../../security-center/alerts-reference.md)

- [Creación de reglas de análisis personalizadas para detectar amenazas](../../sentinel/tutorial-detect-threats-custom.md)

- [Inteligencia sobre amenazas cibernéticas con Azure Sentinel](/azure/architecture/example-scenario/data/sentinel-threat-intelligence)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente**:

- [Infraestructura y seguridad de los puntos de conexión](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Operaciones de seguridad (SecOps)](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Administración de la posición](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)   

- [Seguridad de las aplicaciones y DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

- [Información sobre amenazas](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

## <a name="lt-2-enable-threat-detection-for-azure-identity-and-access-management"></a>LT-2: Habilitación de la detección de amenazas para la administración de identidades y acceso de Azure

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| LT-2 | 6,8 | AU-3, AU-6, AU-12, SI-4 |

Azure AD proporciona los siguientes registros de usuario que se pueden ver en los informes de Azure AD o integrados con Azure Monitor, Azure Sentinel u otras herramientas de SIEM/supervisión para casos de uso de supervisión y análisis más sofisticados: 
-   Inicios de sesión: el informe de inicios de sesión proporciona información sobre el uso de aplicaciones administradas y actividades de inicio de sesión de usuario.

-   Los registros de auditoría proporcionan rastreabilidad mediante los registros de todos los cambios realizados por diversas características de Azure AD. Algunos ejemplos de registros de auditoría incluyen los cambios realizados en cualquier recurso de Azure AD como agregar o quitar usuarios, aplicaciones, grupos, roles y directivas.

-   Inicios de sesión de riesgo: un inicio de sesión de riesgo es un indicador de un intento de inicio de sesión que puede haber realizado alguien que no es el propietario legítimo de una cuenta de usuario.

-   Usuarios marcados en riesgo: un usuario en riesgo es un indicador de una cuenta de usuario que puede haber estado en peligro.

Azure Security Center también puede alertar sobre determinadas actividades sospechosas, como un número excesivo de intentos de autenticación incorrectos y cuentas en desuso en la suscripción. Además de la supervisión básica de la protección de seguridad, el módulo de protección contra amenazas de Azure Security Center también puede recopilar alertas de seguridad más detalladas de recursos individuales de proceso de Azure (como máquinas virtuales, contenedores, App Service), recursos de datos (como base de datos SQL y almacenamiento) y niveles de servicio de Azure. Esta funcionalidad permite ver anomalías de las cuentas dentro de los recursos individuales.

- [Informes de actividad de auditoría en Azure AD](../../active-directory/reports-monitoring/concept-audit-logs.md)

- [Habilitación de Azure Identity Protection](../../active-directory/identity-protection/overview-identity-protection.md)

- [Protección contra amenazas en Azure Security Center](../../security-center/threat-protection.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente**:

- [Infraestructura y seguridad de los puntos de conexión](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Operaciones de seguridad (SecOps)](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Administración de la posición](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)   

- [Seguridad de las aplicaciones y DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

- [Información sobre amenazas](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

## <a name="lt-3-enable-logging-for-azure-network-activities"></a>LT-3: Habilitación del registro para las actividades de red de Azure

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| LT-3 | 9.3, 12.2, 12.5, 12.8 | AU-3, AU-6, AU-12, SI-4 |

Habilite y recopile registros de recursos de grupos de seguridad de red (NSG), registros de flujo de NSG, registros de Azure Firewall y registros de Web Application Firewall (WAF) para un análisis de seguridad que favorezca la investigación de incidentes, la búsqueda de amenazas y la generación de alertas de seguridad. Puede enviar los registros de flujo a un área de trabajo de Azure Monitor Log Analytics y, a continuación, usar Análisis de tráfico para conseguir información detallada. Asegúrese de recopilar registros de consultas de DNS para ayudar a correlacionar otros datos de red.

- [Habilitación de los registros de flujo de grupos de seguridad de red](../../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Métricas y registros de Azure Firewall](../../firewall/logs-and-metrics.md)

- [Habilitación y uso del Análisis de tráfico](../../network-watcher/traffic-analytics.md)

- [Supervisión con Network Watcher](../../network-watcher/network-watcher-monitoring-overview.md)

- [Soluciones de supervisión de redes de Azure en Azure Monitor](../../azure-monitor/insights/azure-networking-analytics.md)

- [Recopilación de información sobre la infraestructura de DNS con la solución DNS Analytics](../../azure-monitor/insights/dns-analytics.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente**:

- [Infraestructura y seguridad de los puntos de conexión](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Operaciones de seguridad (SecOps)](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Administración de la posición](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)   

- [Seguridad de las aplicaciones y DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

- [Información sobre amenazas](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

## <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4: Habilitación del registro para recursos de Azure

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| LT-4 | 6.2, 6.3, 8.8 | AU-3, AU-12 |

Habilite el registro de los recursos de Azure para satisfacer los requisitos de cumplimiento, detección e investigación de amenazas, y búsqueda de amenazas. 

Puede usar Azure Security Center y Azure Policy para habilitar la recopilación de registros de recursos y datos de registro sobre los recursos de Azure para el acceso a registros de auditoría, seguridad y recursos. Los registros de actividad, que están disponibles automáticamente, incluyen el origen del evento, la fecha, el usuario, la marca de tiempo, las direcciones de origen y de destino, y otros elementos útiles. 

- [Descripción del registro y de los distintos tipos de registro de Azure](../../azure-monitor/platform/platform-logs-overview.md)

- [Descripción de la recopilación de datos de Azure Security Center](../../security-center/security-center-enable-data-collection.md)

- [Habilitación y configuración de la supervisión antimalware](../fundamentals/antimalware.md#enable-and-configure-antimalware-monitoring-using-powershell-cmdlets)

**Responsabilidad**: Compartido

**Partes interesadas de seguridad del cliente**:

- [Operaciones de seguridad (SecOps)](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

Infraestructura y seguridad de los puntos de conexión 

- [Seguridad de las aplicaciones y DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

- [Información sobre amenazas](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

## <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5: Centralizar la administración y el análisis de los registros de seguridad

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| LT-5 | 6.5, 6.6 | AU-3, SI-4 |

Centralice el almacenamiento y el análisis de los registros para permitir su correlación. Asegúrese de que, para cada origen de registro, asigna un propietario de datos, una guía de acceso y una ubicación de almacenamiento; que determinar qué herramientas se usan para procesar y acceder a los datos, y los requisitos de retención de datos.

Asegúrese también de que integra los registros de actividad de Azure en el registro central. Recopile registros mediante Azure Monitor para agregar datos de seguridad generados por dispositivos de punto de conexión, recursos de red y otros sistemas de seguridad. En Azure Monitor, use áreas de trabajo de Log Analytics para realizar consultas y análisis, y use cuentas de Azure Storage para el almacenamiento de archivos a largo plazo.

Además, habilite e incorpore los datos a Azure Sentinel o a un sistema SIEM de terceros.

Muchas organizaciones optan por usar Azure Sentinel para los datos de acceso frecuente y Azure Storage para los datos inactivos que se usan con menos frecuencia. 

- [Recopilación de registros y métricas de plataforma con Azure Monitor](../../azure-monitor/platform/diagnostic-settings.md)

- [Incorporación de Azure Sentinel](../../sentinel/quickstart-onboard.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente**:

- [Arquitectura de seguridad](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Seguridad de las aplicaciones y DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

- [Infraestructura y seguridad de los puntos de conexión](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

## <a name="lt-6-configure-log-storage-retention"></a>LT-6: Configuración de la retención del almacenamiento de registros

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| LT-6 | 6.4 | AU-3, AU-11 |

Configure la retención de registros según sus requisitos de cumplimiento y normativos, entre otros. 

En Azure Monitor, puede establecer el período de retención del área de trabajo de Log Analytics de acuerdo con la normativa de cumplimiento de su organización. Use cuentas de área de trabajo de Azure Storage, Data Lake o Log Analytics para el almacenamiento de archivo a largo plazo.

- [Cambio del período de retención de datos en Log Analytics](../../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

Configuración de la directiva de retención para los registros de cuenta de Azure Storage: https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account#configure-logging

- [Exportación de alertas y recomendaciones de Azure Security Center](../../security-center/continuous-export.md)

**Responsabilidad**: Customer

**Partes interesadas de seguridad del cliente**:

- [Arquitectura de seguridad](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Seguridad de las aplicaciones y DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

- [Operaciones de seguridad (SecOps)](/azure/cloud-adoption-framework/organize/cloud-security-operations-center) 

- [Administración del cumplimiento de la seguridad](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="lt-7-use-approved-time-synchronization-sources"></a>LT-7: Uso de orígenes de sincronización de hora aprobados

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| LT-7 | 6.1 | AU-8 |

Microsoft mantiene los orígenes de hora de la mayoría de los servicios de PaaS y SaaS de Azure. En el caso de las máquinas virtuales, use el servidor NTP predeterminado de Microsoft para la sincronización de hora, a menos que tenga un requisito específico.  Si necesita instalar su propio servidor NTP (protocolo de hora de la red), asegúrese de proteger el puerto 123 del servicio UDP.

Todos los registros generados por los recursos de Azure proporcionan marcas de tiempo con la zona horaria especificada de forma predeterminada.

- [Configuración de la sincronización de hora de los recursos de proceso de Windows de Azure](../../virtual-machines/windows/time-sync.md)

- [Configuración de la sincronización de hora de los recursos de proceso de Linux de Azure](../../virtual-machines/linux/time-sync.md)

- [Procedimiento para deshabilitar UDP entrante para los servicios de Azure](https://support.microsoft.com/help/4558520/how-to-disable-inbound-udp-for-azure-services)

**Responsabilidad**: Compartido

**Partes interesadas de seguridad del cliente**:

- [Directivas y estándares](/azure/cloud-adoption-framework/organize/cloud-security-policy-standards)

- [Seguridad de las aplicaciones y DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

- [Infraestructura y seguridad de los puntos de conexión](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

