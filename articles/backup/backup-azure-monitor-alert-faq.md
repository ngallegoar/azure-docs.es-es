---
title: Preguntas más frecuentes sobre la supervisión de alertas e informes
description: En este artículo, encontrará respuestas a preguntas habituales sobre los informes de Azure Backup Monitoring Alert y Azure Backup.
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 07/08/2019
ms.openlocfilehash: f5be97458ba658f315c31ae34e540842b64e3ec4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76989576"
---
# <a name="azure-backup-monitoring-alert---faq"></a>Preguntas frecuentes sobre las alertas de supervisión de Azure Backup

En este artículo se responde a preguntas comunes sobre la supervisión y la creación de informes de Azure Backup.

## <a name="configure-azure-backup-reports"></a>Configuración de informes de Azure Backup

### <a name="how-do-i-check-if-reporting-data-has-started-flowing-into-a-log-analytics-la-workspace"></a>¿Cómo se comprueba que los datos de informes han empezado a fluir a un área de trabajo de Log Analytics (LA)?

Vaya al área de trabajo de LA que ha configurado y desplácese hasta el elemento de menú **Logs** (Registros) y ejecute la consulta CoreAzureBackup | take 1. Si observa que se devuelve un registro, significa que los datos han empezado a fluir al área de trabajo. La inserción de datos inicial puede tardar hasta 24 horas.

### <a name="what-is-the-frequency-of-data-push-to-an-la-workspace"></a>¿Cuál es la frecuencia de inserción de datos en un área de trabajo de LA?

Los datos de diagnóstico del almacén se bombean al área de trabajo de Log Analytics con cierto retraso. Cada evento llega al área de trabajo de Log Analytics entre 20 y 30 minutos después de insertarse desde el almacén de Recovery Services. Estos son detalles adicionales sobre el retardo:

* En todas las soluciones, las alertas integradas del servicio de copia de seguridad se insertan en cuanto se crean. Así pues, suelen aparecer en el área de trabajo de Log Analytics tras haber transcurrido de 20 a 30 minutos.
* En todas las soluciones, los trabajos de copia de seguridad a petición y los trabajos de restauración se insertan en cuanto finalizan.
* En todas las soluciones salvo la copia de seguridad de SQL, los trabajos de copia de seguridad programados se insertan en cuanto finalizan.
* En el caso de la copia de seguridad de SQL, como pueden realizarse copias de seguridad de registros cada 15 minutos, de todos los trabajos de copia de seguridad programados completados, incluidos los registros, la información se une en lotes y se inserta cada 6 horas.
* En todas las soluciones, el resto de la información, como el elemento de copia de seguridad, la directiva, los puntos de recuperación, el almacenamiento, etc. se inserta al menos una vez al día.
* Un cambio en la configuración de copia de seguridad (como por ejemplo un cambio de directiva o la edición de la directiva) desencadena la inserción de toda la información de copia de seguridad relacionada.

### <a name="how-long-can-i-retain-reporting-data"></a>¿Cuánto tiempo se pueden conservar los datos de informes?

Después de crear un área de trabajo de LA, puede elegir conservar los datos durante dos años como máximo. De forma predeterminada, un área de trabajo de LA mantiene los datos durante 31 días.

### <a name="will-i-see-all-my-data-in-reports-after-i-configure-the-la-workspace"></a>¿Aparecerán todos los datos en los informes después de configurar el área de trabajo de LA?

 Todos los datos generados después de configurar los valores de diagnóstico se insertan en el área de trabajo de LA y están disponible en los informes. Los trabajos en curso no se insertan para los informes. Después de que el trabajo termina o genera errores, se envía a los informes.

### <a name="can-i-view-reports-across-vaults-and-subscriptions"></a>¿Se pueden ver los informes en los almacenes y las suscripciones?

Sí, puede ver informes de todos los almacenes y suscripciones, así como de todas las regiones. Los datos pueden residir en una o varias áreas de trabajo de LA.

### <a name="can-i-view-reports-across-tenants"></a>¿Puedo ver informes de todos los inquilinos?

Si es usuario de [Azure Lighthouse](https://azure.microsoft.com/services/azure-lighthouse/) con acceso delegado a las suscripciones o las áreas de trabajo de LA de sus clientes, puede usar informes de copia de seguridad para ver los datos de todos los inquilinos.

### <a name="how-long-does-it-take-for-the-azure-backup-agent-job-status-to-reflect-in-the-portal"></a>¿Cuánto tarda el estado del trabajo del agente de copia de seguridad de Azure en reflejarse en el portal?

Azure Portal puede tardar hasta 15 minutos en reflejar el estado del trabajo del agente de copia de seguridad de Azure.

### <a name="when-a-backup-job-fails-how-long-does-it-take-to-raise-an-alert"></a>Cuando se produce un error en un trabajo de copia de seguridad, ¿cuánto tarda en generarse una alerta?

Se genera una alerta en menos de 20 minutos desde que se produce el error en la copia de seguridad de Azure.

### <a name="is-there-a-case-where-an-email-wont-be-sent-if-notifications-are-configured"></a>¿Hay algún caso en el que no se envíe ningún correo electrónico si se configuran las notificaciones?

Sí. En las siguientes situaciones no se envían notificaciones.

* Si se configuran las notificaciones por horas y una alerta se genera y se resuelve en menos de una hora
* Cuando se cancela un trabajo
* Si se produce un error en el segundo trabajo de copia de seguridad porque el original está en curso

## <a name="recovery-services-vault"></a>Almacén de Recovery Services

### <a name="how-long-does-it-take-for-the-azure-backup-agent-job-status-to-reflect-in-the-portal"></a>¿Cuánto tarda el estado del trabajo del agente de copia de seguridad de Azure en reflejarse en el portal?

Azure Portal puede tardar hasta 15 minutos en reflejar el estado del trabajo del agente de copia de seguridad de Azure.

### <a name="when-a-backup-job-fails-how-long-does-it-take-to-raise-an-alert"></a>Cuando se produce un error en un trabajo de copia de seguridad, ¿cuánto tarda en generarse una alerta?

Se genera una alerta en menos de 20 minutos desde que se produce el error en la copia de seguridad de Azure.

### <a name="is-there-a-case-where-an-email-wont-be-sent-if-notifications-are-configured"></a>¿Hay algún caso en el que no se envíe ningún correo electrónico si se configuran las notificaciones?

Sí. En las siguientes situaciones no se envían notificaciones:

* Si se configuran las notificaciones por horas y una alerta se genera y se resuelve en menos de una hora
* Cuando se cancela un trabajo
* Si se produce un error en el segundo trabajo de copia de seguridad porque el original está en curso

## <a name="next-steps"></a>Pasos siguientes

Lea las otras preguntas más frecuentes:

* [Preguntas comunes](backup-azure-vm-backup-faq.md) sobre las copias de seguridad de máquinas virtuales de Azure
* [Preguntas comunes](backup-azure-file-folder-backup-faq.md) sobre el agente de Azure Backup
