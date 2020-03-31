---
title: Galerías de runbooks y módulos para Azure Automation
description: Dispone de runbooks y módulos de Microsoft y la comunidad para instalarlos y usarlos en su entorno de Azure Automation.  En este artículo, se describe cómo acceder a estos recursos y contribuir sus runbooks a la galería.
services: automation
ms.subservice: process-automation
ms.date: 03/20/2019
ms.topic: conceptual
ms.openlocfilehash: 52a0ab0a31600c1548283c7d899b17e497811b5a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75421484"
---
# <a name="runbook-and-module-galleries-for-azure-automation"></a>Galerías de runbooks y módulos para Azure Automation

En lugar de crear sus propios runbooks y módulos en Azure Automation, dispone de acceso a escenarios ya compilados por Microsoft y la comunidad.

Puede obtener runbooks y [módulos](#modules-in-powershell-gallery) de PowerShell de la Galería de PowerShell y [runbooks de Python](#python-runbooks) de la galería del Centro de scripts. También puede compartir los escenarios que desarrolle para contribuir a la comunidad; para ello, consulte Adición de un runbook a la Galería de runbooks.

## <a name="runbooks-in-powershell-gallery"></a>Runbooks en la Galería de PowerShell

La [Galería de PowerShell](https://www.powershellgallery.com/packages) ofrece diversos runbooks de Microsoft y la comunidad que se pueden importar a Azure Automation. Puede usar uno al descargarlo de la galería, o puede importar runbooks directamente desde la galería o de la cuenta de Automation de Azure Portal.

Solo se puede importar directamente desde la Galería de PowerShell mediante Azure Portal. No se puede llevar a cabo esta función mediante PowerShell.

> [!NOTE]
> Debería validar el contenido de los runbooks que obtenga de la Galería de PowerShell y extremar el cuidado al instalarlos y ejecutarlos en un entorno de producción.

### <a name="to-import-a-powershell-runbook-from-the-runbook-gallery-with-the-azure-portal"></a>Para importar un runbook de PowerShell de la Galería de runbooks con Azure Portal

1. En Azure Portal, abra su cuenta de Automation.
2. En **Automatización de procesos**, haga clic en **Galería de runbooks**
3. Seleccione **Origen: Galería de PowerShell**.
4. Busque el elemento de la galería que desee y selecciónelo para ver sus detalles. En la parte izquierda puede especificar otros parámetros de búsqueda para el anunciante y el tipo.

   ![Examinar galería](media/automation-runbook-gallery/browse-gallery.png)

5. Haga clic en **Ver proyecto de origen** para ver el elemento en el [Centro de scripts de TechNet](https://gallery.technet.microsoft.com/).
6. Para importar un elemento, haga clic en él para ver sus detalles y después haga clic en el botón **Importar** .

   ![Botón Importar](media/automation-runbook-gallery/gallery-item-detail.png)

7. Si lo desea, cambie el nombre del runbook y después haga clic en **Aceptar** para importarlo.
8. El runbook aparecerá en la pestaña **Runbooks** para la cuenta de Automation.

### <a name="adding-a-powershell-runbook-to-the-gallery"></a>Adición de un runbook de PowerShell a la galería

Microsoft recomienda agregar a la Galería de PowerShell aquellos runbooks que piense que podrían ser útiles para otros clientes. La Galería de PowerShell acepta módulos y scripts de PowerShell. Puede agregar un runbook [cargándolo en la Galería de PowerShell](/powershell/scripting/gallery/how-to/publishing-packages/publishing-a-package).

> [!NOTE]
> Los runbooks gráficos no se admiten en la Galería de PowerShell.

## <a name="modules-in-powershell-gallery"></a>Módulos en la Galería de PowerShell

Los módulos de PowerShell contienen cmdlets que puede usar en sus runbooks; los módulos existentes que se pueden instalar en Azure Automation están disponibles en la [Galería de PowerShell](https://www.powershellgallery.com). Puede iniciar esta galería desde Azure Portal e instalarlos directamente en Azure Automation o puede descargarlos e instalarlos de forma manual.

### <a name="to-import-a-module-from-the-automation-module-gallery-with-the-azure-portal"></a>Para importar un módulo de la Galería de módulos de automatización con el Portal de Azure

1. En Azure Portal, abra su cuenta de Automation.
2. Seleccione **Módulos** en **Recursos compartidos** para abrir la lista de módulos.
3. Haga clic en **Examinar la galería** en la parte superior de la página.

   ![Galería de módulos](media/automation-runbook-gallery/modules-blade.png)

4. En la página **Examinar galería**, puede buscar por los campos siguientes:

   * Nombre del módulo
   * Etiquetas
   * Autor
   * Nombre de cmdlet/recurso de DSC

5. Busque un módulo que le interese y selecciónelo para ver sus detalles.

   Al explorar un módulo específico, puede ver más información. Esta información incluye un vínculo a la Galería de PowerShell, las dependencias necesarias y todos los cmdlets o recursos de DSC que contiene el módulo.

   ![Detalles del módulo de PowerShell](media/automation-runbook-gallery/gallery-item-details-blade.png)

6. Para instalar el módulo directamente en Azure Automation, haga clic en el botón **Importar** .
7. Al hacer clic en el botón Importar, en el panel **Importar**, verá el nombre del módulo que se va a importar. Si están instaladas todas las dependencias, el botón **Aceptar** estará activo. Si faltan dependencias, debe importarlas antes de importar este módulo.
8. En la página **Importar**, haga clic en **Aceptar** para importar el módulo. Cuando Azure Automation importa un módulo en la cuenta, extrae metadatos sobre el módulo y los cmdlets. Esta acción puede tardar un par de minutos, ya que deben extraerse todas las actividades.
9. Recibirá una notificación inicial indicando que el módulo se está implementado y otra notificación cuando se haya completado.
10. Una vez importado el módulo, puede ver las actividades disponibles. Puede usar sus recursos en los runbooks y la configuración de estado deseada.

> [!NOTE]
> Los módulos que solo admiten PowerShell Core no son compatibles con Azure Automation y no pueden importarse en Azure Portal, ni implementarse directamente desde la Galería de PowerShell.

## <a name="python-runbooks"></a>Runbooks de Python

Los runbooks de Python están disponibles en la [galería del Centro de scripts](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B1%5D.Type=ProgrammingLanguage&f%5B1%5D.Value=Python&f%5B1%5D.Text=Python&sortBy=Date&username=). Puede contribuir con runbooks de Python para la galería del Centro de scripts haciendo clic en **Upload a contribution** (Cargar una contribución). Al hacerlo, asegúrese de agregar la etiqueta **Python** al cargar su contribución.

> [!NOTE]
> Para cargar contenido al [Centro de scripts](https://gallery.technet.microsoft.com/scriptcenter) se requiere un mínimo de 100 puntos.

## <a name="requesting-a-runbook-or-module"></a>Solicitud de un runbook o módulo

Puede enviar solicitudes a [Voz de usuario](https://feedback.azure.com/forums/246290-azure-automation/).  Si necesita ayuda para escribir un runbook o se plantea preguntas acerca de PowerShell, publique una pregunta en nuestro [foro](https://social.msdn.microsoft.com/Forums/windowsazure/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc).

## <a name="common-solutions-available-in-the-runbook-gallery"></a>Soluciones comunes disponibles en la galería de runbooks

La lista siguiente contiene algunos runbooks que proporcionan soluciones para escenarios comunes. Para obtener una lista completa de los runbooks creados por el equipo de Azure Automation, consulte el [perfil de AzureAutomationTeam](https://www.powershellgallery.com/profiles/AzureAutomationTeam).

* [Update-ModulesInAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/): importa la versión más reciente en la Galería de PowerShell de todos los módulos de una cuenta de Automation.
* [Enable-AzureDiagnostics](https://www.powershellgallery.com/packages/Enable-AzureDiagnostics/): este script configura Azure Diagnostics y Log Analytics para recibir registros de Azure Automation que contengan el estado y los flujos de trabajo.
* [Copy-ItemFromAzureVM](https://www.powershellgallery.com/packages/Copy-ItemFromAzureVM/): este runbook copia un archivo remoto de una máquina virtual Windows de Azure.
* [Copy-ItemFromAzureVM](https://www.powershellgallery.com/packages/Copy-ItemToAzureVM/): este runbook copia un archivo local en una máquina virtual de Azure.

## <a name="next-steps"></a>Pasos siguientes

* Para empezar a trabajar con runbooks, consulte el artículo sobre la [Administración de un runbook en Azure Automation](manage-runbooks.md)
* Para comprender las diferencias entre PowerShell y los flujos de trabajo de PowerShell para runbooks, consulte [Aprendizaje del flujo de trabajo de Windows PowerShell](automation-powershell-workflow.md)
* Para obtener más información sobre PowerShell, incluidos los módulos de referencia de lenguaje y aprendizaje, consulte la [documentación de PowerShell](https://docs.microsoft.com/powershell/scripting/overview).
