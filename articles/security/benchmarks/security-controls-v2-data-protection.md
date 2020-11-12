---
title: Protección de datos de Azure Security Benchmark V2
description: Protección de datos de Azure Security Benchmark V2
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/20/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: bde95006865a2aab2da4a5d6d27ccccc14d74d12
ms.sourcegitcommit: 17b36b13857f573639d19d2afb6f2aca74ae56c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94408929"
---
# <a name="security-control-v2-data-protection"></a>Control de seguridad V2: Protección de datos

La protección de datos cubre el control de la protección de datos en reposo, en tránsito y a través de mecanismos de acceso autorizados. Esto incluye detectar, clasificar, proteger y supervisar los recursos de datos confidenciales mediante el control de acceso, el cifrado y el registro en Azure.

## <a name="dp-1-discovery-classify-and-label-sensitive-data"></a>DP-1: Detección, clasificación y etiquetado de datos confidenciales

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| DP-1 | 13.1, 14.5, 14.7 | SC-28 |

Detecte, clasifique y etiquete los datos confidenciales para que se puedan diseñar los controles adecuados que garanticen que los sistemas tecnológicos de la organización almacenen, procesen y transmitan la información confidencial de forma segura. 

Use Azure Information Protection (y su herramienta de detección asociada) con la información confidencial de los documentos de Office en Azure, del entorno local, de Office 365 y de otras ubicaciones. 

Puede utilizar Azure SQL Information Protection para ayudar en la clasificación y el etiquetado de la información almacenada en las instancias de Azure SQL Database.

- [Etiquetado de información confidencial mediante Azure Information Protection](/azure/information-protection/what-is-information-protection) 

- [Implementación de la detección de datos de Azure SQL](../../azure-sql/database/data-discovery-and-classification-overview.md)

**Responsabilidad** : Compartido

