---
title: Diagnóstico de rendimiento para máquinas virtuales de Azure | Microsoft Docs
description: En este artículo se presenta el diagnóstico de rendimiento de Azure para Windows.
services: virtual-machines-windows'
documentationcenter: ''
author: anandhms
manager: cshepard
editor: przlplx
tags: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 9/20/2018
ms.author: anandh
ms.openlocfilehash: 04b70e593e8b1bee8beb72ac88bc8441bc38bb9a
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91963251"
---
# <a name="performance-diagnostics-for-azure-virtual-machines"></a>Diagnóstico de rendimiento para máquinas virtuales de Azure

La herramienta Performance Diagnostics le ayudará a solucionar problemas de rendimiento que pueden afectar a una máquina virtual Windows o Linux. Los escenarios para la solución de problemas admitidos incluyen operaciones para comprobar rápidamente problemas ya conocidos, procedimientos recomendados e información sobre problemas complejos, en los que se incluye un rendimiento lento de la máquina virtual, un uso excesivo de la CPU, el espacio en disco o la memoria.

Puede ejecutar el diagnóstico de rendimiento directamente desde Azure Portal, donde también puede revisar información y obtener un informe sobre diversos registros, configuraciones enriquecidas y datos de diagnóstico. Le recomendamos que ejecute el diagnóstico de rendimiento y revise la información y los datos de diagnóstico antes de ponerse en contacto con el Soporte técnico de Microsoft.

> [!NOTE]
> En Windows,Performance Diagnostics es actualmente compatible con las máquinas virtuales que tienen instalada la versión 4.5 o posterior del SDK de .NET. Para conocer los pasos que debe dar para ejecutar el diagnóstico de rendimiento en máquinas virtuales clásicas, consulte la [Azure Performance Diagnostics VM extension](performance-diagnostics-vm-extension.md) (Extensión de máquina virtual de diagnóstico de rendimiento de Azure).

## <a name="supported-operating-systems"></a>Sistemas operativos admitidos

### <a name="windows"></a>Windows

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows 10
* Windows 8.1
* Windows 8

### <a name="linux"></a>Linux

- Actualmente se admiten las siguientes distribuciones:

    | Distribución               | Versión                                         |
    |----------------------------|-------------------------------------------------|
    | Servidor de Oracle Linux        | 6.10 [`*`], 7.3, 7.6, 7.5 |
    | CentOS                     | 6.5 [`*`], 7.6                                    |
    | RHEL                       | 7.2, 7.5, 8.0 [`*`]                               |
    | Ubuntu                     | 14.04, 16.04, 18.04, 20.04                               |
    | Debian                     | 8, 9, 10 [`*`]                                    |
    | SLES                       | 12 SP4 [`*`]                                      |
    |                            |                                                   |

