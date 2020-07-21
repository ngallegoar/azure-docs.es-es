---
title: 'Inicio rápido: Creación de un presupuesto con una plantilla de Azure Resource Manager'
description: Inicio rápido que muestra cómo crear un presupuesto con una plantilla de Azure Resource Manager
author: bandersmsft
ms.author: banders
tags: azure-resource-manager
ms.service: cost-management-billing
ms.topic: quickstart
ms.date: 06/10/2020
ms.custom: subject-armqs
ms.openlocfilehash: 5bff8e6057475701a2e78835fb5a950dcb8c8fcb
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86252460"
---
# <a name="quickstart-create-a-budget-with-an-arm-template"></a>Inicio rápido: Creación de un presupuesto con una plantilla de Resource Manager

Los presupuestos en Cost Management le ayudan a planear y dirigir la presentación de cuentas de la organización. Con presupuestos, puede tener en cuenta los servicios de Azure que consume o a los que se suscribe durante un período específico. Le ayudan a informar a otros usuarios sobre sus gastos a fin de administrar de manera proactiva los costos y supervisar cómo avanza el gasto a lo largo del tiempo. Cuando se superan los umbrales presupuestarios que ha creado, se desencadenan las notificaciones. Ninguno de los recursos se ve afectado y no se detiene el consumo. Puede usar los presupuestos para comparar y realizar un seguimiento de gastos para analizar los costos. En este inicio rápido se muestra cómo crear un presupuesto con una plantilla de Azure Resource Manager.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Si su entorno cumple los requisitos previos y está familiarizado con el uso de plantillas de Resource Manager, seleccione el botón **Implementar en Azure**. La plantilla se abrirá en Azure Portal.

