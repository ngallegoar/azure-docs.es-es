---
title: Novedades del firewall de aplicaciones web de Azure
description: Conozca las novedades del firewall de aplicaciones web de Azure, como, por ejemplo, las notas de la versión más recientes, los problemas conocidos, las correcciones de errores, las funcionalidades en desuso y los próximos cambios.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.topic: overview
ms.date: 10/24/2019
ms.author: victorh
ms.openlocfilehash: 11212c931e38b1c6319c7c3554155d9d17a1080c
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83745143"
---
# <a name="whats-new-in-azure-web-application-firewall"></a>Novedades del firewall de aplicaciones web de Azure

El firewall de aplicaciones web de Azure se actualiza de forma continuada. Para mantenerse al día con los avances más recientes, este artículo proporciona información acerca de los elementos siguientes:

- Versiones más recientes
- Problemas conocidos
- Corrección de errores
- Funciones obsoletas

## <a name="new-features"></a>Nuevas características

|Característica  |Descripción  |Fecha de adición  |
|---------|---------|---------|
|Conjunto de reglas de mitigación de bots (versión preliminar)|Puede habilitar un conjunto de reglas de mitigación de bots, junto con el conjunto de reglas de CRS que elija. | Noviembre de 2019 |
|Integración con GeoDB (versión preliminar)|Ahora puede crear reglas personalizadas que restrinjan el tráfico por país o región de origen. | Noviembre de 2019 |
|Directiva de WAF por sitio/por URI (versión preliminar)|WAF-v2 ahora admite la aplicación de una directiva a los clientes de escucha, así como reglas basadas en rutas de acceso. Consulte [Creación de una directiva de WAF](create-waf-policy-ag.md). | Noviembre de 2019 |
|Reglas personalizadas del firewall de aplicaciones web |Application Gateway WAF_v2 ya admite la creación de reglas personalizadas. Consulte [Reglas personalizadas de Application Gateway](custom-waf-rules-overview.md). |Junio de 2019 |
|Configuración de WAF y lista de exclusión     |Hemos agregado más opciones para ayudarle a configurar el WAF y reducir los falsos positivos. Consulte [Listas de exclusión y límites de tamaño de solicitud del firewall de aplicaciones web](application-gateway-waf-configuration.md) para más información.|Diciembre de 2018|

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el firewall de aplicaciones web en Application Gateway, consulte [Firewall de aplicaciones web de Azure en Azure Application Gateway](ag-overview.md).
