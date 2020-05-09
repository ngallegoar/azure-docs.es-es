---
title: Alertas de registro de actividad en Azure Monitor
description: Reciba notificaciones por SMS, webhook y correo electrónico, entre otros, cuando se produzcan determinados eventos en el registro de actividad.
ms.subservice: alerts
ms.topic: conceptual
ms.date: 09/17/2018
ms.openlocfilehash: 1a932aba55ec9bd5d92c60338a3c1fc4bb481c1b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82137712"
---
# <a name="alerts-on-activity-log"></a>Alertas de registro de actividad

## <a name="overview"></a>Información general

Las alertas del registro de actividad son alertas que se activan cuando un nuevo [evento del registro de actividad](activity-log-schema.md) cumple las condiciones especificadas en la alerta. Según el orden y el volumen de los eventos registrados en el [registro de actividad de Azure](platform-logs-overview.md), se activará la regla de alerta. Las regla de alertas del registro de actividad son recursos de Azure, por lo que pueden crearse con una plantilla de Azure Resource Manager. También se pueden crear, actualizar o eliminar en Azure Portal. Este artículo presenta los conceptos relativos a las alertas del registro de actividad. Para más información sobre cómo crear o usar reglas de alertas de registro de actividad, consulte [Creación y administración de alertas del registro de actividad](alerts-activity-log.md).

> [!NOTE]
> **No se pueden** crear alertas para eventos en la categoría Alerta del registro de actividad.

Por lo general, se crean alertas del registro de actividad para recibir notificaciones cuando:

* Se produzcan operaciones específicas en los recursos de la suscripción de Azure, que abarcan normalmente grupos de recursos o recursos en particular. Por ejemplo, si quiere que le notifiquen cuando se elimine alguna máquina virtual de myProductionResourceGroup. O, podría querer recibir una notificación si se asigna algún rol nuevo a un usuario de la suscripción.
* Se produce un evento de mantenimiento del servicio. Los eventos de mantenimiento del servicio incluyen la notificación de incidentes y eventos de mantenimiento que se aplican a recursos de la suscripción.

Una simple analogía para comprender las condiciones en las que se pueden crear reglas de alertas en el registro de actividad es explorar o filtrar eventos a través del [Registro de actividad en Azure Portal](activity-log-view.md#azure-portal). En "Azure Monitor: registro de actividad" se puede filtrar o buscar un evento necesario y crear una alerta mediante el botón **Agregar alerta de registro de actividad**.

En cualquier caso, una alerta del registro de actividad solo supervisa eventos de la suscripción en la que se ha creado la alerta.

Puede configurar una alerta del registro de actividad según las propiedades de nivel superior del objeto JSON de un evento del registro de actividad. Para obtener más información, consulte [Categorías del Registro de actividad](activity-log-view.md#categories-in-the-activity-log). Para más información acerca de los eventos de mantenimiento del servicio, consulte [Recibir alertas del registro de actividad con las notificaciones del servicio](alerts-activity-log-service-notifications.md). 

Las alertas del registro de actividad tienen algunas opciones en común:

- **Categoría**: Administración, Mantenimiento del servicio, Escalado automático, Seguridad, Directiva y Recomendación. 
- **Ámbito**: el recurso individual o conjunto de recursos para el que se define la alerta de registro de actividad. El ámbito de una alerta de registro de actividad se puede definir en distintos niveles:
    - Nivel de recurso: por ejemplo, para una máquina virtual concreta
    - Nivel de grupo de recursos: por ejemplo, todas las máquinas virtuales de un grupo de recursos específico
    - Nivel de suscripción: por ejemplo, todas las máquinas virtuales de una suscripción (o) todos los recursos de una suscripción
- **Grupo de recursos**: de forma predeterminada, la regla de alertas se guarda en el grupo de recursos que tiene el destino definido en el ámbito. El usuario también puede definir el grupo de recursos donde se debe almacenar la regla de alertas.
- **Tipo de recurso**: Resource Manager ha definido el espacio de nombres para el destino de la alerta.
- **Nombre de la operación**: Nombre de la [operación de Azure Resource Manager](../../role-based-access-control/resource-provider-operations.md) utilizado por el control de acceso basado en rol. No se pueden usar operaciones no registradas con Azure Resource Manager en una regla de alerta de registro de actividad.
- **Nivel**: el nivel de gravedad del evento (informativo, advertencia, error o crítico).
- **Estado**: el estado del evento, normalmente Iniciado, Error o Correcto.
- **Evento iniciado por**: También se denomina "llamador". La dirección de correo electrónico o el identificador de Azure Active Directory del usuario que realizó la operación.

> [!NOTE]
> En una suscripción se pueden crear hasta 100 reglas de alertas para una actividad del ámbito en el nivel: de un único recurso, de todos los recursos de un grupo de recursos (o) de suscripción completa.

Cuando se activa una alerta del registro de actividad, usa un grupo de acciones para generar acciones o notificaciones. Un grupo de acciones es un conjunto reutilizable de destinatarios de notificación, como direcciones de correo electrónico, direcciones URL del webhook o números de teléfono para SMS. Se puede hacer referencia a los receptores desde varias alertas para centralizar y agrupar los canales de notificación. Al definir la alerta del registro de actividad, tiene dos opciones. Puede:

* Usar un grupo de acciones existente en la alerta del registro de actividad.
* Crear un nuevo grupo de acciones.

Para más información sobre los grupos de acciones, consulte [Creación y administración de grupos de acciones en Azure Portal](action-groups.md).


## <a name="next-steps"></a>Pasos siguientes

- Obtener una [Introducción a las alertas](alerts-overview.md).
- Obtenga información sobre la [creación y modificación de las alertas del registro de actividad](alerts-activity-log.md).
- Revise el [Esquema de webhook de alertas del registro de actividad](activity-log-alerts-webhook.md).
- Más información acerca de las [Notificaciones del estado del servicio](service-notifications.md).
