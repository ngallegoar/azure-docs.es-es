---
ms.openlocfilehash: 3fc2475569765116d46a175629f25d9d49634942
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95993963"
---
En este artículo se enumeran las versiones y las características del agente de aprovisionamiento de Azure Active Directory Connect que se han publicado. El equipo de Azure AD actualiza periódicamente el agente de aprovisionamiento con nuevas características y funciones. El agente de aprovisionamiento se actualiza de forma automática cuando se publica una nueva versión. 

Microsoft proporciona soporte técnico directo para la versión más reciente del agente y una versión anterior.

## <a name="112810"></a>1.1.281.0

### <a name="release-status"></a>Estado de la versión

23 de noviembre de 2020: publicado para descarga.

### <a name="new-features-and-improvements"></a>Nuevas características y mejoras

* Compatibilidad con [gMSA](../articles/active-directory/cloud-provisioning/how-to-prerequisites.md#group-managed-service-accounts).
* Compatibilidad con grupos de hasta 1500 miembros durante el ciclo de sincronización incremental o diferencial. Esto se puede aplicar cuando se usa el filtro de ámbito de grupo
* Compatibilidad con grupos grandes, de hasta 15 000 miembros
* Mejoras en la sincronización inicial
* Registro detallado avanzado
* Introducción del [módulo AADCloudSyncTools de PowerShell](../articles/active-directory/cloud-provisioning/reference-powershell.md)
* Limitaciones fijas para que el agente se pueda instalar en un servidor que no esté en inglés
* Compatibilidad con el filtrado PHS solo para los objetos en el ámbito (originalmente, se sincronizaban los hash de contraseña de todos los objetos)
* Se ha corregido el problema de fuga de memoria en el agente
* Registros de aprovisionamiento mejorados


## <a name="11960"></a>1.1.96.0

### <a name="release-status"></a>Estado de la versión

4 de diciembre de 2019: publicado para descarga.

### <a name="new-features-and-improvements"></a>Nuevas características y mejoras

* Incluye compatibilidad con el [aprovisionamiento en la nube de Azure AD Connect](../articles/active-directory/cloud-provisioning/what-is-cloud-provisioning.md) para sincronizar datos de usuarios, contactos y grupos de la instancia local de Active Directory a Azure AD


## <a name="11670"></a>1.1.67.0

### <a name="release-status"></a>Estado de la versión

9 de septiembre de 2019: publicada para actualización automática

### <a name="new-features-and-improvements"></a>Nuevas características y mejoras

* Capacidad de configurar el seguimiento y el registro adicionales para depurar problemas del agente de aprovisionamiento
* Capacidad de capturar solo los atributos de Azure AD configurados en la asignación para mejorar el rendimiento de la sincronización

### <a name="fixed-issues"></a>Problemas corregidos

* Se corrigió un error que hacía que el agente dejara de responder si había problemas de conexión con Azure AD.
* Se corrigió un error que causaba problemas cuando se leían datos binarios de Azure Active Directory.
* Se corrigió un error por el que el agente no podía renovar la confianza con el servicio de identidad híbrida en la nube.

## <a name="11300"></a>1.1.30.0

### <a name="release-status"></a>Estado de la versión

23 de enero de 2019: publicado para descarga.

### <a name="new-features-and-improvements"></a>Nuevas características y mejoras

* Se ha renovado la arquitectura del conector y el agente de aprovisionamiento para mejorar el rendimiento, la estabilidad y la confiabilidad. 
* Se ha simplificado la configuración del agente de aprovisionamiento simplificado mediante el Asistente para la instalación basada en la interfaz de usuario 
* Se ha agregado la compatibilidad para las actualizaciones automáticas del agente.


