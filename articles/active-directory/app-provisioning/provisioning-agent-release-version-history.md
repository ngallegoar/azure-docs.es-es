---
title: 'Agente de aprovisionamiento de Azure AD Connect: Historial de lanzamiento de versiones | Microsoft Docs'
description: En este artículo se enumeran todas las versiones del agente de aprovisionamiento de Azure AD Connect y se describen las nuevas características y los problemas corregidos.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.topic: reference
ms.workload: identity
ms.date: 02/26/2020
ms.subservice: app-provisioning
ms.author: kenwith
ms.reviewer: celested
ms.openlocfilehash: 9e05d1a85f17800ddf4d77e4e4acba6396a8da47
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84781572"
---
# <a name="azure-ad-connect-provisioning-agent-version-release-history"></a>Agente de aprovisionamiento de Azure AD Connect: Historial de lanzamiento de versiones

En este artículo se enumeran las versiones y las características del agente de aprovisionamiento de Azure Active Directory Connect que se han publicado. El equipo de Azure AD actualiza periódicamente el agente de aprovisionamiento con nuevas características y funciones. El agente de aprovisionamiento se actualiza de forma automática cuando se publica una nueva versión. 

Microsoft proporciona soporte técnico directo para la versión más reciente del agente y una versión anterior.

## <a name="11960"></a>1.1.96.0

### <a name="release-status"></a>Estado de la versión

4 de diciembre de 2019: publicado para descarga.

### <a name="new-features-and-improvements"></a>Nuevas características y mejoras

* Incluye compatibilidad con el [aprovisionamiento en la nube de Azure AD Connect](../cloud-provisioning/what-is-cloud-provisioning.md) para sincronizar datos de usuarios, contactos y grupos de la instancia local de Active Directory a Azure AD


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

