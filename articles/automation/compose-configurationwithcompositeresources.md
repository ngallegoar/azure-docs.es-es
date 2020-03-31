---
title: Compilación de configuraciones de DSC en Azure Automation State Configuration (DSC) mediante recursos compuestos
description: Obtenga información sobre cómo crear configuraciones mediante recursos compuestos en Azure Automation State Configuration (DSC)
keywords: powershell dsc, configuración de estado deseado, powershell dsc azure, recursos compuestos
services: automation
ms.subservice: dsc
ms.date: 08/21/2018
ms.topic: conceptual
ms.openlocfilehash: e5083ec55ee0a57cd7defd466f5baf1704336320
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77370655"
---
# <a name="composing-dsc-configurations-in-azure-automation-state-configuration-dsc-using-composite-resources"></a>Compilación de configuraciones de DSC en Azure Automation State Configuration (DSC) mediante recursos compuestos

Cuando un recurso debe administrarse con más de una configuración de estado deseado (DSC), la mejor ruta es usar [recursos compuestos](/powershell/scripting/dsc/resources/authoringresourcecomposite). Un recurso compuesto es una configuración anidada y parametrizada que se usa como recurso de DSC dentro de otra configuración. Esto permite la creación de configuraciones complejas, al tiempo que permite administrar y compilar individualmente los recursos compuestos subyacentes (configuraciones parametrizadas).

Azure Automation permite la [importación y compilación de recursos compuestos](automation-dsc-compile.md).
Una vez que se han importado los recursos compuestos de su cuenta de Automation, pueden usar la experiencia **Configuración de Compose** en la página **State Configuration (DSC)** (Configuración de estado [DSC]).

## <a name="composing-a-configuration-from-composite-resources"></a>Creación de una configuración a partir de recursos compuestos

Antes de poder asignar una configuración realizada desde recursos compuestos en Azure Portal, debe componerla. Esto puede hacerse mediante **Configuración de Compose** en la página **State Configuration (DSC)** mientras se encuentra en la pestaña **Configuraciones** o **Configuraciones compiladas**.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. A la izquierda, haga clic en **Todos los recursos** y, luego, en el nombre de la cuenta de Automation.
1. En la página **Cuenta de Automation**, seleccione **State configuration (DSC)** (Configuración de estado [DSC]) en **Administración de configuración**.
1. En la página **State configuration (DSC)** , haga clic en la pestaña **Configuraciones** o **Configuraciones compiladas** y, después, haga clic en **Configuración de Compose**  en el menú en la parte superior de la página.
1. En el paso **Conceptos básicos**, proporcione el nuevo nombre de configuración (obligatorio) y haga clic en cualquier parte en la fila de cada recurso compuesto que quiera incluir en la nueva configuración, luego, haga clic en **Siguiente** o haga clic en el paso **Código fuente**. Para conocer los pasos siguientes, hemos seleccionado los recursos compuestos **PSExecutionPolicy** y **RenameAndDomainJoin**.
   ![Captura de pantalla del paso básico de la página Configuración de Compose](./media/compose-configurationwithcompositeresources/compose-configuration-basics.png)
1. El paso **Código fuente** muestra el aspecto de la configuración compuesta de los recursos compuestos seleccionados. Puede ver la combinación de todos los parámetros y cómo se pasan al recurso compuesto. Cuando haya terminado de revisar el código fuente nuevo, haga clic en **Siguiente** o haga clic en el paso **Parámetros**.
   ![Captura de pantalla del paso de código fuente de la página Configuración de Compose](./media/compose-configurationwithcompositeresources/compose-configuration-sourcecode.png)
1. En el paso **Parámetros**, el parámetro que tiene cada recurso compuesto se expone para que pueda proporcionarse. Si un parámetro tiene una descripción, se muestra junto al campo de parámetro. Si un campo tiene un parámetro de tipo **PSCredential**, la lista desplegable de configuración proporciona una lista de objetos **Credential** en la cuenta actual de Automation. También hay disponible una opción **+ Agregar una credencial**. Una vez que se hayan proporcionado todos los parámetros necesarios, haga clic en **Guardar y compilar**.
   ![Captura de pantalla del paso de parámetros de la página Configuración de Compose](./media/compose-configurationwithcompositeresources/compose-configuration-parameters.png)

Una vez que se guarde la nueva configuración, se envía para la compilación. El estado del trabajo de compilación puede verse como cualquier configuración importada. Para más información, consulte [Visualización de un trabajo de compilación](automation-dsc-getting-started.md#viewing-a-compilation-job).

Una vez que la compilación finalice correctamente, la nueva configuración aparece en la pestaña **Configuraciones compiladas**. Una vez que se vea en esta pestaña, puede asignarse a un nodo administrado mediante los pasos descritos en [Reasignación de un nodo a una configuración de nodo diferente](automation-dsc-getting-started.md#reassigning-a-node-to-a-different-node-configuration).

## <a name="next-steps"></a>Pasos siguientes

- Para empezar a usar Azure Automation State Configuration, consulte [Introducción a Azure Automation State Configuration](automation-dsc-getting-started.md)
- Para aprender a incorporar nodos, consulte [Incorporación de máquinas para administrarlas con Azure Automation State Configuration](automation-dsc-onboarding.md)
- Para obtener información acerca de la compilación de configuraciones de DSC para que poder asignarlas a los nodos de destino, consulte [Compilación de configuraciones en Azure Automation State Configuration](automation-dsc-compile.md)
- Para la referencia de cmdlets de PowerShell, consulte [cmdlets de Azure Automation State Configuration](/powershell/module/azurerm.automation/#automation)
- Para obtener información de precios, consulte [Precios de Azure Automation State Configuration](https://azure.microsoft.com/pricing/details/automation/)
- Para ver un ejemplo del uso de Azure Automation State Configuration en una canalización de implementación continua, consulte [Implementación continua mediante Azure Automation State Configuration y Chocolatey](automation-dsc-cd-chocolatey.md)
