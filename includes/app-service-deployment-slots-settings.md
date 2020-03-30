---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 09/18/2019
ms.author: cephalin
ms.openlocfilehash: e00db06346b19ef85eb77626eb2ed169d2224b6c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "71129692"
---
Cuando crea un clon de la configuración de otro espacio de implementación, la configuración clonada se puede editar. Algunos elementos de configuración siguen al contenido en los intercambios (no son específicos de la ranura), mientras que otros permanecen en la misma ranura después de este (específicos). Las listas siguientes muestran la configuración que cambia cuando se intercambian las ranuras.

**Configuraciones que se intercambian**:

* Configuración general: por ejemplo, versión de Framework, 32 o 64 bits, Web Sockets
* Configuración de la aplicación (puede configurarse para ajustarse a un espacio)
* Cadenas de conexión (puede configurarse para ajustarse a un espacio)
* Asignaciones de controlador
* Certificados públicos
* Contenido de WebJobs
* Conexiones híbridas *
* Integración de la red virtual *
* Puntos de conexión de servicio *
* Azure Content Delivery Network *

Se prevé que las características marcadas con un asterisco (*) no se intercambien. 

**Valores que no se intercambian**:

* Extremos de publicación
* Nombres de dominio personalizados
* Certificados no públicos y configuración de TLS/SSL
* Configuración de escala
* Programadores de WebJobs
* Restricciones de IP
* Always On
* Configuración del registro de diagnóstico
* Uso compartido de recursos entre orígenes (CORS)

> [!NOTE]
> Algunas configuraciones de aplicaciones que se aplican a configuraciones no intercambiadas no se intercambian. Por ejemplo, como la configuración del registro de diagnóstico no se intercambia, las configuraciones de aplicaciones relacionada, como `WEBSITE_HTTPLOGGING_RETENTION_DAYS` y `DIAGNOSTICS_AZUREBLOBRETENTIONDAYS` tampoco lo hacen, aunque no se muestren como valores de ranura.
>