[![Implementación en Azure](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fcreate-budget%2Fazuredeploy.json)

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

La plantilla de Resource Manager solo admite suscripciones de Azure para Contratos Enterprise (EA). La plantilla no admite otros tipos de suscripción.

Para crear y administrar presupuestos, debe tener permiso de colaborador. Puede crear presupuestos individuales para las suscripciones de EA y los grupos de recursos. Sin embargo, no puede crear presupuestos para las cuentas de facturación de EA. En el caso de las suscripciones con contrato Enterprise de Azure, debe tener acceso de lectura para ver los presupuestos.

Una vez creado el presupuesto, se necesita al menos acceso de lectura en la cuenta de Azure para verlo.

Si tiene una suscripción nueva, no puede crear un presupuesto ni usar las características de Cost Management de inmediato. Para poder hacerlo deberán transcurrir un máximo de 48 horas.

Se admiten los siguientes permisos o ámbitos de Azure por suscripción para los presupuestos por usuario y grupo. Para más información sobre los ámbitos, consulte [Descripción y uso de ámbitos](understand-work-scopes.md).

- Propietario: puede crear, modificar o eliminar los presupuestos para una suscripción.
- Colaborador y Colaborador de Cost Management: puede crear, modificar o eliminar sus propios presupuestos. Puede modificar el importe presupuestario para los presupuestos creados por otros usuarios.
- Lector y Lector de Cost Management: puede ver los presupuestos para los que tiene permiso.

Para más información sobre cómo asignar permisos a los datos de Cost Management, consulte [Asignación del acceso a los datos de Cost Management](assign-access-acm-data.md).

## <a name="review-the-template"></a>Revisión de la plantilla

La plantilla usada en este inicio rápido forma parte de las [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/create-budget).

:::code language="json" source="~/quickstart-templates/create-budget/azuredeploy.json" range="1-146" highlight="110-139":::

En la plantilla, se define un recurso de Azure:

* [Microsoft.Consumption/budgets](/azure/templates/microsoft.consumption/budgets): Cree un presupuesto de Azure.

## <a name="deploy-the-template"></a>Implementación de la plantilla

1. Seleccione la imagen siguiente para iniciar sesión en Azure y abrir una plantilla. La plantilla crea un presupuesto.

   [![Implementación en Azure](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fcreate-budget%2Fazuredeploy.json)

2. Seleccione o escriba los siguientes valores.

   [![Plantilla de Resource Manager, creación de presupuesto, implementación del portal](./media/quick-create-budget-template/create-budget-using-template-portal.png)](./media/quick-create-budget-template/create-budget-using-template-portal.png#lightbox)

    * **Suscripción**: seleccione una suscripción de Azure.
    * **Resource group** (Grupo de recursos): seleccione **Create new** (Crear nuevo) y escriba un nombre único para el grupo de recursos; a continuación, haga clic en **OK** (Aceptar) o seleccione un grupo de recursos existente.
    * **Ubicación**: seleccione una ubicación. Por ejemplo, **Centro de EE. UU**.
    * **Budget Name** (Nombre del presupuesto): escriba un nombre para el presupuesto. Debe ser único dentro de un grupo de recursos. Solo se permiten caracteres alfanuméricos, guiones bajos y guiones.
    * **Amount** (Importe): especifique la cantidad total de costo o uso que se va a seguir con el presupuesto.
    * **Budget Category** (Categoría de presupuesto): seleccione la categoría del presupuesto, si el presupuesto realiza el seguimiento del **costo** o del **uso**.
    * **Time Grain** (Intervalo de agregación): especifique el tiempo que se incluye en un presupuesto. Los valores permitidos son Monthly (Mensual), Quarterly (Trimestral) o Annually (Anualmente). El presupuesto se restablece al final del intervalo de agregación.
    * **Start date** (Fecha de inicio): especifique la fecha de inicio con el primer día del mes en formato AAAA-MM-DD. La fecha de inicio futura no puede ser superior a de tres meses a partir de hoy. Puede especificar una fecha de inicio en el pasado con el período del intervalo de agregación.
    * **End Date** (Fecha de finalización): especifique la fecha de finalización del presupuesto en formato AAAA-MM-DD. Si no se proporciona, el valor predeterminado se establece en 10 años a partir de la fecha de inicio.
    * **Operator** (Operador): seleccione un operador de comparación. Los valores posibles son EqualTo, GreaterThan o GreaterThanOrEqualTo.
    * **Threshold** (Umbral): escriba un valor de umbral para la notificación. Se envía una notificación cuando el costo supera el umbral. Es siempre un porcentaje y debe estar entre 0 y 1000.
    * **Contact Emails** (Correos electrónicos de contacto): escriba una lista de direcciones de correo electrónico a las que enviar la notificación de presupuesto cuando se supere el umbral. El formato esperado es `["user1@domain.com","user2@domain.com"]`.
    * **Contact Roles** (Roles de contacto): escriba la lista de roles de contacto a los que se enviará la notificación de presupuesto cuando se supere el umbral. Los valores predeterminados son Owner (Propietario), Contributor (Colaborador) y Reader (Lector). El formato esperado es `["Owner","Contributor","Reader"]`.
    * **Contact Groups** (Grupos de contactos): escriba una lista de identificadores de recursos de los grupos de acciones, en forma de identificador URI completo de los recursos, a los que enviar la notificación del presupuesto cuando se supere el umbral. Acepta una matriz de cadenas. El formato esperado es `["action group resource ID1","action group resource ID2"]`. Si no desea usar grupos de acciones, escriba `[]`.
    * **Resources Filter** (Filtro de recursos): escriba una lista de filtros para los recursos. El formato esperado es `["Resource Filter Name1","Resource Filter Name2"]`. Si no desea aplicar un filtro, escriba `[]`. Si escribe un filtro de recursos, también debe escribir los valores de los **filtros de medidores**.
    * **Meters Filter** (Filtro de medidores): escriba una lista de filtros en los medidores, obligatorios para los presupuestos con la categoría de presupuesto **Usage** (Uso). El formato esperado es `["Meter Filter Name1","Meter Filter Name2"]`. Si no ha especificado un **filtro de recursos**, escriba `[]`.
    * **Acepto los términos y condiciones anteriores**: Seleccionar.

3. Seleccione **Comprar**. Cuando haya implementado correctamente el presupuesto, recibirá una notificación:

   ![Plantilla de Resource Manager, presupuesto, notificación de implementación del portal](./media/quick-create-budget-template/resource-manager-template-portal-deployment-notification.png)

Azure Portal se usa para implementar la plantilla. Además de Azure Portal, también puede usar Azure PowerShell, CLI de Azure y API REST. Para más información sobre otras plantillas de implementación, consulte [Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell](../../azure-resource-manager/templates/deploy-powershell.md).

## <a name="validate-the-deployment"></a>Validación de la implementación

Puede usar Azure Portal para comprobar que se crea el presupuesto. Para ello, vaya a **Administración de costos + facturación** > seleccione un ámbito > **Presupuestos**. O bien, utilice los siguientes scripts de la CLI de Azure o Azure PowerShell para ver el presupuesto.

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
az consumption budget list
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
Get-AzConsumptionBudget
```

---

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite un presupuesto, utilice cualquiera de los siguientes métodos para eliminarlo:

### <a name="azure-portal"></a>Azure portal

Vaya a **Administración de costos + facturación** > seleccione un ámbito de facturación > **Presupuestos** > seleccione un presupuesto > seleccione **Eliminar presupuesto**.

### <a name="command-line"></a>Línea de comandos

Para quitar el presupuesto se pueden usar la CLI de Azure o Azure PowerShell.

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
echo "Enter the budget name:" &&
read budgetName &&
az consumption budget delete --budget-name $budgetName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$budgetName = Read-Host -Prompt "Enter the budget name"
Remove-AzConsumptionBudget -Name $budgetName
Write-Host "Press [ENTER] to continue..."
```

---

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha creado una implementación de presupuesto de Azure. Para más información sobre Azure Cost Management y facturación y Azure Resource Manager, continúe con los artículos siguientes.

- Consulte la introducción a [Administración de costos + facturación](../cost-management-billing-overview.md).
- [Creación de presupuestos](tutorial-acm-create-budgets.md) en Azure Portal
- Obtenga más información sobre [Azure Resource Manager](../../azure-resource-manager/management/overview.md).
