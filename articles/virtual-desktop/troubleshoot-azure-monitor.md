---
title: Solución de problemas de Azure Monitor para Windows Virtual Desktop (versión preliminar) - Azure
description: Solución de problemas con Azure Monitor para Windows Virtual Desktop.
author: Heidilohr
ms.topic: troubleshooting
ms.date: 12/01/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 91cf6729911cdb674c5451f172e76a2e9d5943e4
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96465870"
---
# <a name="troubleshoot-azure-monitor-for-windows-virtual-desktop-preview"></a>Solución de problemas de Azure Monitor para Windows Virtual Desktop (versión preliminar)

>[!IMPORTANT]
>Azure Monitor para Windows Virtual Desktop se encuentra actualmente en versión preliminar pública. Esta versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se presentan problemas conocidos y soluciones para problemas comunes en Azure Monitor para Windows Virtual Desktop (versión preliminar).

## <a name="the-configuration-workbook-isnt-working-properly"></a>El libro de configuración no funciona correctamente

Si el libro de configuración de Azure Monitor no funciona, puede usar los siguientes recursos para configurar manualmente sus componentes:

- Para habilitar manualmente los diagnósticos o acceder al área de trabajo de Log Analytics, consulte [Envío de diagnósticos de Windows Virtual Desktop a Log Analytics](diagnostics-log-analytics.md).
- Para instalar manualmente la extensión de Log Analytics en un host, consulte [Extensión de máquina virtual de Log Analytics para Windows](../virtual-machines/extensions/oms-windows.md).
- Para configurar una nueva área de trabajo de Log Analytics, consulte [Creación de un área de trabajo de Log Analytics en Azure Portal](../azure-monitor/learn/quick-create-workspace.md).
- Para obtener más información sobre cómo leer contadores de rendimiento, consulte [Configuración de contadores de rendimiento](../azure-monitor/platform/data-sources-performance-counters.md).
- Para configurar eventos para un área de trabajo de Log Analytics, consulte [Recopilación de orígenes de datos del registro de eventos de Windows con el agente de Log Analytics](../azure-monitor/platform/data-sources-windows-events.md).

Como alternativa, el problema puede deberse a la falta de recursos o a que no tiene los permisos necesarios.

Si la suscripción no tiene ningún recurso de Windows Virtual Desktop, no se mostrará en el parámetro *Subscription*.

Si no tiene acceso de lectura a las suscripciones correctas, no se mostrarán en el parámetro *Subscription* y no se verán sus datos en el panel. Para solucionar este problema, póngase en contacto con el propietario de la suscripción y solicite acceso de lectura.

## <a name="my-data-isnt-displaying-properly"></a>Los datos no se muestran correctamente

Si los datos no se muestran correctamente, puede que se haya producido algún error durante el proceso de configuración de Azure Monitor. En primer lugar, asegúrese de que ha rellenado todos los campos del libro de configuración tal como se describe en [Uso de Azure Monitor para Windows Virtual Desktop para la supervisión de implementaciones](azure-monitor.md). Puede cambiar la configuración de los entornos nuevos y existentes en cualquier momento. Si faltan contadores o eventos, los datos asociados a ellos no se mostrarán en Azure Portal.

Si no le falta información, pero los datos todavía no se muestran correctamente, puede haber un problema en la consulta o en los orígenes de datos. 

Si no ve ningún error de instalación y aún no se muestran los datos esperados, se recomienda esperar 15 minutos y actualizar la fuente. Azure Monitor tiene un período de latencia de 15 minutos para rellenar los datos de registro. Para más información, consulte [Tiempo de la ingesta de datos de registro en Azure Monitor](../azure-monitor/platform/data-ingestion-time.md).

Por último, si no le falta información, pero los datos siguen sin mostrarse, puede que haya un problema en la consulta o en los orígenes de datos. En ese caso, debería ponerse en contacto con el servicio de soporte técnico para resolver el problema.

## <a name="i-want-to-customize-azure-monitor-for-windows-virtual-desktop"></a>Quiero personalizar Azure Monitor para Windows Virtual Desktop

Azure Monitor para Windows Virtual Desktop usa los libros de Azure Monitor. Los libros permiten guardar una copia de la plantilla de libro de Windows Virtual Desktop y realizar sus propias personalizaciones.

Las plantillas personalizadas no se actualizan cuando el grupo de productos actualiza la plantilla original. Este comportamiento es así por diseño en la herramienta de libros; tendrá que guardar una copia de la plantilla actualizada y volver a realizar las personalizaciones para adoptar actualizaciones. Para obtener más información, consulte [Solución de problemas de conclusiones basadas en libros](../azure-monitor/insights/troubleshoot-workbooks.md) y la [información general sobre libros](../azure-monitor/platform/workbooks-overview.md).

## <a name="i-cant-interpret-the-data"></a>No puedo interpretar los datos

Obtenga más información sobre los términos de datos en el [glosario de Azure Monitor para Windows Virtual Desktop](azure-monitor-glossary.md).

## <a name="the-data-i-need-isnt-available"></a>Los datos que necesito no están disponibles

¿No encuentra un punto de datos para diagnosticar un problema? ¡Envíenos sus comentarios!

- Para obtener información sobre cómo dejar comentarios, consulte [Información general, comentarios y soporte técnico para la solución de problemas de Windows Virtual Desktop](troubleshoot-set-up-overview.md).
- También puede dejar comentarios sobre Windows Virtual Desktop en el [Centro de opiniones de Windows Virtual Desktop](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app) o en nuestro [foro UserVoice](https://windowsvirtualdesktop.uservoice.com/forums/921118-general).

## <a name="known-issues"></a>Problemas conocidos

Estos son los problemas que conocemos actualmente y estamos trabajando para solucionarlos:

- Actualmente, solo puede supervisar una suscripción, un grupo de recursos y un grupo de hosts a la vez. Por este motivo, al usar la página Informes de usuario para comprender la experiencia de un usuario, debe comprobar que tiene el grupo de hosts correcto que el usuario ha estado usando; de lo contrario, sus datos no rellenarán los elementos visuales.

- Actualmente no es posible guardar su configuración favorita en Azure Monitor a menos que guarde una plantilla personalizada del libro. Esto significa que los administradores de TI tendrán que escribir el nombre de la suscripción, los nombres de los grupos de recursos y las preferencias del grupo de hosts cada vez que abran Azure Monitor para Windows Virtual Desktop.

- Actualmente no hay ninguna manera de exportar los datos de Azure Monitor para Windows Virtual Desktop a Excel.

- Todas las alertas de gravedad 1 de Azure Monitor para todos los productos de la suscripción seleccionada aparecerán en la página Información general. Esto es así por diseño, ya que las alertas de otros productos de la suscripción pueden afectar a Windows Virtual Desktop. En este momento, la consulta se limita a las alertas de gravedad 1, por lo que se excluyen las alertas de gravedad 0 de alta prioridad de la página Información general.

- Algunos mensajes de error no están escritos de manera descriptiva y no todos los mensajes de error están descritos en la documentación.

## <a name="next-steps"></a>Pasos siguientes

Si no está seguro de cómo interpretar los datos o quiere obtener más información sobre los términos comunes, consulte el [glosario de Azure Monitor para Windows Virtual Desktop](azure-monitor-glossary.md). Si quiere obtener información sobre cómo configurar y usar Azure Monitor para Windows Virtual Desktop, consulte [Uso de Azure Monitor para Windows Virtual Desktop para la supervisión de implementaciones](azure-monitor.md).