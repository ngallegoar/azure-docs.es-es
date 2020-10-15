---
title: 'Tutorial: Creación de una directiva WAF para Azure CDN con Azure Portal'
description: En este tutorial aprenderá a crear una directiva de firewall de aplicaciones web (WAF) en Azure CDN mediante Azure Portal.
author: vhorne
ms.service: web-application-firewall
services: web-application-firewall
ms.topic: tutorial
ms.date: 09/16/2020
ms.author: victorh
ms.openlocfilehash: c5505b9437a4bd8dced6a090817b17d5e29374f2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91327945"
---
# <a name="tutorial-create-a-waf-policy-on-azure-cdn-using-the-azure-portal"></a>Tutorial: Creación de una directiva WAF para Azure CDN con Azure Portal

Este tutorial le muestra cómo crear una directiva básica de firewall de aplicaciones web (WAF) de Azure y cómo aplicarla a un punto de conexión en Azure Content Delivery Network (CDN).

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una directiva WAF
> * Asociarla a un punto de conexión de CDN. Solo puede asociar una directiva de WAF a los puntos de conexión que se hospedan en la SKU **Azure CDN estándar de Microsoft**.
> * Configurar las reglas de WAF

## <a name="prerequisites"></a>Prerrequisitos

Cree un perfil de Azure CDN y un punto de conexión siguiendo las instrucciones de [Inicio rápido: Creación de un perfil y un punto de conexión de Azure CDN](../../cdn/cdn-create-new-endpoint.md). 

## <a name="create-a-web-application-firewall-policy"></a>Creación de una directiva de firewall de aplicaciones web

En primer lugar, cree una directiva básica WAF con un conjunto de reglas predeterminado (DRS) administrado con el portal.

1. En la parte superior izquierda de la pantalla, seleccione **Crear un recurso**> busque **WAF**> seleccione **Firewall de aplicaciones web** > seleccione **Crear**.
2. En la pestaña **Datos básicos** de la página **Crear una directiva WAF**, escriba o seleccione la siguiente información, acepte los valores predeterminados para el resto de la configuración y, luego, seleccione **Revisar y crear**:

    | Configuración                 | Value                                              |
    | ---                     | ---                                                |
    | Directiva de            |Seleccione Azure CDN (versión preliminar).|
    | Subscription            |Seleccione el nombre de la suscripción a Front Door.|
    | Resource group          |Seleccione el nombre del grupo de recursos de Front Door.|
    | Nombre de la directiva             |Escriba un nombre único para la directiva WAF.|

   ![Creación de una directiva WAF](../media/waf-cdn-create-portal/basic.png)

3. En la pestaña **Asociación** de la página **Crear una directiva WAF**, seleccione **Agregar punto de conexión de CDN**, escriba la siguiente configuración y, a continuación, seleccione **Agregar**:

    | Configuración                 | Value                                              |
    | ---                     | ---                                                |
    | Perfil de CDN              | Seleccione el nombre del perfil de CDN.|
    | Punto de conexión           | Seleccione el nombre del punto de conexión y, después, **Agregar**.|
    
    > [!NOTE]
    > Si el punto de conexión está asociado a una directiva WAF, se muestra atenuada. Debe quitar primero el punto de conexión de la directiva asociada y, a continuación, volver a asociarlo a una nueva directiva WAF.
1. Seleccione **Revisar y crear** y, luego, **Crear**.

## <a name="configure-web-application-firewall-policy-optional"></a>Configuración de la directiva de firewall de aplicaciones web (opcional)

### <a name="change-mode"></a>Cambio del modo

Cuando se crea una directiva WAF, de forma predeterminada está en modo *Detección*. En el modo de *detección*, WAF no bloquea ninguna solicitud. En su lugar, las reglas de WAF coincidentes se registran en los registros de WAF.

Para ver WAF en acción, puede cambiar la configuración del modo de *Detección* a *Prevención*. En el modo *Prevención*, las solicitudes que coinciden con las reglas que se definen en el conjunto de reglas predeterminado (DRS) se bloquean y se registran en los registros de WAF.

 ![Cambio del modo de directiva WAF](../media/waf-cdn-create-portal/policy.png)

### <a name="custom-rules"></a>Reglas personalizadas

Para crear una regla personalizada, seleccione **Agregar regla personalizada** debajo de la sección **Reglas personalizadas**. Se abrirá la página Configuración de reglas personalizadas. Existen dos tipos de reglas personalizadas: **regla de coincidencia** y regla de **límite de frecuencia**.

En la captura de pantalla siguiente se muestra una regla de coincidencia personalizada si la cadena de consulta contiene el valor **blockme**.

![Incorporación de una regla de coincidencia personalizada](../media/waf-cdn-create-portal/custommatch.png)

Las reglas de límite de frecuencia requieren dos campos adicionales: **Duración del límite de frecuencia** y **Umbral de límite de frecuencia (solicitudes)** , como se muestra en el ejemplo siguiente:

![Incorporación de una regla de límite de frecuencia](../media/waf-cdn-create-portal/customrate.png)

### <a name="default-rule-set-drs"></a>Conjunto de reglas predeterminado (DRS)

El conjunto de reglas predeterminado administrado por Azure está habilitado de forma predeterminada. Para deshabilitar una regla individual dentro de un grupo de reglas, expanda las reglas dentro de ese grupo de reglas, active la casilla delante del número de regla y seleccione **Deshabilitar** en la pestaña anterior. Para cambiar los tipos de acciones para los conjuntos de reglas individuales dentro del conjunto de reglas, active la casilla situada delante del número de regla y, a continuación, seleccione la pestaña **Cambiar acción** anterior.

 ![Cambio del conjunto de reglas de WAF](../media/waf-cdn-create-portal/managed2.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine el grupo de recursos y todos los recursos relacionados.


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre el firewall de aplicaciones web de Azure](../overview.md)
