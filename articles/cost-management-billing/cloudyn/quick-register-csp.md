---
title: Registro mediante la información de asociado de CSP con Cloudyn en Azure
description: Obtenga información detallada sobre el proceso de registro que usan los asociados para incorporar sus clientes en el portal de Cloudyn.
author: bandersmsft
ms.author: banders
ms.date: 10/23/2020
ms.topic: quickstart
ms.custom: seodec18
ms.service: cost-management-billing
ms.subservice: cloudyn
ms.reviewer: benshy
ROBOTS: NOINDEX
ms.openlocfilehash: 95d8a425314ad1c968915085b35ca89aa3f62711
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2020
ms.locfileid: "92543328"
---
# <a name="register-with-the-csp-partner-program-and-view-cost-data"></a>Registrarse con el programa de asociados de CSP y ver los datos de costo

Como asociado de CSP y usuario registrado de Cloudyn, puede ver y analizar el gasto en la nube en Cloudyn. [Azure Cost Management está disponible de forma nativa para los asociados directos](../costs/get-started-partners.md) que hayan incorporado sus clientes a un Contrato de cliente de Microsoft y hayan adquirido un plan de Azure.

Este registro le proporciona acceso al portal de Cloudyn. En esta guía de inicio rápido se detalla el proceso de registro necesario para crear una suscripción de evaluación de Cloudyn e iniciar sesión en el portal de Cloudyn.

[!INCLUDE [cloudyn-note](../../../includes/cloudyn-note.md)]

## <a name="configure-indirect-csp-access-in-cloudyn"></a>Configuración del acceso al CSP indirecto en Cloudyn

De forma predeterminada, Partner Center API solo es accesible para los CSP directos. Sin embargo, un proveedor de CSP directo puede configurar el acceso para sus clientes o asociados de CSP indirecto mediante grupos de entidades de Cloudyn.

Para habilitar el acceso a los clientes o asociados de CSP indirectos, complete los pasos siguientes para segmentar los datos de CSP indirectos mediante grupos de entidades de Cloudyn. Seguidamente, asigne los permisos de usuario correspondientes a los grupos de entidades.

1. Cree un grupo de entidades con la información en [Crear entidades](tutorial-user-access.md#create-and-manage-entities).
2. Siga los pasos que se describen en [Assigning subscriptions to Cost Entities](https://www.youtube.com/watch?v=d9uTWSdoQYo) (Asignación de suscripciones a entidades de costo). Asocie la cuenta del cliente de CSP indirecto y sus suscripciones de Azure a la entidad que creó anteriormente.
3. Siga los pasos que se describen en [Crear un usuario con acceso de administrador](tutorial-user-access.md#create-a-user-with-admin-access) para crear una cuenta de usuario con acceso de administrador. A continuación, asegúrese de que la cuenta de usuario tenga acceso de administrador a las entidades específicas que creó anteriormente para la cuenta indirecta.

Los asociados de CSP indirecto inician sesión en el portal de Cloudyn con las cuentas que ha creado para ellos.


[!INCLUDE [cost-management-create-account-view-data](../../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, se usa la información de CSP para registrarse en Cloudyn. También se inicia sesión en el portal de Cloudyn para que pueda empezar a visualizar los datos de costo. Para más información sobre Cloudyn, continúe con el tutorial de Cloudyn.

> [!div class="nextstepaction"]
> [Revisión del uso y los costos](tutorial-review-usage.md)