>[!Note]
>[`*` ] Consulte los [problemas conocidos](how-to-use-perfinsights-linux.md#known-issues).

## <a name="install-and-run-performance-diagnostics-on-your-vm"></a>Instalar y ejecutar el diagnóstico de rendimiento en la máquina virtual

Performance Diagnostics instala una extensión de máquina virtual que ejecuta una herramienta de diagnóstico denominada PerfInsights. PerfInsights está disponible tanto para [Windows](./how-to-use-perfinsights.md) como para [Linux](./how-to-use-perfinsights-linux.md). Para instalar y ejecutar el diagnóstico de rendimiento, siga estos pasos:

1. En la columna de comandos de la izquierda, seleccione **Máquinas virtuales**.
1. En la lista de nombres de máquinas virtuales, seleccione la máquina virtual en la que va a ejecutar el diagnóstico.
1. En la columna de comandos derecha, seleccione **Diagnóstico de rendimiento**.

    ![Captura de pantalla de Azure Portal, con el botón Install performance diagnostics (Instalar diagnóstico de rendimiento) resaltado](media/performance-diagnostics/performance-diagnostics-install.png)

    > [!NOTE]
    > En esta captura de pantalla, la hoja de nombres de máquinas virtuales está oculta.
1. Seleccione una cuenta de almacenamiento (opcional)

    Si quiere usar una sola cuenta de almacenamiento para almacenar los resultados de los diagnósticos de rendimiento de varias máquinas virtuales, puede seleccionar una cuenta de almacenamiento haciendo clic en el botón **Configuración** de la barra de herramientas. Haga clic en el botón **Aceptar** una vez que seleccione la cuenta de almacenamiento.

    Si no especifica una cuenta de almacenamiento, se creará una cuenta de almacenamiento nueva de forma predeterminada.

    ![Captura de pantalla de la hoja de diagnósticos de rendimiento, con el botón Configuración de la barra de herramientas resaltado](media/performance-diagnostics/settings-button.png)

    ![Captura de pantalla de la selección de cuentas de almacenamiento de la hoja de configuración de los diagnósticos de rendimiento](media/performance-diagnostics/select-storage-account.png)

1. Seleccione el botón **Install performance diagnostics** (Instalar diagnósticos de rendimiento).
1. Seleccione la casilla **Ejecutar diagnóstico** si quiere ejecutar un diagnóstico una vez completada la instalación. Si realiza esta selección, podrá elegir el escenario de análisis de rendimiento y las opciones relacionadas con el mismo.

    ![Captura de pantalla del botón de instalación de diagnósticos de rendimiento](media/performance-diagnostics/install-diagnostics-button.png)

## <a name="select-an-analysis-scenario-to-run"></a>Seleccione el escenario de análisis que se ejecutará

Los siguientes escenarios de análisis están disponibles en Azure Portal. Seleccione un análisis, en función del problema de rendimiento que tenga. Seleccione las opciones de duración y seguimiento del análisis según sea necesario.

* **Análisis rápido del rendimiento**  
    Esta opción comprueba si hay problemas conocidos, analiza los procedimientos recomendados y recopila datos de diagnóstico. Tenga en cuenta que el análisis puede tardar varios minutos en ejecutarse. Más información para [Windows](./how-to-use-perfinsights.md) o [Linux](./how-to-use-perfinsights-linux.md).

* **Análisis de rendimiento**  
    Incluye todas las comprobaciones del análisis rápido de rendimiento, y supervisa el alto consumo de recursos. Use esta versión para solucionar problemas de rendimiento generales, como el uso excesivo de la CPU, la memoria y el uso del disco. Este análisis tarda entre 30 segundos y 15 minutos en realizarse, dependiendo de la duración que haya seleccionado. Más información para [Windows](./how-to-use-perfinsights.md) o [Linux](./how-to-use-perfinsights-linux.md).

* **Análisis avanzado del rendimiento** `*`  
    Incluye todas las comprobaciones en el análisis de rendimiento y recopila uno o varios de los seguimientos, tal como se muestra en las secciones siguientes. Use este escenario para solucionar problemas complejos en los que sea necesario realizar seguimientos adicionales. Si ejecuta este escenario durante períodos más largos, aumentará el tamaño total del resultado del diagnóstico, en función del tamaño de la máquina virtual y las opciones de seguimiento que se hayan seleccionado. Este análisis tarda entre 30 segundos y 15 minutos en ejecutarse, dependiendo de la duración que haya seleccionado. [Más información](./how-to-use-perfinsights.md)

* **Análisis de Azure Files** `*`  
    Incluye todas las comprobaciones en el análisis de rendimiento y captura el seguimiento de red y los contadores SMB. Use este escenario para solucionar los problemas de rendimiento de Azure Files. Este análisis tarda entre 30 segundos y 15 minutos en ejecutarse, dependiendo de la duración que haya seleccionado. [Más información](./how-to-use-perfinsights.md)

>[!Note]
>[`*`] Estos escenarios de análisis solo se admiten en Windows.

![Captura de pantalla del panel Ejecutar diagnóstico de la hoja de diagnósticos de rendimiento](media/performance-diagnostics/run-diagnostics-pane.png)

### <a name="provide-symptoms-optional"></a>Proporcione los síntomas (opcional)

Seleccione cualquier síntoma preseleccionado de la lista, o agregue nuevos síntomas. Gracias a ello, podremos mejorar el análisis en el futuro.

### <a name="provide-support-request-number-if-available-optional"></a>Proporcione el número de solicitud de soporte, si está disponible (opcional)

Si está trabajando con un ingeniero del soporte técnico de Microsoft en una incidencia de soporte técnico existente, proporcione el número de dicha incidencia de soporte técnico.

### <a name="review-the-privacy-policy-and-legal-terms-and-select-the-check-box-to-acknowledge-required"></a>Lea la política de privacidad y los términos legales, y seleccione la casilla para confirmar que los ha leído (obligatorio)

Para ejecutar el diagnóstico, debe aceptar los términos legales y aceptar la política de privacidad.

### <a name="select-ok-to-run-the-diagnostics"></a>Seleccione Aceptar para ejecutar el diagnóstico.

Se mostrará una notificación cuando se inicie la instalación de los diagnósticos de rendimiento. Una vez completada la instalación, verá una notificación que indica que la instalación se realizó correctamente. Asimismo, el análisis seleccionado se ejecutará en el tiempo especificado. Este es un buen momento para reproducir el problema de rendimiento, y que así los datos de diagnóstico se puedan capturar en el momento correcto.

Una vez finalizado el análisis, los siguientes elementos se cargan en las tablas de Azure, así como un contenedor de objetos binarios de gran tamaño (BLOB) en la cuenta de almacenamiento especificada:

* Todos los detalles y la información relacionada sobre la ejecución.
* Un archivo comprimido (. zip) de salida (denominado **PerformanceDiagnostics_aaaa-MM-dd_hh-mm-ss-fff.zip**) en Windows y un archivo tarball (denominado **PerformanceDiagnostics_aaaa-MM-dd_hh-mm-ss-fff.tar.gz**) en Linux que contiene archivos de registro.
* Un informe de HTML.

Después de la carga, aparece un nuevo informe de diagnóstico en Azure Portal.

![Captura de pantalla de una lista de informes de diagnóstico en hoja de diagnósticos de rendimiento](media/performance-diagnostics/diagnostics-report-list.png)

## <a name="how-to-change-performance-diagnostics-settings"></a>Cómo cambiar la configuración del diagnóstico de rendimiento

Use el botón de la barra de herramientas **Configuración** para cambiar la cuenta de almacenamiento donde se pueden almacenar los detalles y los resultados de los diagnósticos. Puede usar la misma cuenta de almacenamiento para varias máquinas virtuales que usen la opción de diagnóstico de rendimiento. Recuerde que si cambia la cuenta de almacenamiento, los informes y los detalles anteriores no se eliminarán. Sin embargo, ya no se mostrarán en la lista de informes de diagnóstico.

## <a name="review-insights-and-performance-diagnostics-report"></a>Revisar las conclusiones y el informe de diagnóstico de rendimiento

Cada vez que se ejecuta un diagnóstico, este devuelve una lista de conclusiones y recomendaciones, los recursos afectados, los archivos de registro y otra información de diagnóstico enriquecida que se haya recopilado, además de un informe para poder ver estos datos sin conexión. Para obtener una lista completa de todos los datos de diagnóstico recopilados, consulte **¿Qué tipo de información recopila PerfInsights?** en [Windows](how-to-use-perfinsights.md#what-kind-of-information-is-collected-by-perfinsights) o [Linux](how-to-use-perfinsights-linux.md#what-kind-of-information-is-collected-by-perfinsights).

### <a name="select-a-performance-diagnostics-report"></a>Seleccione un informe de diagnóstico de rendimiento

Puede usar la lista de informes de diagnóstico para buscar todos los informes de diagnóstico que se ejecutaron. En esta lista se incluyen los detalles sobre el análisis que se utilizó, las conclusiones que se sacaron y sus niveles de impacto. Seleccione una fila para ver más detalles.

![Captura de pantalla de la selección de un informe de diagnóstico de la hoja de diagnósticos de rendimiento](media/performance-diagnostics/select-report.png)

### <a name="review-a-performance-diagnostics-report"></a>Revise un informe de diagnósticos de rendimiento

Cada informe de diagnóstico de rendimiento puede contener varias conclusiones e indicar un nivel de impacto Alto, Medio o Bajo. Cada conclusión también contiene recomendaciones para ayudarle a reducir el problema. Asimismo, las conclusiones se agrupan para facilitar el filtrado.

Los niveles de impacto representan la posibilidad de que surjan problemas de rendimiento, en función de factores tales como una configuración incorrecta, problemas conocidos o problemas que notifican otros usuarios. Es posible que aún no haya visto o experimentado uno o más de los problemas que se enumeran. Por ejemplo, puede tener archivos de registro de SQL y archivos de base de datos en el mismo disco de datos. Esta condición tiene un alto riesgo de crear cuellos de botella y otros problemas de rendimiento si el uso de la base de datos es alto; además, es posible que no note un problema si el uso de la base de datos es bajo.

![Captura de pantalla de la hoja de información general del informe de diagnóstico de rendimiento](media/performance-diagnostics/performance-diagnostics-report-overview.png)

### <a name="reviewing-performance-diagnostics-insights-and-recommendations"></a>Revisar las conclusiones y recomendaciones de los diagnósticos de rendimiento

Puede seleccionar una conclusión para ver más detalles sobre los recursos que se han visto afectados, las mitigaciones sugeridas y los vínculos de referencia.

![Captura de pantalla de un detalle de la conclusión correspondiente al diagnóstico de rendimiento](media/performance-diagnostics/insight-detail.png)

### <a name="download-and-review-the-full-performance-diagnostics-report"></a>Descargar y revisar el informe de diagnósticos de rendimiento completo

Puede usar el botón **Descargar informe** para descargar un informe HTML que contenga información enriquecida y adicional acerca del diagnóstico como, por ejemplo, el almacenamiento y la configuración de la red, los contadores de rendimiento, los seguimientos realizados, la lista de procesos y los registros. El contenido dependerá del análisis que seleccione. Para la solución avanzada de problemas, el informe puede contener información adicional y gráficos interactivos relacionados con el uso elevado de la CPUo el disco y los procesos que consumen demasiada memoria. Para obtener más información sobre el informe de Performance Diagnostics, consulte [Windows](how-to-use-perfinsights.md#review-the-diagnostics-report) o [Linux](how-to-use-perfinsights-linux.md#review-the-diagnostics-report).

## <a name="manage-performance-diagnostics-reports"></a>Gestionar informes de diagnósticos de rendimiento

Puede eliminar uno o más informes de diagnósticos de rendimiento si usa el botón **Eliminar informe**.

## <a name="how-to-uninstall-performance-diagnostics"></a>Cómo desinstalar diagnósticos de rendimiento

Puede desinstalar los diagnósticos de rendimiento de una máquina virtual. Esta acción elimina la extensión de máquina virtual, pero no afecta a ningún dato de diagnóstico que se encuentre en la cuenta de almacenamiento.

![Captura de pantalla de la barra de herramientas de la hoja de diagnóstico de rendimiento con el botón Desinstalar resaltado](media/performance-diagnostics/uninstal-button.png)

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="where-is-the-diagnostics-data-from-my-vm-stored"></a>¿Dónde se almacenan los datos de diagnóstico de mi máquina virtual?

Todos los informes y conclusiones de los diagnósticos de rendimiento se almacenan en su propia cuenta de almacenamiento. Las conclusiones se almacenan en las tablas de Azure. El archivo comprimido de informes se almacena en un contenedor de objetos binarios grandes (BLOB) denominado azdiagextnresults.

Puede ver la información de la cuenta de almacenamiento si usa el botón Configuración en la barra de herramientas.

### <a name="how-do-i-share-this-data-with-microsoft-customer-support"></a>¿Cómo comparto estos datos con el soporte técnico de Microsoft?

Existen varias formas de compartir el informe de diagnóstico con Microsoft.

**Opción 1:** compartir automáticamente el último informe.  
Al abrir una incidencia de soporte técnico con Microsoft, es importante compartir el informe de diagnóstico de rendimiento. Si optó por compartir esta información con Microsoft mientras ejecuta los diagnósticos (al seleccionar la casilla "**Accedo a compartir la información de diagnóstico con Microsoft**"), Microsoft podrá obtener acceso al informe desde su cuenta de almacenamiento mediante un vínculo SAS, que le llevará al archivo .zip de salida; este archivo se guardará un máximo de 30 días a partir de la fecha de ejecución. Recuerde que solo el último informe está disponible para que el ingeniero del soporte técnico pueda acceder al mismo.

**Opción 2:** generar una Firma de acceso compartido para el archivo comprimido del informe de diagnóstico.  
Puede compartir un vínculo al archivo comprimido de informes si usa las firmas de acceso compartido. Para ello, siga estos pasos.

1. En Azure Portal, busque la cuenta de almacenamiento en la que se almacenan los datos de diagnóstico.
1. Seleccione **Blob** en la sección **Blob service**.
1. Seleccione el contenedor **azdiagextnresults**.
1. Seleccione el archivo comprimido de salida correspondiente al diagnóstico de rendimiento que quiera compartir.
1. En la pestaña **Generar SAS**, seleccione los criterios para compartir.
1. Haga clic en **Generate blob SAS token and URL** (Generar token SAS de blob y URL).
1. Copie la **dirección URL del blob SAS** y compártala con el ingeniero del soporte técnico.

**Opción 3:** descargar el informe de la cuenta de almacenamiento.

También puede buscar el archivo comprimido del informe de diagnóstico de rendimiento mediante los pasos 1 a 4 de la opción 2. Seleccione la opción para descargar el archivo y compártalo por correo electrónico, o solicite instrucciones al ingeniero del soporte técnico para que cargue el archivo.  

### <a name="how-do-i-capture-the-diagnostics-data-at-the-correct-time"></a>¿Cómo capturo los datos de diagnóstico en el momento correcto?

Cada ejecución de diagnóstico de rendimiento tiene dos fases:

1. Instalar o actualizar la extensión de la máquina virtual del diagnóstico de rendimiento.
1. Ejecutar los diagnósticos durante el tiempo especificado.

Actualmente no hay una manera fácil de saber exactamente cuándo se completa la instalación de la extensión de máquina virtual. En general, se tarda entre 45 segundos y 1 minuto en instalar la extensión de la máquina virtual. Después de instalar la extensión de la máquina virtual, puede ejecutar los pasos de reproducción para que los diagnósticos de rendimiento capturen el conjunto correcto de datos para la solución de problemas.

## <a name="next-steps"></a>Pasos siguientes

Después de revisar las conclusiones y los informes de los diagnósticos de rendimiento, si aún no puede determinar la causa del problema y necesita más ayuda, puede abrir una incidencia de soporte técnico con el servicio de soporte técnico de Microsoft.

Si necesita más ayuda con cualquier aspecto de este artículo, puede ponerse en contacto con los expertos de Azure en los [foros de MSDN Azure o Stack Overflow](https://azure.microsoft.com/support/forums/). Como alternativa, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte técnico**. Para más información sobre el uso del soporte técnico de Azure, lea las [preguntas más frecuentes](https://azure.microsoft.com/support/faq/).