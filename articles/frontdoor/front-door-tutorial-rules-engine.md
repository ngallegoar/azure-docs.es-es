---
title: Azure Front Door | Microsoft Docs
description: En este artículo se ofrece información general sobre Azure Front Door. Averigüe si es la elección correcta para equilibrar la carga de tráfico de usuario de la aplicación.
services: frontdoor
documentationcenter: ''
author: megan-beatty
editor: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 4/30/2020
ms.author: mebeatty
ms.openlocfilehash: 4574597c0b93f2985953bfbc815cca220ecc4f28
ms.sourcegitcommit: eaec2e7482fc05f0cac8597665bfceb94f7e390f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82515870"
---
# <a name="configure-your-rules-engine"></a>Configuración del motor de reglas 

> [!IMPORTANT]
> Esta versión preliminar pública se proporciona sin un acuerdo de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="configure-rules-engine-in-azure-portal"></a>Configuración del motor de reglas en Azure Portal 
1. Antes de crear una configuración del motor de reglas, [cree una instancia de Front Door](quickstart-create-front-door.md).

2. En el recurso de Front Door, vaya a **Configuración** y seleccione **Configuración del motor de reglas**. Haga clic en **Agregar**, asigne un nombre a la configuración y empiece a crear la primera configuración del motor de reglas. 

![buscar motor de reglas](./media/front-door-rules-engine/rules-engine-tutorial-1.png)

3. Haga clic en **Agregar regla** para crear la primera regla. A continuación, haga clic en **Agregar condición** o **Agregar acción** para definir la regla. 
    
    *Notas:*
    - Para eliminar una condición o acción de la regla, use la papelera que aparece en el lado derecho de la condición o acción específica.
    - Para crear una regla que se aplique a todo el tráfico entrante, no especifique ninguna condición. 
    - Para detener la evaluación de reglas una vez que se cumpla la primera condición de coincidencia, marque **Detener evaluación de regla**. 

![buscar motor de reglas](./media/front-door-rules-engine/rules-engine-tutorial-4.png)

4. Para determinar la prioridad de las reglas de configuración, use los botones Mover hacia arriba, Mover hacia abajo y Mover a la parte superior. La prioridad está en orden ascendente, lo que significa que la regla que aparece en primer lugar es la regla más importante. 

5. Una vez que haya creado una o varias reglas, presione **Guardar**. Esta acción crea la configuración del motor de reglas. 

6. Una vez que haya creado una o varias configuraciones, asocie una configuración del motor de reglas a una regla de enrutamiento. Aunque una sola configuración se puede aplicar a muchas reglas de enrutamiento, una regla de enrutamiento solo puede contener una configuración del motor de reglas. Para realizar la asociación, vaya a **Diseñador de Front Door** > **Reglas de enrutamiento**. Seleccione la regla de enrutamiento a la que desea agregar la configuración del motor de reglas, vaya a **Detalles de la ruta** > **Configuración del motor de reglas** y seleccione la configuración que desea asociar. 

![buscar motor de reglas](./media/front-door-rules-engine/rules-engine-tutorial-5.png)


## <a name="configure-rules-engine-in-azure-cli"></a>Configuración del motor de reglas en la CLI de Azure 

1. Si aún no lo ha hecho, instale la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Agregue la extensión "front-Door":- az extension add --name front-door. A continuación, inicie sesión y cambie a la suscripción: az account set --subscription <nombre_o_Id>. 

2. Empiece por crear un motor de reglas: este ejemplo muestra una regla con una acción basada en el encabezado y una condición de coincidencia. 

```azurecli-interactive
az network front-door rules-engine rule create -f {front_door} -g {resource_group} --rules-engine-name {rules_engine} --name {rule1} --priority 1 --action-type RequestHeader --header-action Overwrite --header-name Rewrite --header-value True --match-variable RequestFilenameExtension --operator Contains --match-values jpg png --transforms Lowercase
```

2.  Enumere todas las reglas. 

```azurecli-interactive
az network front-door rules-engine rule list -f {front_door} -g {rg} --name {rules_engine}
```

3.  Agregue una acción de invalidación del enrutamiento de reenvío. 

```azurecli-interactive
az network front-door rules-engine rule action add -f {front_door} -g {rg} --rules-engine-name {rules_engine} --name {rule1} --action-type ForwardRouteOverride --backend-pool {backend_pool_name} --caching Disabled
```

4.  Enumere todas las acciones de una regla. 

```azurecli-interactive
az network front-door rules-engine rule action list -f {front_door} -g {rg} -r {rules_engine} --name {rule1}
```

5. Vincule una configuración del motor de reglas a una regla de enrutamiento.  

```azurecli-interactive
az network front-door routing-rule update -g {rg} -f {front_door} -n {routing_rule_name} --rules-engine {rules_engine}
```

6. Desvincule el motor de reglas. 

```azurecli-interactive
az network front-door routing-rule update -g {rg} -f {front_door} -n {routing_rule_name} --remove rulesEngine # case sensitive word ‘rulesEngine’
```

Para más información, puede encontrar una lista completa de los comandos del motor de reglas de Azure Front Door [aquí](https://docs.microsoft.com/cli/azure/ext/front-door/network/front-door/rules-engine?view=azure-cli-latest).   

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre el [motor de reglas de Azure Front Door](front-door-rules-engine.md). 
- Aprenda a [crear una instancia de Front Door](quickstart-create-front-door.md).
- Más información acerca de cómo [funciona Front Door](front-door-routing-architecture.md).
- Consulte más información en la [referencia de la CLI](https://docs.microsoft.com/cli/azure/ext/front-door/network/front-door/rules-engine?view=azure-cli-latest) del motor de reglas de Azure Front Door. 
- Consulte más información en la [referencia de la CLI](https://docs.microsoft.com/powershell/module/az.frontdoor/?view=azps-3.8.0) del motor de reglas de Azure Front Door. 
