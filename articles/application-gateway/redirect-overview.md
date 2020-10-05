---
title: Introducción al redireccionamiento para Azure Application Gateway
description: Obtenga información sobre la funcionalidad de redirección en Azure Application Gateway para redirigir el tráfico recibido en un agente de escucha a otro agente de escucha o a un sitio externo.
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.topic: conceptual
ms.date: 11/16/2019
ms.author: amsriva
ms.openlocfilehash: fb01d5a4923410f693b682d66be8d5d09f9019d0
ms.sourcegitcommit: 6e1124fc25c3ddb3053b482b0ed33900f46464b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2020
ms.locfileid: "90561585"
---
# <a name="application-gateway-redirect-overview"></a>Introducción a la redirección de Application Gateway

Puede usar Application Gateway para redirigir el tráfico.  Cuenta con un mecanismo de redireccionamiento genérico que permite el redireccionamiento del tráfico recibido en un cliente de escucha a otro cliente de escucha o a un sitio externo. Esto simplifica la configuración de la aplicación, optimiza el uso de recursos y admite nuevos escenarios de redirección incluida la global y la basada en la ruta de acceso.

Resulta un escenario de redireccionamiento común para muchas aplicaciones web el admitir la redirección automática de HTTP a HTTPS para asegurarse de que toda la comunicación entre la aplicación y sus usuarios se produce a través de una ruta de acceso cifrada. En el pasado, los clientes usaban técnicas como la creación de un grupo back-end dedicado cuya única finalidad era redirigir las solicitudes que recibía de HTTP a HTTPS. Gracias a la compatibilidad con el redireccionamiento de Application Gateway, puede realizar esta tarea simplemente agregando una nueva configuración de redireccionamiento a una regla de enrutamiento y especificando otro agente de escucha con el protocolo HTTPS como agente de escucha de destino.

Se admiten los siguientes tipos de redireccionamiento:

- 301 (redirección permanente)
- 302 (encontrado)
- 303 (ver otras)
- 307 (redirección temporal)

La compatibilidad con la redirección de Application Gateway ofrece las siguientes funcionalidades:

-  **Redireccionamiento global**

   Redirecciona desde un agente de escucha a otro en la puerta de enlace. Permite la redirección de HTTP a HTTPS en un sitio.
- **Redireccionamiento basado en la ruta de acceso**

   Este tipo de redirección permite la redirección de HTTP a HTTPS solo en un área de un sitio específico, por ejemplo un área de carro de la compra que se indica mediante /carro/*.
- **Redirección a un sitio externo**

![En el diagrama se muestran los usuarios y una instancia de App Gateway y las conexiones entre ambos, incluida una flecha de color rojo HTTP desbloqueada, una flecha roja directa 301 no permitida y una flecha de color verde HTTPS bloqueada.](./media/redirect-overview/redirect.png)

Con este cambio, los clientes tienen que crear un nuevo objeto de configuración de redireccionamiento, que especifique el agente de escucha de destino o el sitio externo al que se desea dirigir el redireccionamiento. El elemento de configuración también admite opciones para anexar la cadena de consulta y la ruta de acceso URI para la dirección URL redirigida. También puede elegir el tipo de redireccionamiento. Una vez creada esta configuración de redireccionamiento, se adjunta al agente de escucha de origen a través de una nueva regla. Cuando se usa una regla básica, la configuración de redirección se asocia a un agente de escucha de origen y es una redirección global. Cuando se utiliza una regla basada en rutas de acceso, la configuración de redireccionamiento se define en el mapa de rutas de acceso de dirección URL. Por lo tanto, solo se aplica al área específica de la ruta de acceso de un sitio.

### <a name="next-steps"></a>Pasos siguientes

[Configuración de la redirección de direcciones URL en una puerta de enlace de aplicaciones](tutorial-url-redirect-powershell.md)
