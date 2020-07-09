---
title: Composición de configuraciones DSC
description: En este artículo se indica cómo componer configuraciones con recursos compuestos en State Configuration de Azure Automation.
keywords: powershell dsc, configuración de estado deseado, powershell dsc azure, recursos compuestos
services: automation
ms.subservice: dsc
ms.date: 08/21/2018
ms.topic: conceptual
ms.openlocfilehash: 9c5524d2847f59dda1f2c24f67e1e18f18d49b1f
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86185575"
---
# <a name="compose-dsc-configurations"></a>Composición de configuraciones DSC

Cuando debe administrar un recurso con más de una configuración de estado deseado (DSC), la mejor opción es usar [recursos compuestos](/powershell/scripting/dsc/resources/authoringresourcecomposite). Un recurso compuesto es una configuración anidada y parametrizada que se usa como recurso de DSC dentro de otra configuración. Este uso le permite crear configuraciones complejas, así como administrar y compilar individualmente los recursos compuestos subyacentes.

Azure Automation permite la [importación y compilación de recursos compuestos](automation-dsc-compile.md). Una vez que haya importado los recursos compuestos en su cuenta de Automation, podrá usar Azure Automation State Configuration a través de la característica **State Configuration (DSC)** en Azure Portal.

## <a name="compose-a-configuration"></a>Composición de una configuración

Para poder asignar una configuración realizada desde recursos compuestos en Azure Portal, debe componerla. La composición usa **Configuración de Compose** en la página State Configuration (DSC) mientras se encuentra en la pestaña **Configuraciones** o **Configuraciones compiladas**.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. A la izquierda, haga clic en **Todos los recursos** y, luego, en el nombre de la cuenta de Automation.
1. En la página Cuenta de Automation, seleccione **State configuration (DSC)** en **Administración de configuración**.
1. En la página State Configuration (DSC), haga clic en la pestaña **Configuraciones** o **Configuraciones compiladas** y, después, haga clic en **Configuración de Compose** en el menú de la parte superior de la página.
1. En el paso **Conceptos básicos**, proporcione el nuevo nombre de configuración (obligatorio) y haga clic en cualquier parte en la fila de cada recurso compuesto que quiera incluir en la nueva configuración, luego, haga clic en **Siguiente** o haga clic en el paso **Código fuente**. En los pasos siguientes, hemos seleccionado `PSExecutionPolicy` y `RenameAndDomainJoin` recursos compuestos.
   ![Captura de pantalla del paso básico de la página Configuración de Compose](./media/compose-configurationwithcompositeresources/compose-configuration-basics.png)
1. El paso **Código fuente** muestra el aspecto de la configuración compuesta de los recursos compuestos seleccionados. Puede ver la combinación de todos los parámetros y cómo se pasan al recurso compuesto. Cuando haya terminado de revisar el código fuente nuevo, haga clic en **Siguiente** o haga clic en el paso **Parámetros**.
   ![Captura de pantalla del paso de código fuente de la página Configuración de Compose](./media/compose-configurationwithcompositeresources/compose-configuration-sourcecode.png)
1. En el paso **Parámetros**, el parámetro de cada recurso compuesto se expone para que puedan proporcionarse valores. Si un parámetro tiene una descripción, se muestra junto al campo de parámetro. Si un parámetro es de tipo `PSCredential`, el menú desplegable proporciona una lista de objetos **Credential** en la cuenta de Automation actual. También hay disponible una opción **+ Agregar una credencial**. Una vez que se hayan proporcionado todos los parámetros necesarios, haga clic en **Guardar y compilar**.
   ![Captura de pantalla del paso de parámetros de la página Configuración de Compose](./media/compose-configurationwithcompositeresources/compose-configuration-parameters.png)

## <a name="submit-the-configuration-for-compilation"></a>Envío de la configuración para su compilación

Una vez que se guarde la nueva configuración, se envía para la compilación. Puede ver el estado del trabajo de compilación igual que con cualquier configuración importada. Para más información, consulte la sección sobre [visualización de un trabajo de compilación](automation-dsc-getting-started.md#view-a-compilation-job).

Una vez que la compilación finalice correctamente, la nueva configuración aparece en la pestaña **Configuraciones compiladas**. A continuación, puede asignar la configuración a un nodo administrado, siguiendo los pasos descritos en [Reasignación de un nodo a una configuración de nodo diferente](automation-dsc-getting-started.md#reassign-a-node-to-a-different-node-configuration).

## <a name="next-steps"></a>Pasos siguientes

- Para aprender a habilitar nodos, consulte el artículo sobre [habilitación de State Configuration de Azure Automation](automation-dsc-onboarding.md).
- Para aprender a compilar configuraciones de DSC para poder asignarlas a los nodos de destino, consulte [Compilación de configuraciones de DSC en Azure Automation State Configuration](automation-dsc-compile.md).
- Para ver un ejemplo del uso de State Configuration de Azure Automation en una canalización de implementación continua, consulte [Configuración de la implementación continua con Chocolatey](automation-dsc-cd-chocolatey.md).
- Para obtener información de precios, consulte [Precios de State Configuration de Azure Automation](https://azure.microsoft.com/pricing/details/automation/).
- Para ver una referencia de los cmdlets de PowerShell, consulte [Az.Automation](/powershell/module/az.automation/?view=azps-3.7.0#automation).