**Partes interesadas de seguridad del cliente** ( [Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Seguridad de las aplicaciones y DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)  

- [Seguridad de los datos](/azure/cloud-adoption-framework/organize/cloud-security-data-security) 

- [Infraestructura y seguridad de los puntos de conexión](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

## <a name="dp-2-protect-sensitive-data"></a>DP-2: Protección de datos confidenciales

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| DP-2 | 13.2, 2.10 | SC-7, AC-4 |

Proteja los datos confidenciales mediante la restricción del acceso con el control de acceso basado en rol de Azure (Azure RBAC), los controles de acceso basados en la red y los controles específicos de los servicios de Azure (como el cifrado en SQL y otras bases de datos). 

Para garantizar un control de acceso coherente, todos los tipos de control de acceso se deben alinear con la estrategia de segmentación de la empresa. La estrategia de segmentación de la empresa también debe estar informada de la ubicación de los datos y sistemas confidenciales o críticos para la empresa.

En el caso de la plataforma subyacente administrada por Microsoft, Microsoft trata todo el contenido de los clientes como confidencial y protege a los clientes contra la pérdida y exposición de los datos. Para garantizar la seguridad de los datos de los clientes dentro de Azure, Microsoft ha implementado algunos controles y funcionalidades de protección de datos predeterminados.

- [Control de acceso basado en rol (RBAC) de Azure](../../role-based-access-control/overview.md)

- [Descripción de la protección de datos de los clientes en Azure](../fundamentals/protection-customer-data.md)

**Responsabilidad** : Compartido

**Partes interesadas de seguridad del cliente** ( [Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Seguridad de las aplicaciones y DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

- [Seguridad de los datos](/azure/cloud-adoption-framework/organize/cloud-security-data-security)

- [Infraestructura y seguridad de los puntos de conexión](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

## <a name="dp-3-monitor-for-unauthorized-transfer-of-sensitive-data"></a>DP-3: Supervisión de la transferencia no autorizada de datos confidenciales

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| DP-3 | 13.3 | AC-4, SI-4 |

Supervisión de la transferencia de datos no autorizada a ubicaciones que se encuentran fuera de la visibilidad y el control de la empresa. Normalmente, esto implica la supervisión de actividades anómalas (transferencias grandes o inusuales) que podrían indicar una filtración de datos no autorizada. 

Advanced Threat Protection (ATP) para Azure Storage y Azure SQL ATP pueden alertar sobre la transferencia anómala de información que podría indicar transferencias no autorizadas de información confidencial. 

Azure Information Protection (AIP) proporciona funcionalidades de supervisión para la información que se ha clasificado y etiquetado. 

Si es necesario para el cumplimiento de la prevención de la pérdida de datos (DLP por sus siglas en inglés), se puede usar una solución de DLP basada en host para aplicar controles de detección o prevención a fin de evitar la filtración de datos.

- [Habilitación de ATP para Azure SQL](../../azure-sql/database/threat-detection-overview.md)

- [Habilitación de ATP para Azure Storage](../../storage/common/azure-defender-storage-configure.md?tabs=azure-security-center)

**Responsabilidad** : Compartido

**Partes interesadas de seguridad del cliente** ( [Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Operaciones de seguridad](/azure/cloud-adoption-framework/organize/cloud-security) 

- [Seguridad de las aplicaciones y DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

- [Infraestructura y seguridad de los puntos de conexión](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

## <a name="dp-4-encrypt-sensitive-information-in-transit"></a>DP-4: Cifrado de la información confidencial en tránsito

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| DP-4 | 14.4 | SC-8 |

Para complementar los controles de acceso, los datos en tránsito deben protegerse frente a ataques de "fuera de banda" (por ejemplo, captura del tráfico) mediante cifrado para que los atacantes no puedan leer ni modificar los datos fácilmente. 

Aunque esto es opcional en el caso del tráfico de redes privadas, es fundamental para el tráfico de redes externas y públicas. Asegúrese de que los clientes que se conectan a los recursos de Azure puedan negociar TLS v1.2 o superior. Para la administración remota, use SSH (para Linux) o RDP/TLS (para Windows) en lugar de un protocolo sin cifrar. Se deben deshabilitar los protocolos y las versiones de SSL, TLS y SSH obsoletos, así como los cifrados débiles.  

De forma predeterminada, Azure proporciona el cifrado de los datos en tránsito entre los centros de datos de Azure. 

- [Descripción del cifrado en tránsito con Azure](../fundamentals/encryption-overview.md#encryption-of-data-in-transit)

- [Información sobre la seguridad de TLS](/security/engineering/solving-tls1-problem)

- [Cifrado doble para datos de Azure en tránsito](../fundamentals/double-encryption.md#data-in-transit)

**Responsabilidad** : Compartido

**Partes interesadas de seguridad del cliente** ( [Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Arquitectura de seguridad](/azure/cloud-adoption-framework/organize/cloud-security-architecture) 

- [Infraestructura y seguridad de los puntos de conexión](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Seguridad de las aplicaciones y DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

- [Seguridad de los datos](/azure/cloud-adoption-framework/organize/cloud-security-data-security)

## <a name="dp-5-encrypt-sensitive-data-at-rest"></a>DP-5: Cifrado de datos confidenciales en reposo

| Identificador de Azure | Identificadores de CIS Controls v7.1 | Identificadores de NIST SP800-53 r4 |
|--|--|--|--|
| DP-5 | 14.8 | SC-28, SC-12 |

Para complementar los controles de acceso, los datos en reposo deben protegerse frente a ataques de "fuera de banda" (por ejemplo, el acceso al almacenamiento subyacente) mediante cifrado. Esto ayuda a garantizar que los atacantes no puedan leer ni modificar los datos fácilmente. 

Azure proporciona cifrado de datos en reposo de forma predeterminada. En el caso de datos muy confidenciales, tiene opciones para implementar el cifrado adicional en reposo en todos los recursos de Azure donde estén disponibles. Azure administra las claves de cifrado de forma predeterminada, pero también le ofrece opciones para administrar sus propias claves (claves administradas por el cliente) para determinados servicios de Azure.

- [Descripción del cifrado en reposo en Azure](../fundamentals/encryption-atrest.md#encryption-at-rest-in-microsoft-cloud-services)

- [Configuración de las claves de cifrado administradas por el cliente](../../storage/common/customer-managed-keys-configure-key-vault.md)

- [Tabla de modelo de cifrado y administración de claves](../fundamentals/encryption-models.md)

- [Cifrado doble de datos en reposo en Azure](../fundamentals/double-encryption.md#data-at-rest)

**Responsabilidad** : Compartido

**Partes interesadas de seguridad del cliente** ( [Más información](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Arquitectura de seguridad](/azure/cloud-adoption-framework/organize/cloud-security-architecture) 

- [Infraestructura y seguridad de los puntos de conexión](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Seguridad de las aplicaciones y DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Seguridad de los datos](/azure/cloud-adoption-framework/organize/cloud-security-data-security)