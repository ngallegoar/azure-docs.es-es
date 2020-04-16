---
title: Movimiento de recursos de Azure App Service
description: Use Azure Resource Manager para trasladar recursos de App Service a un nuevo grupo de recursos o a una nueva suscripción.
ms.topic: conceptual
ms.date: 12/13/2019
ms.openlocfilehash: d0ecd117bdcda9238e310a3020dba19a6871a3fc
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80655782"
---
# <a name="move-guidance-for-app-service-resources"></a>Orientaciones para el traslado de recursos de App Service

En este artículo se describen los pasos para trasladar recursos de App Service. Hay requisitos específicos para trasladar recursos de App Service a una nueva suscripción.

## <a name="move-across-subscriptions"></a>Traslado entre suscripciones

Al trasladar una instancia de Web App entre suscripciones, se aplican las guías siguientes:

- El grupo de recursos de destino no debe tener ningún recurso de App Service. Entre los recursos de App Service se incluyen:
    - Web Apps
    - Planes de App Service
    - Certificados TLS/SSL cargados o importados
    - Entornos de App Service
- Todos los recursos de App Service del grupo de recursos se deben mover conjuntamente. Tenga en cuenta que los entornos de App Service Environment no se pueden trasladar a un nuevo grupo de recursos ni a una nueva suscripción.
- Puede trasladar un certificado enlazado a una web sin eliminar los enlaces TLS, siempre y cuando el certificado se mueva con el resto de recursos del grupo.
- Los recursos de App Service solo se pueden mover del grupo de recursos en el que se crearon originalmente. Si un recurso de App Service ya no está en su grupo de recursos original, muévalo a su grupo de recursos original. A continuación, mueva el recurso entre las suscripciones.

Si no recuerda el grupo de recursos original, se puede encontrar a través de un diagnóstico. Para la aplicación web, seleccione **Diagnosticar y solucionar problemas**. A continuación, seleccione **Configuración y administración**.

![Selección de diagnósticos](./media/app-service-move-limitations/select-diagnostics.png)

Seleccione **Opciones de migración**.

![Selección de opciones de migración](./media/app-service-move-limitations/select-migration.png)

Seleccione la opción de los pasos recomendados para trasladar la aplicación web.

![Selección de pasos recomendados](./media/app-service-move-limitations/recommended-steps.png)

Puede ver las acciones recomendadas que deben realizarse antes de trasladar los recursos. La información incluye el grupo de recursos original de la aplicación web.

![Recomendaciones](./media/app-service-move-limitations/recommendations.png)

## <a name="move-support"></a>Compatibilidad con el movimiento

Para determinar qué recursos de App Service se pueden mover, consulte el estado de compatibilidad con el traslado de:

- [Microsoft.AppService](../move-support-resources.md#microsoftappservice)
- [Microsoft.CertificateRegistration](../move-support-resources.md#microsoftcertificateregistration)
- [Microsoft.DomainRegistration](../move-support-resources.md#microsoftdomainregistration)
- [Microsoft.Web](../move-support-resources.md#microsoftweb)

## <a name="next-steps"></a>Pasos siguientes

Para ver los comandos para trasladar recursos, vea [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](../move-resource-group-and-subscription.md).
