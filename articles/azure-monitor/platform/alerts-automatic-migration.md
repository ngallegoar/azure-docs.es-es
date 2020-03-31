---
title: Descripción de cómo funciona el proceso de migración automática de alertas clásicas de Azure Monitor
description: Sepa cómo funciona el proceso de migración automática.
ms.topic: conceptual
ms.date: 08/19/2019
ms.subservice: alerts
ms.openlocfilehash: 8df83439d6754440648688ac1cc36ff66556a4e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77668254"
---
# <a name="understand-the-automatic-migration-process-for-your-classic-alert-rules"></a>Descripción del proceso de migración automática de reglas de alertas clásicas

Tal como se [anunció anteriormente](monitoring-classic-retirement.md), las alertas clásicas de Azure Monitor se retirarán en septiembre de 2019 (la fecha original era julio de 2019). Como parte del proceso de retirada, hay disponible una [herramienta de migración](alerts-using-migration-tool.md) en Azure Portal para que los clientes puedan desencadenar la migración ellos mismos. Si no ha usado la herramienta de migración antes del 31 de agosto de 2019, Azure Monitor iniciará el proceso de migración automática de las alertas clásicas a partir del 1 de septiembre de 2019.
Este artículo le guiará a través del proceso de migración automática y le ayudará a resolver los problemas que pueda tener.

  > [!NOTE]
  > Este artículo solo es válido en la nube pública de Azure. El proceso de retirada de alertas clásicas de Azure Monitor en la nube de Azure Government y Azure China 21Vianet se anunciará próximamente.

## <a name="what-will-happen-during-the-automatic-migration-process"></a>¿Qué ocurrirá durante el proceso de migración automática?

- A partir del **1 de septiembre de 2019**, los clientes no podrán crear reglas de alertas clásicas, excepto en [algunas métricas](alerts-understand-migration.md#classic-alert-rules-that-will-not-be-migrated).
  - En el caso de las excepciones, los clientes podrán seguir creando reglas de alertas clásicas y usar sus alertas clásicas hasta junio de 2020.
- A partir del **1 de septiembre de 2019**, la migración de alertas clásicas de todos los clientes que tengan alertas clásicas se desencadenará por lotes.
- Al igual que sucede con la herramienta de migración voluntaria, algunas reglas de alertas clásicas que no se pueden migrar se dejarán tal cual están. Estas reglas de alertas clásicas se seguirán admitiendo hasta junio de 2020. Sin embargo, las reglas de alertas clásicas no válidas se eliminarán, porque dejarán de ser funcionales.
Las reglas de alertas clásicas que supervisen recursos de destino eliminados o que remitan a [métricas que ya no se admiten](alerts-understand-migration.md#classic-alert-rules-on-deprecated-metrics) se considerarán no válidas.
- Una vez iniciada la migración de la suscripción, esta deberá completarse en un plazo de una hora (a menos que surjan problemas). Los clientes pueden supervisar el estado de la migración en la [hoja de migración en Azure Monitor](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/MigrationBladeViewModel).
- Los propietarios de la suscripción recibirán un correo electrónico cuando la migración se complete correctamente.
- Si hay algún problema durante la migración, los propietarios de la suscripción también recibirán un correo electrónico donde se les informará convenientemente. Los clientes pueden usar la hoja de migración para ver todos los detalles del problema.
- En caso de que se requiera una acción del cliente, como deshabilitar temporalmente un bloqueo de recursos o cambiar una asignación de directiva, los clientes tendrán que resolver cualquier problema existente antes del 31 de octubre de 2019. Si no se resuelven, no se puede garantizar la migración correcta de las alertas clásicas.

    > [!NOTE]
    > Si no desea esperar a que se inicie el proceso de migración automática, puede desencadenarla voluntariamente con la herramienta de migración.

## <a name="important-things-to-note"></a>Aspectos importantes que tener en cuenta

El proceso de migración convierte las reglas de alertas clásicas a las reglas de alertas nuevas equivalentes y crea grupos de acciones. En la preparación, tenga en cuenta los siguientes aspectos:

- Los formatos de carga de notificación de las nuevas reglas de alertas son diferentes de las reglas de alertas clásicas porque admiten más características. Si tiene aplicaciones lógicas, runbooks o webhooks que se desencadenan mediante una regla de alerta clásica, podrían dejar de funcionar según lo previsto una vez que se complete la migración, dadas las diferencias en las cargas de notificación. [Obtenga información sobre cómo preparar la migración](alerts-prepare-migration.md).

- Algunas reglas de alertas clásicas no se pueden migrar mediante la herramienta. [Obtenga información sobre las reglas que no se pueden migrar y qué hacer con ellas](alerts-understand-migration.md#classic-alert-rules-that-will-not-be-migrated).

    > [!NOTE]
    > El proceso de migración no afecta a la evaluación de las reglas de alertas clásicas. Seguirán ejecutándose y enviando alertas hasta que se hayan migrado y las nuevas reglas de alertas surtan efecto.

## <a name="what-if-the-automatic-migration-fails"></a>¿Qué ocurre si se produce un error en la migración automática?

Cuando se produce un error en el proceso de migración automática, los propietarios de la suscripción recibirán un correo electrónico donde se les informará del problema. Se puede usar la hoja de migración en Azure Monitor para ver todos los detalles del problema.

Consulte la [guía de solución de problemas](alerts-understand-migration.md#common-problems-and-remedies) para obtener ayuda con los problemas que se pueden producir durante la migración.

  > [!NOTE]
  > En caso de que se requiera una acción del cliente, como deshabilitar temporalmente un bloqueo de recursos o cambiar una asignación de directiva, los clientes tendrán que resolver cualquier problema existente antes del 31 de octubre de 2019. Si no se resuelven, no se puede garantizar la migración correcta de las alertas clásicas.

## <a name="next-steps"></a>Pasos siguientes

- [Preparación para la migración](alerts-prepare-migration.md)
- [Descripción del funcionamiento de la herramienta de migración](alerts-understand-migration.md)
