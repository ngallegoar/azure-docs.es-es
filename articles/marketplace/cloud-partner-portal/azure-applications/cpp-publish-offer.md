---
title: Publicación de una oferta de aplicación de Azure | Azure Marketplace
description: Describe el proceso y los pasos de publicación de una oferta de aplicación de Azure en Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: dsindona
ms.openlocfilehash: 9f89e31c2d17ef74971d2057ba58e9572e92184c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280106"
---
# <a name="publish-azure-application-offer"></a>Publicación de una oferta de aplicación de Azure

Después de crear una oferta al proporcionar la información de la página **Nueva oferta**, puede publicarla. Seleccione **Publicar** para iniciar el proceso de publicación.

El diagrama siguiente muestra los pasos principales del proceso de publicación de una oferta para "lanzarla".

![Pasos para la publicación de una oferta](./media/offer-publishing-steps.png)


## <a name="detailed-description-of-publishing-steps"></a>Descripción detallada de los pasos de publicación

En la siguiente tabla se enumera y se describe cada paso de publicación, y se incluye una estimación de tiempo para completar cada paso.  Los tiempos, estimados en días, se definen como los hábiles, fines de semana y festivos excluidos.

|  **Paso de publicación**           | **Time**    | **Descripción**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Validar requisitos previos         | <15 min    | Se validan la información y la configuración de la oferta.                        |
| Validar la configuración de los ingresos influidos | <15 min  | Se comprueba la atribución de uso de recursos de Azure para la oferta.             |
| Certificación                  | <1 día     | El equipo de certificación de Azure analiza la oferta. Se examina la oferta en busca de virus, malware, cumplimiento de seguridad y problemas de seguridad. La oferta se comprueba para ver si cumple todos los criterios de elegibilidad. Para más información, consulte los [requisitos previos](./cpp-prerequisites.md). Si se detecta un problema, se proporcionan comentarios. |
| Validación de la versión de prueba          | <2 horas   | (Opcional) Si hay una versión de prueba, Microsoft valida que puede implementar y replicar.  |
| Empaquetado y registro de la generación de clientes potenciales | <1 hora  | Los recursos técnicos de la oferta se empaquetan para el uso del cliente y se configuran e implementan los sistemas del cliente potencial. |
|  Aprobación del publicador             |  manual    | Revisión final del publicador y confirmación antes del lanzamiento de la oferta. La oferta está ahora disponible para la versión preliminar.  Puede implementar la oferta en las suscripciones seleccionadas (en los pasos de información de la oferta) para comprobar que cumple todos los requisitos.  Una vez verificada la oferta, seleccione **Transmitir** para que la oferta pueda pasar al paso siguiente. |
| Revisión de Microsoft                | Entre 7 y 14 días | Microsoft revisa holísticamente la aplicación de Azure y le envía por correo electrónico si se detectan problemas.  La duración de este paso depende de la complejidad de la aplicación, y de los problemas detectados y la rapidez con la que responda ante ellos.  |
| En vivo                           | <1 día | La oferta se lanza, se replica en las regiones especificadas y se pone a disposición del público. |
|   |&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|   |

Puede supervisar el proceso de publicación en la pestaña **Status** (Estado) de la oferta en Cloud Partner Portal.

![Pestaña de estado de una oferta de aplicación de Azure](./media/offer-status-tab.png)

Cuando termina el proceso de publicación, la oferta se mostrará en la [categoría de aplicaciones de Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/).

>[!Note]
>La participación en el canal de asociados de Proveedores de soluciones en la nube (CSP) ya está disponible.  Consulte [Proveedores de soluciones en la nube](../../cloud-solution-providers.md) para obtener más información sobre el marketing que ofrece a través de los canales de asociados de CSP de Microsoft.

## <a name="errors-and-review-feedback"></a>Errores y comentarios de revisión

Además de mostrar el estado de publicación de la oferta, la pestaña **Status** (Estado) también muestra los mensajes de error y los comentarios de los pasos de publicación en los que se encontró un problema.  Si el problema es crítico, se cancelará la publicación.  A continuación, debe corregir los problemas notificados y volver a publicar la oferta.  Dado que el paso de **revisión de Microsoft** representa una revisión extensiva de la oferta y sus recursos técnicos asociados (especialmente la plantilla de Azure Resource Manager), los problemas normalmente aparecen como vínculos de solicitud de incorporación de cambios.  Para obtener una explicación sobre cómo ver y responder a estas solicitudes de incorporación de cambios, consulte [Administración de la revisión de comentarios](./cpp-handling-review-feedback.md).


## <a name="next-steps"></a>Pasos siguientes

Si encontró errores en uno o varios de los pasos de la publicación, debe corregirlos y volver a publicar su oferta.  Si se detectan problemas críticos en el paso de **revisión de Microsoft**, debe [controlar los comentarios de revisión](./cpp-handling-review-feedback.md) mediante el acceso al repositorio de Azure DevOps del equipo de revisión de Microsoft.

Una vez publicada correctamente una aplicación de Azure, puede [actualizar la oferta existente](./cpp-update-existing-offer.md) para reflejar cambios empresariales o de los requisitos técnicos. 
