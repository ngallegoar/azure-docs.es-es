---
title: Azure Front Door | Microsoft Docs
description: En este artículo se ofrece información general sobre Azure Front Door. Averigüe si es la elección correcta para equilibrar la carga de tráfico de usuario de la aplicación.
services: frontdoor
documentationcenter: ''
author: megan-beatty
editor: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 4/30/2020
ms.author: mebeatty
ms.openlocfilehash: 19deb763c8e750490854892c90d0293d3e209c09
ms.sourcegitcommit: eaec2e7482fc05f0cac8597665bfceb94f7e390f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82515800"
---
# <a name="what-is-rules-engine-for-azure-front-door"></a>¿Qué es el motor de reglas de Azure Front Door? 

El motor de reglas permite personalizar el modo en que se administran las solicitudes HTTP en el perímetro y proporciona un mayor control sobre el comportamiento de la aplicación web. El motor de reglas de Azure Front Door consta de varias características clave, entre las que se incluyen:

- Enrutamiento basado en encabezados: se enrutan las solicitudes según los patrones del contenido de los encabezados de solicitud, las cookies y las cadenas de consulta.
- Enrutamiento basado en parámetros: se aprovecha una serie de condiciones de coincidencia entre las que se incluyen argumentos POST, cadenas de consulta, cookies y métodos de solicitud, para enrutar solicitudes basándose en los parámetros de la solicitud HTTP. 
- Invalidaciones de las configuraciones de enrutamiento: 
    - Use las funcionalidades de redireccionamiento para devolver los redireccionamientos 301/302/307/308 al cliente y redirigir a nuevos nombres de host, rutas de acceso y protocolos. 
    - Use las funcionalidades de reenvío para volver a escribir la ruta de la dirección URL de la solicitud sin realizar un redireccionamiento tradicional y reenviar la solicitud al back-end adecuado del grupo de back-ends configurado. 
    - Personalice la configuración de almacenamiento en caché y cambie dinámicamente una ruta del método de reenvío al de almacenamiento en caché en función de las condiciones de coincidencia. 

> [!IMPORTANT]
> Esta versión preliminar pública se proporciona sin un acuerdo de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="architecture"></a>Architecture 

El motor de reglas administra las solicitudes en el perímetro. Una vez que se configura el motor de reglas, cuando una solicitud alcanza el punto de conexión de Front Door, primero se ejecuta el firewall de aplicaciones web y, después, la configuración del motor de reglas asociada a su front-end o dominio. Cuando se ejecuta una configuración del motor de reglas, significa que la regla de enrutamiento principal ya es una coincidencia. El hecho de que se ejecuten todas las acciones de cada una de las reglas de la configuración del motor de reglas está sujeto a que se satisfagan todas las condiciones de coincidencia de esa regla. Si una solicitud no coincide con ninguna de las condiciones de la configuración del motor de reglas, se ejecuta la regla de enrutamiento predeterminada. 

Por ejemplo, en la configuración siguiente, se configura un motor de reglas para anexar un encabezado de respuesta que cambia la duración máxima del control de caché si se cumple la condición de coincidencia. 

![acción del encabezado de respuesta](./media/front-door-rules-engine/rules-engine-architecture-3.png)

En otro ejemplo, vemos que el motor de reglas está configurado para enviar a un usuario a una versión móvil del sitio web si la condición de coincidencia "tipo de dispositivo" es true. 

![invalidación de la configuración de enrutamiento](./media/front-door-rules-engine/rules-engine-architecture-1.png)

En ambos ejemplos, si no se cumple ninguna de las condiciones de coincidencia, se ejecuta la regla de ruta especificada. 

## <a name="terminology"></a>Terminología 

Con el motor de reglas de Azure Front Door, puede crear una serie de configuraciones para dicho motor, cada una de las cuales se compone de un conjunto de reglas. A continuación se describen ciertos términos útiles que se utilizarán al configurar el motor de reglas. 

- *Configuración del motor de reglas*: Conjunto de reglas que se aplican a una única regla de enrutamiento. Cada configuración se limita a 5 reglas. Puede crear hasta 10 configuraciones. 
- *Regla del motor de reglas*: Una regla compuesta de un máximo de 10 condiciones de coincidencia y 5 acciones.
- *Condición de coincidencia*: Existen numerosas condiciones de coincidencia que se pueden usar para analizar las solicitudes entrantes. Una regla puede contener hasta 10 condiciones de coincidencia. Las condiciones de coincidencia se evalúan con un operador **AND**. [Aquí](front-door-rules-engine-match-conditions.md) encontrará una lista completa de las condiciones de coincidencia. 
- *Acción*: Las acciones indican lo que ocurre en las solicitudes entrantes: en la actualidad hay disponibles acciones de encabezado de solicitud o respuesta, reenvío, redireccionamientos y reescrituras. Una regla puede contener hasta 5 acciones; sin embargo, una regla puede contener solo una invalidación de la configuración de enrutamiento.  [Aquí](front-door-rules-engine-actions.md) encontrará una lista completa de las acciones.


## <a name="next-steps"></a>Pasos siguientes

- Aprenda a configurar su primera [configuración del motor de reglas](front-door-tutorial-rules-engine.md). 
- Aprenda a [crear una instancia de Front Door](quickstart-create-front-door.md).
- Más información acerca de cómo [funciona Front Door](front-door-routing-architecture.md).
