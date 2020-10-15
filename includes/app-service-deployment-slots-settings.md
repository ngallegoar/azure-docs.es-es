---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 09/18/2019
ms.author: cephalin
ms.openlocfilehash: b4234acb2ce32980a268e389cb31de9a57ed18e6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "82132007"
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
* Configuración de diagnóstico
* Uso compartido de recursos entre orígenes (CORS)

> [!NOTE]
> Algunas configuraciones de aplicaciones que se aplican a configuraciones no intercambiadas no se intercambian. Por ejemplo, como los valores de diagnóstico no se intercambian, los valores de aplicaciones relacionados como `WEBSITE_HTTPLOGGING_RETENTION_DAYS` y `DIAGNOSTICS_AZUREBLOBRETENTIONDAYS` tampoco lo hacen, aunque no se muestren como valores de ranura.
>
