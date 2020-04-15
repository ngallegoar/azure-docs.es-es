---
title: 'Plan de continuidad empresarial: QnA Maker'
description: El objetivo principal del plan de continuidad empresarial consiste en crear un punto de conexión de base de conocimiento resistente, que garantizaría que el bot o la aplicación que lo consume no tengan tiempos de inactividad.
ms.topic: conceptual
ms.date: 04/07/2020
ms.openlocfilehash: 5d1501ecc42fe948959075cec7d728f6c9df908a
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80887082"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>Cómo crear un plan de continuidad empresarial para el servicio QnA Maker

El objetivo principal del plan de continuidad empresarial consiste en crear un punto de conexión de base de conocimiento resistente, que garantizaría que el bot o la aplicación que lo consume no tengan tiempos de inactividad.

## <a name="business-continuity-with-traffic-manager"></a>Continuidad del negocio con Traffic Manager

> [!div class="mx-imgBorder"]
> ![Plan bcp de QnA Maker](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

La idea de alto nivel como se representa anteriormente es la siguiente:

1. Configure dos [servicios QnA Maker](set-up-qnamaker-service-azure.md) paralelos en [Regiones emparejadas de Azure](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

2. [Realice una copia de seguridad](../../../app-service/manage-backup.md) del servicio de aplicaciones de QnA Maker principal y [restáurela](../../../app-service/web-sites-restore.md) en la configuración secundaria. De este modo, se asegurará de que ambas configuraciones funcionan con el mismo nombre de host y las mismas claves.

3. Mantenga sincronizados los índices principal y secundario de Azure Search. Use el ejemplo de GitHub [aquí](https://github.com/pchoudhari/QnAMakerBackupRestore) para ver cómo realizar una copia de seguridad de los índices de Azure y cómo restaurarlos.

4. Realice una copia de seguridad de Application Insights con la [exportación continua](../../../application-insights/app-insights-export-telemetry.md).

5. Una vez configuradas las pilas principal y secundaria, use [Traffic Manager](../../../traffic-manager/traffic-manager-overview.md) para configurar los dos puntos de conexión y establecer un método de enrutamiento.

6. Debe crear un certificado de Seguridad de la capa de transporte (TLS), conocido anteriormente como Capa de sockets seguros (SSL), para el punto de conexión de Traffic Manager. [Enlace el certificado TLS/SSL](../../../app-service/configure-ssl-bindings.md) en los servicios de aplicaciones.

7. Por último, use el punto de conexión de Traffic Manager del bot o de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Elección de capacidad](./improve-knowledge-base.md)