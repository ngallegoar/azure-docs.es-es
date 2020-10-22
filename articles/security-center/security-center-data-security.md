---
title: Seguridad de datos de Azure Security Center | Microsoft Docs
description: En este documento se explica cómo se administran y protegen los datos en Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 33f2c9f4-21aa-4f0c-9e5e-4cd1223e39d7
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2020
ms.author: memildin
ms.openlocfilehash: 49533947ff01aea07eaacd9d761b6414fb672a1c
ms.sourcegitcommit: f88074c00f13bcb52eaa5416c61adc1259826ce7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92339482"
---
# <a name="azure-security-center-data-security"></a>Seguridad de datos de Azure Defender

Para ayudar a los clientes a evitar, detectar y responder a las amenazas, Azure Security Center recopila y procesa datos relacionados con la seguridad, entre los que se incluyen la información de configuración, los metadatos y los registros de eventos, entre otros. Microsoft se adhiere a instrucciones estrictas de seguridad y cumplimiento de normas, desde la codificación hasta la operación de un servicio.

En este artículo se explica cómo se administran y protegen los datos en Defender.

## <a name="data-sources"></a>Orígenes de datos
En Defender se analizan los datos de los orígenes siguientes para proporcionar visibilidad de su estado de seguridad, identificar vulnerabilidades y recomendar mitigaciones y detectar amenazas activas:

- **Servicios de Azure**: utiliza la información acerca de la configuración de los servicios de Azure que se hayan implementado mediante la comunicación con el proveedor de recursos de cada uno de dichos servicios.
- **Tráfico de red**: usa metadatos muestreados del tráfico de red de la infraestructura de Microsoft, como la IP o el puerto de origen o destino, el tamaño de paquete y el protocolo de red.
- **Soluciones de asociados**: usa alertas de seguridad de las soluciones de asociados integradas, como firewalls y soluciones antimalware.
- **Los equipos**: usa los detalles de configuración y la información sobre eventos de seguridad, como los eventos y registros de auditoría de Windows, y los mensajes de Syslog de las máquinas virtuales.


## <a name="data-protection"></a>Protección de los datos

### <a name="data-segregation"></a>Segregación de datos
los datos se mantienen separados de forma lógica en cada componente a lo largo de todo el servicio. Todos los datos se etiquetan por organización. Este etiquetado persiste a lo largo del ciclo de vida de los datos y se aplica en cada nivel del servicio.

### <a name="data-access"></a>Acceso a datos
Para proporcionar recomendaciones de seguridad e investigar las posibles amenazas de seguridad, el personal de Microsoft puede obtener acceso a la información recopilada o analizada por los servicios de Azure, incluidos los eventos de creación de proceso y otros artefactos, que de forma involuntaria pueden incluir datos de cliente o datos personales de sus máquinas. 

Cumplimos el [anexo de protección de datos de Microsoft Online Services](https://www.microsoftvolumelicensing.com/Downloader.aspx?DocumentId=17880), en donde se estipula que Microsoft no utilizará los datos de los clientes ni información derivada de ellos con fines comerciales, publicitarios o similares. Solo usamos los datos del cliente necesarios para proporcionar los servicios de Azure. El usuario conserva todos los derechos sobre los datos del cliente.

### <a name="data-use"></a>Uso de datos
Microsoft utiliza los patrones y la información sobre amenazas vistos en varios inquilinos para mejorar las funcionalidades de detección y prevención; esto se realiza según lo dispuesto en los compromisos de privacidad que se describen en nuestra [declaración de privacidad](https://privacy.microsoft.com/privacystatement).

## <a name="manage-data-collection-from-machines"></a>Administración de recopilación de datos de máquinas
Al habilitar Security Center en Azure, la recopilación de datos se activa para todas las suscripciones de Azure. También se puede habilitar la recopilación de datos para las suscripciones en Defender. Cuando se activa la recopilación de datos, Defender aprovisiona al agente de Log Analytics en todas las máquinas virtuales de Azure compatibles existentes y en las que se crean.

El agente de Log Analytics busca diversos eventos y configuraciones relacionados con la seguridad y los envía a seguimientos de [Seguimiento de eventos para Windows](/windows/win32/etw/event-tracing-portal) (ETW). Además, el sistema operativo generará eventos del registro de eventos mientras la máquina se ejecute. Estos son algunos ejemplos de dichos datos: tipo y versión del sistema operativo, registros del sistema operativo (registros de eventos de Windows), procesos en ejecución, nombre de la máquina, direcciones IP, usuario conectado e identificador de inquilino. El agente de Log Analytics lee las entradas de los registros de eventos y los seguimientos de ETW, y los copia en las áreas de trabajo para el análisis. El agente de Log Analytics también habilita eventos de creación de procesos y la auditoría de línea de comandos.

Si no utiliza Azure Defender, también puede deshabilitar la recopilación de datos de las máquinas virtuales en la directiva de seguridad. La recopilación de datos es necesaria para las suscripciones que están protegidas por Azure Defender. Las instantáneas de disco de máquina virtual y la recolección de artefactos continuará habilitada aunque la recopilación de datos se deshabilite.

Puede especificar el área de trabajo y la región donde se almacenan los datos recopilados de sus máquinas. El valor predeterminado es almacenar los datos recopilados de las máquinas en el área de trabajo más cercana, tal como se muestra en la tabla siguiente:

| Geoárea de la máquina virtual                                      | Geoárea del área de trabajo  |
|---------------------------------------------|----------------|
| Estados Unidos, Brasil, Canadá         | Estados Unidos  |
| Canadá                                      | Canadá         |
| Europa (excepto el Reino Unido)           | Europa         |
| Reino Unido                              | Reino Unido |
| Asia (excepto India, Japón, Corea, China) | Asia Pacífico   |
| Corea                                       | Asia Pacífico   |
| India                                       | India          |
| Japón                                       | Japón          |
| China                                       | China          |
| Australia                                   | Australia      |
|                                             |                |

> [!NOTE]
> **Azure Defender para Storage** almacena los artefactos en regiones según la ubicación del recurso de Azure relacionado. Más información en [Introducción a Azure Defender para Storage](defender-for-storage-introduction.md).


## <a name="data-consumption"></a>Consumo de datos

Los clientes pueden acceder a los datos relacionados de Defender desde los siguientes flujos de datos:


| STREAM                                                                                | Tipos de datos                                                                                                                                                                                                          |
|---------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Registro de actividad de Azure](../azure-monitor/platform/activity-log.md)                       | Todas las alertas de seguridad, las solicitudes de acceso [Just-In-Time](security-center-just-in-time.md) aprobadas de Defender y todas las alertas generadas por los [controles de aplicaciones adaptables](security-center-adaptive-application.md).|
| [Registros de Azure Monitor](../azure-monitor/platform/data-platform.md)                      | Todas las alertas de seguridad.                                                                                                                                                                                                |
| [Azure Resource Graph](../governance/resource-graph/overview.md)                      | Las alertas de seguridad, las recomendaciones de seguridad, los resultados de la evaluación de vulnerabilidades, la información de puntuación segura, el estado de las comprobaciones de cumplimiento, etc.                                                                       |
| [API REST de Azure Security Center](/rest/api/securitycenter/) | Las alertas de seguridad, las recomendaciones de seguridad y mucho más.                                                                                                                                                                |
|                                                                                       |                                                                                                                                                                                                                     |

## <a name="next-steps"></a>Pasos siguientes

En este documento, se ha explicado cómo se administran y protegen los datos en Azure Security Center. 

Para más información sobre Azure Defender, consulte [¿Qué es Azure Defender?](security-center-introduction.md)