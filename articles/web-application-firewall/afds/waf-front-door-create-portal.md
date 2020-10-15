---
title: 'Tutorial: Creación de una directiva de firewall de aplicaciones web para Azure Front Door: Azure Portal'
description: En este tutorial aprenderá a crear una directiva de firewall de aplicaciones web (WAF) mediante Azure Portal.
author: vhorne
ms.service: web-application-firewall
services: web-application-firewall
ms.topic: tutorial
ms.date: 09/15/2020
ms.author: victorh
ms.openlocfilehash: 7c7ea5297276ed9a1d1f2ca8f4190997dcab57c3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90602224"
---
# <a name="tutorial-create-a-web-application-firewall-policy-on-azure-front-door-using-the-azure-portal"></a>Tutorial: Creación de una directiva de firewall de aplicaciones web en Azure Front Door mediante Azure Portal

En este tutorial se muestra cómo crear una directiva de Azure Web Application Firewall (WAF) básica y aplicarla a un host de front-end en Azure Front Door.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una directiva WAF
> * Asociarla a un host de front-end
> * Configurar las reglas de WAF

## <a name="prerequisites"></a>Prerrequisitos

Para crear un perfil de Front Door siga las instrucciones que se describen en [Inicio rápido: Creación de un perfil de Front Door](../../frontdoor/quickstart-create-front-door.md). 

## <a name="create-a-web-application-firewall-policy"></a>Creación de una directiva de firewall de aplicaciones web

En primer lugar, en el portal, cree una directiva WAF con un conjunto de reglas predeterminado (DRS) administrado. 

1. En la parte superior izquierda de la pantalla, seleccione **Crear un recurso**> busque **WAF**> seleccione **Firewall de aplicaciones Web (versión preliminar)** > seleccione  **Crear**.
2. En la pestaña **Datos básicos** de la página **Crear una directiva WAF**, escriba o seleccione la siguiente información, acepte los valores predeterminados para el resto de la configuración y, luego, seleccione **Revisar y crear**:

    | Configuración                 | Value                                              |
    | ---                     | ---                                                |
    | Subscription            |Seleccione el nombre de la suscripción a Front Door.|
    | Resource group          |Seleccione el nombre del grupo de recursos de Front Door.|
    | Nombre de la directiva             |Escriba un nombre único para la directiva WAF.|

   ![Creación de una directiva WAF](../media/waf-front-door-create-portal/basic.png)

3. En la pestaña **Asociación** de la página **Crear una directiva WAF**, seleccione **Agregar un host de front-end**, escriba la siguiente configuración y, a continuación, seleccione **Agregar**:

    | Configuración                 | Value                                              |
    | ---                     | ---                                                |
    | Front Door              | Seleccione el nombre de su perfil de Front Door.|
    | Host de front-end           | Seleccione el nombre de su host de Front Door y luego seleccione **Agregar**.|
    
    > [!NOTE]
    > Si el host de front-end está asociado a una directiva WAF, se muestra como atenuado. Debe quitar primero el host de front-end de la directiva asociada y, a continuación, volver a asociarlo a una nueva directiva WAF.
1. Seleccione **Revisar y crear** y, luego, **Crear**.

## <a name="configure-web-application-firewall-rules-optional"></a>Configuración de las reglas del firewall de aplicaciones web (opcional)

### <a name="change-mode"></a>Cambio del modo

Cuando se crea una directiva WAF, de forma predeterminada, esta está en modo **Detección**. En el modo **Detección**, WAF no bloquea las solicitudes, sino que las solicitudes que coinciden con las reglas de WAF se registran en los registros de WAF.
Para ver WAF en acción, puede cambiar la configuración del modo de **Detección** a **Prevención**. En el modo **Prevención**, las solicitudes que coinciden con las reglas que se definen en el conjunto de reglas predeterminado (DRS) se bloquean y se registran en los registros de WAF.

 ![Cambio del modo de directiva WAF](../media/waf-front-door-create-portal/policy.png)

### <a name="custom-rules"></a>Reglas personalizadas

Puede crear una regla personalizada si selecciona **Agregar regla personalizada** bajo la sección **Reglas personalizadas**. De esta forma se inicia la página de configuración de reglas personalizadas. A continuación se presenta un ejemplo de cómo configurar una regla personalizada para bloquear una solicitud si la cadena de consulta contiene **blockme**.

![Reglas personalizadas](../media/waf-front-door-create-portal/customquerystring2.png)

### <a name="default-rule-set-drs"></a>Conjunto de reglas predeterminado (DRS)

El conjunto de reglas predeterminado administrado por Azure está habilitado de forma predeterminada. Para deshabilitar una regla individual dentro de un grupo de reglas, expanda las reglas dentro de ese grupo de reglas, active la **casilla** delante del número de regla y seleccione **Deshabilitar** en la pestaña anterior. Para cambiar los tipos de acciones para los conjuntos de reglas individuales dentro del conjunto de reglas, active la casilla situada delante del número de regla y, a continuación, seleccione la pestaña **Cambiar acción** anterior.

 ![Cambio del conjunto de reglas de WAF](../media/waf-front-door-create-portal/managed2.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine el grupo de recursos y todos los recursos relacionados.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre Azure Front Door](../../frontdoor/front-door-overview.md).