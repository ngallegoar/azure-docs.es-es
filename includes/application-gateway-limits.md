---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 03/04/2020
ms.author: victorh
ms.openlocfilehash: 92ec7c0a1469c9f02855cd6191faa8514e54c8f0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91828929"
---
| Resource | Límite | Nota: |
| --- | --- | --- |
| Azure Application Gateway |1000 por suscripción | |
| Configuración de direcciones IP de front-end |2 |1 pública y 1 privada |
| Puertos de front-end |100<sup>1</sup> | |
| Grupos de direcciones de back-end |100<sup>1</sup> | |
| Servidores back-end por grupo |1,200 | |
| Agentes de escucha HTTP |200<sup>1</sup> |Limitado a 100 clientes de escucha activos que enrutan el tráfico. Clientes de escucha activos = número total de clientes de escucha - clientes de escucha no activos.<br>Si se establece una configuración predeterminada dentro de una regla de enrutamiento para enrutar el tráfico (por ejemplo, tiene un cliente de escucha, un grupo de back-end y la configuración de HTTP), también cuenta como cliente de escucha.|
| Reglas de equilibrio de carga HTTP |100<sup>1</sup> | |
| Configuración de HTTP de back-end |100<sup>1</sup> | |
| Instancias por puerta de enlace |V1 SKU: 32<br>V2 SKU: 125 | |
| Certificados SSL |100<sup>1</sup> |Uno por cliente de escucha HTTP |
| Tamaño máximo de certificados SSL |V1 SKU: 10 KB<br>V2 SKU: 16 KB| |
| Certificados de autenticación |100 | |
| Certificados raíz de confianza |100 | |
| Tiempo de espera de solicitud mínimo |1 segundo | |
| Tiempo de espera de solicitud máximo |24 horas | |
| Número de sitios |100<sup>1</sup> |Uno por cliente de escucha HTTP |
| Asignaciones de URL por agente de escucha |1 | |
| Número máximo de reglas basadas en rutas por mapa de direcciones URL|100||
| Configuraciones de redirección |100<sup>1</sup>| |
| Conexiones simultáneas de WebSocket |Puertas de enlace medianas 20k<br> Puertas de enlace grandes 50k| |
| Longitud máxima de dirección URL|32 KB| |
| Tamaño de encabezado máximo para HTTP/2 |4 KB| |
| Tamaño máximo de carga de archivos (estándar) |2 GB | |
| Tamaño máximo de carga de archivos WAF |Puertas de enlaces v1 medianas WAF, 100 MB<br>Puertas de enlace v1 grandes WAF, 500 MB<br>V2 WAF, 750 MB| |
| Límite de tamaño de cuerpo de WAF (sin archivos)|128 KB||
| Máximo de reglas personalizadas de WAF|100||
| Exclusiones máximas de WAF|100||

<sup>1</sup> En caso de las SKU con WAF habilitado, debe limitar el número de recursos a 40.
